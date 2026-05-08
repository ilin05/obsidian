# Unlocking the Potential of CXL for Disaggregated Memory in Cloud-Native Databases

Existing RDMA-based solutions face critical challenges such as limited bandwidth, higher latency, limited concurrency and complexity in managing cache coherency. The emergence of CXL addresses these challenges by offering low-latency, high-bandwidth connections with native cache cohenrecy and memory pooling/sharing support, significantly improving the performance of cloud databases.

Limitations of RDMA-based disaggregated memory in cloud databases:

- Data transferred between the tiered structure is at the page level (4-16 KB), even small data requests trigger the transfer of entire pages, causing significant read/write amplification.
- Maintaining a local buffer pool increases RDMA read/write overhead, and buffer pool misses lead to frequent RDMA read/write operations, reducing performance.
- The overall recovery process takes considerable time, as the disaggregated memory is not leveraged to optimize the recovery scheme and still relies on the ARIES-style recovery scheme.
- It faces challenges under high concurrency.
- Database must manage its own cache coherency mechanisms, which adds overhaed and limits performance.

CXL offers sufficient speed and supports native memory load/store instruc- tions, allowing it to directly store all buffered pages. This design eliminates the read/write amplification caused by page copying in tiered structures, saving bandwidth resources. Meanwhile, elimi-nating the local buffer avoids the local memory overhead and saves costs. 

In PolarRecv, we move the entire buffer pool, including both data and metadata, to CXL memory. With CXL’s low latency and support for load/store instructions, the database can directly operate on the buffer pool’s data and metadata without requiring changes to the database logic.

## Background and Motivation

### CXL introduction

CXL（Compute Express Link）是一种基于 PCIe 物理层的新一代高速互联标准，旨在解决高性能计算中 CPU 与设备、设备与设备之间的内存一致性问题。CXL 引入了三种核心协议 ：  

1. **CXL.io**：用于设备的发现、配置和中断管理，类似于 PCIe。
2. **CXL.cache**：允许设备（如加速器）缓存主机的内存，并由硬件维护缓存一致性。
3. **CXL.mem**：允许主机 CPU 直接访问设备上的内存，就像访问本地 DRAM 一样，支持 Load/Store 指令。

研究基于 CXL 2.0 标准，特别是利用了 **CXL Switch（交换机）** 技术。与 CXL 1.1 的点对点连接不同，CXL 2.0 支持通过交换机构建内存池（Memory Pooling），允许多个主机连接到同一个内存资源池，并根据需求动态分配内存片段 。  

**关键硬件特性：**

- **低延迟**：通过 CXL Switch 访问远端内存的延迟约为 **500-600ns**。虽然高于本地 DRAM（~100ns），但相比 RDMA（~5000ns）有着数量级的优势 。  
- **内存语义（Memory Semantics）**：这是 CXL 区别于 RDMA 的根本。CPU 可以直接向 CXL 内存地址发起 `MOV`、`LOAD`、`STORE` 指令，利用 CPU 的 Load/Store 单元和各级缓存（L1/L2/L3）来处理数据，而无需经过网卡、驱动或复杂的 DMA 操作。

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 17.27.03.png" alt="截屏2026-01-14 17.27.03" style="zoom: 67%;" />

##### Data transfer of RDMA *vs* CXL

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 17.40.04.png" alt="截屏2026-01-14 17.40.04" style="zoom:50%;" />

For small data sizes, CXL demonstrates significant advantages over RDMA. However, as the data size increases to 16KB, CXL’s latency rises more substantially than RDMA’s. RDMA’s latency is less sensitive to data size due to its fixed overhead, while CXL latency grows with data size due to limited CPU load/store buffer depth. 

consider load/store instructions for direct memory accesses and eliminate the local buffer completely, instead of copying data from the remote memory to the local buffer. Directly accessing data on CXL memory is much faster than with RDMA, making it a feasible option to run databases directly on CXL memory. 

##### Database performance on CXL

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 17.44.08.png" alt="截屏2026-01-14 17.44.08" style="zoom:67%;" />

**RDMA 分层内存结构 (Tiered Memory)**：

- **设计**：采用“本地 DRAM + 远端 RDMA 内存”的双层架构。数据库必须在本地计算节点维护一个较小的缓冲池（Local Buffer Pool, LBP）来存放热数据，而将冷数据置换到远端内存 。
- **痛点**：不仅需要管理复杂的页面换入换出逻辑，计算节点还必须为此支付昂贵的本地内存成本（DRAM 成本通常占服务器成本的一半左右）。

**CXL 统一内存结构 (Unified Memory)**：

- **设计**：PolarCXLMem 摒弃了本地数据缓冲池，直接将数据库的 Buffer Pool 完全构建在远端 CXL 内存池上。CPU 将 CXL 内存设备直接映射到物理地址空间，视为“统一”的内存资源 。
- **优势**：计算节点只需保留极少量内存用于线程上下文等，大幅降低了硬件成本，且简化了数据库内存管理的复杂度 。

## Design and Implementation 

### *PolarCXLMem*

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 17.46.31.png" alt="截屏2026-01-14 17.46.31" style="zoom:67%;" />

The key difference between *PolarCXLMem* and RDMA-based disaggregated memory is that the database constructs its buffer pool, including both data pages and metadata, directly on CXL memory. Local DRAM is reserved for variables and other transaction engine-related data structures.

##### CXL Memory Allocation

1. **设备暴露**：BIOS 将 CXL 内存识别为特殊的系统物理地址范围，OS 将其配置为 `/dev/dax` 设备
2. **内存映射**：PolarDB 进程启动时，通过 `mmap` 系统调用将 CXL 设备映射到用户空间的虚拟地址空间
3. **自定义分配器**：PolarDB 内部实现了一个针对 CXL 优化的内存分配器，将大块 CXL 内存切分为 16KB 的数据块（Block），用于存放数据页和元数据（Page Headers）

Manager为每个节点分配内存，确保没有任何两个节点访问重叠的 CXL 内存区域。当一个节点请求 CXL 内存时，它通过 RPC（远程过程调用）与管理者通信，指定所需的内存大小。管理者随后返回已分配内存的偏移量（Offset），允许该节点从该偏移量开始使用内存。由于用于缓冲池的 CXL 内存仅在数据库启动期间分配一次，因此内存分配的开销在运行时没有任何影响。

##### CXL-based buffer pool

During database startup, the instance calculates the memory required for the buffer pool and allocates CXL memory from the CXL memory manager.

When the transaction engine requests a page, if the page is in the in-use list, the data pointer is returned directly to the transaction engine. If not, a block from the free list is assigned, an I/O operation is performed to load the page into memory.

##### Avoiding Tiered Memory

The buffer pool directly on CXL memory provides nearly the same performance as using local DRAM. Therefore, a tiered memory structure is unnecessary with CXL-based disaggregated memory.

##### Physical Topology

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 18.57.57.png" alt="截屏2026-01-14 18.57.57" style="zoom:80%;" />

### Instant Recovery on *PolarCXLMem*

PolarCXLMem 的架构中，CXL 内存池位于独立的物理机箱或通过独立电源供电的扩展柜中，其生命周期独立于计算节点。当计算节点因 OS 崩溃或断电重启时，CXL 内存中的数据依然完好无损 。

###### 传统恢复的痛点（ARIES 算法）

在 ARIES 恢复协议中，数据库崩溃意味着内存数据的丢失。重启后必须执行以下步骤：

1. **加载 Checkpoint**：读取最近的检查点。
2. **重放 Redo Log**：从磁盘读取大量的 WAL 日志，按顺序重放，将数据页恢复到崩溃前的状态。
3. **Undo 未提交事务**：回滚未完成的事务。 这一过程是 I/O 密集型（读取日志和数据页）和 CPU 密集型（解析和应用日志）的，且随着数据量的增加线性增长。

Upon committing a transaction, the database writes these redo logs to persistent storage without flushing the modified page itself. After a crash, the modified pages in the buffer are lost. During recovery, the system scans redo logs from the latest checkpoint onward, reading corresponding pages from storage and applying logs to rebuild pages in the buffer. 

*PolarCXLMem* allows the database to operate directly on CXL memory. After a crash, CXL momory retains all the latest updates, enabling us to initialize the buffer pool structure directly from CXL memory.

There are additional issues that must be addressed to enable instant recovery:

1. The LRU list may be inconsistent after a crash if the crash occurred during the movement of an LRU node.
2.  If a crash happens during structure modification operations (SMOs) in the B-tree, the B-tree structure may be inconsistent. The system must restore the B-tree to a consistent state during recovery.
3. When a crash occurs during a page update, the page may be left in a partial state. Page atomicity must be guaranteed. 
4. Since the redo log buffer still uses local DRAM, any logs not flushed to storage at the time of the crash will be lost.

##### *PolarRecv* Design

1. **元数据持久化**：PolarDB 不仅将数据页存放在 CXL 内存中，还将所有关键的 **Buffer Pool 元数据**（如脏页标记、LSN、LRU 链表指针、锁状态）也存放在 CXL 内存中。
2. **崩溃后接管（Takeover）**：数据库重启时，进程重新 `mmap` 之前的 CXL 内存区域。
3. **跳过 Redo 重放**：由于 CXL 内存中保存了崩溃一瞬间的完整内存状态（包含所有已提交和未提交的脏页），数据库 **不需要** 从磁盘重放 Redo Log 来重建缓冲池。
4. **一致性修复**：
   - 系统仅需扫描 CXL 内存中的元数据。
   - 检查是否存在 **“正在写入中”** 的页面（In-flight Writes）。如果崩溃发生在 CPU 将 Cache Line 写回 CXL 内存的过程中，可能会导致页面数据损坏（部分写）。
   - PolarRecv 利用页面 checksum 或特定的 Dirty Bit 标记识别这些损坏页面。**只有** 这些极少数的损坏页面需要通过 Redo Log 进行恢复。
   - 对于绝大多数完好的页面，直接复用。
5. **热启动（Warm Start）**：恢复完成后，Buffer Pool 已经是“热”的（数据都在内存中），数据库可以立即以全速性能提供服务，完全没有传统架构重启后的“缓存预热”过程 。  

##### CXL-based data sharing on *PolarCXLMem*

<img src="/Users/qinjianwu/Library/Application Support/typora-user-images/截屏2026-01-14 19.45.42.png" alt="截屏2026-01-14 19.45.42" style="zoom:67%;" />

PolarCXLMem 利用 CXL 的字节寻址特性，将同步粒度从 **页面（Page, 16KB）** 降低到了 **缓存行（Cache Line, 64B）**

1. **基于 CXL 的锁管理**：锁状态直接存储在 CXL 内存的元数据区。节点通过原子指令（Atomic Instructions，如 CAS）直接在 CXL 内存上竞争锁，无需经过中心化的 GLM 服务器，大幅降低了锁开销。
2. **CLFLUSH 强制刷脏**：当节点 A 修改完数据释放锁时，它执行 `CLFLUSH`（Cache Line Flush）指令。
   - 这会强制 CPU 将 L1/L2 Cache 中被修改的 **那几行** 脏数据立即写回 CXL 内存。
   - **优势**：仅传输 64 字节的倍数，而非 16KB，几乎消除了写放大。
3. **版本号失效机制**：
   - 每个页面在 CXL 元数据中维护一个 `Version` 字段。
   - 节点 A 写回数据后，原子递增 `Version`。
   - 节点 B 在读取该页面前，先检查本地缓存的 Version 与 CXL 中的 Version 是否一致。如果不一致，则指令 CPU 使得本地对应的 Cache Line 失效，重新从 CXL 读取最新数据。
   - 这一检查过程极快（纳秒级），且避免了广播式的失效消息风暴 