# TurboQuant

Address both mean-squared error (MSE) and inner product distortion, overcoming limitations of existing methods that fail to achieve optimal distortion rates.

TurboQuant achieves this by randomly rotating input vectors, inducing a concentrated Beta distribution on coordinates, and leveraging the near-independence property of distinct corrdinates in high dimensions to simply apply optimal scalar quantizers per each coordinate.

We propose a two-stage approach: applying an MSE quantizer followed by a 1-bit Quantized JL transform on the residual, resuting in an unbiased inner product quantizer.

**1. 均方误差 (Mean-Squared Error, MSE)** MSE 用于衡量原始高维向量在经过“量化-反量化”过程重建后，所产生的几何结构失真程度 。具体来说，它是指原始向量 与重建向量之间欧几里得距离平方的期望值 。其数学定义为：
$$
D_{mse} := \mathbb{E}_Q[||x - Q^{-1}(Q(x))||_2^2]
$$
MSE 越小，说明重建后的向量在欧几里得空间位置上越接近原始向量。

**2. 内积失真 (Inner product distortion)** 内积失真用于衡量量化操作对两个向量之间内积计算结果准确性的影响 。这在极度依赖内积运算的场景（如大型语言模型的键值缓存和向量数据库的近邻检索）中至关重要 。它的数学定义是，任意查询向量与原始向量的真实内积，同查询向量与重建向量的近似内积之间差值平方的期望 ： 
$$
D_{prod} := \mathbb{E}_Q[|\langle y,x \rangle - \langle y, Q^{-1}(Q(x)) \rangle|^2]
$$
 内积失真越小，说明使用量化后的向量进行相似度检索或注意力机制计算时的误差越小。此外，优秀的内积量化器通常还被要求具备无偏性，即近似内积的期望完全等于真实的内积 。

**1. TurboQuant-MSE（针对均方误差优化的流程）**

- **量化过程 (Quantization)：**
  1. **随机旋转**：首先，算法生成一个随机旋转矩阵，将其与输入的 $d$ 维高维向量相乘。这一步非常关键，它使得旋转后向量的每一个坐标在高维空间中近似独立，且服从特定的Beta分布（在大维度下趋近正态分布）。
  2. **独立标量量化**：由于各个坐标已经近似独立，算法可以忽略坐标间的相关性，对每个坐标独立进行处理。它将每个坐标的值与预先计算好的最优标量量化质心（通过求解一维连续K-Means或Lloyd-Max算法得出）进行比对，找到距离最近的质心，并记录下该质心的**索引值** 。最终输出的量化结果就是这一组索引。
- **重建/反量化过程 (Dequantization)：**
  1. **查表还原**：根据记录的索引值，去预先存好的质心码本中查找对应的具体数值，从而拼凑出一个近似的“旋转后向量”。
  2. **逆向旋转**：将这个近似向量乘以之前那个随机旋转矩阵的转置矩阵，把向量在几何空间中“转”回原来的坐标系，这就得到了最终的重建向量。

**2. TurboQuant-Prod（针对内积优化的两阶段流程）**

因为仅用 MSE 优化的量化器在计算内积时会产生偏差，TurboQuant-Prod 采用了一种基于残差的“两阶段”策略 ：

- **量化过程 (Quantization)：**
  1. **第一阶段（MSE基础量化）**：用总预算比特数减去1（即 $b-1$ 比特），对原始向量跑一遍上述的 TurboQuant-MSE 量化流程，得到基础量化索引 。
  2. **提取残差**：将原始向量减去第一阶段的 MSE 重建向量，得到一个误差极小的“残差向量” 。
  3. **第二阶段（残差QJL量化）**：使用1比特的 QJL（Quantized Johnson-Lindenstrauss）变换对该残差向量进行量化。具体操作是提取残差映射后的符号（+1 或 -1），并计算记录下残差向量的整体标量长度（L2范数） 。最终的量化结果包含：MSE索引、QJL的符号向量以及残差的标量长度。
- **重建/反量化过程 (Dequantization)：**
  1. **还原基础向量**：利用存储的 MSE 索引，通过 TurboQuant-MSE 的反量化流程重建出基础近似向量 。
  2. **还原残差向量**：利用存储的符号向量和残差长度，通过 QJL 的逆映射公式重建出近似的残差向量 。
  3. **结果合并**：将“基础近似向量”与“近似残差向量”相加。数学上可以证明，这样组合出来的重建向量在与任何查询向量计算内积时，其结果完全无偏，且方差极小 。

### INTRO

As LLM capabilities depend heavily on their model size and context length, serving them requires substantial memory demands and increased inference latency. This latency is primarily attributed to communication bottlenecks between HBM and SRAM on accelerators, or across distributed clusters. By compressing or quantizing model weights and activations, we can effectively mitigate these bottlenecks, resulting in significant reductions in inference costs. 

Existing VQ algorithms present a trade-off: either they lack accelerator(vectorization) compatibility and exhibit slow computation, or they suffer from suboptimal distortion bounds relative to bit-width.

First, we develop a vector quantizer with optimal distortion rate in terms of mean-squared error. Subsequentially, we apply a 1-bit quantizer to the residual, resulting in an unbiased and low distortion inner product quantizer. Our MSE-optimal quantizer starts by randomly rotating d-dimensional input vectors. Observing the key fact that each coordinate in the rotated vectors follows a Beta distribution, we design optimal Lloyd-Max quantizer for each coordinate by solving a continuous k-means problem. This method gives optimal MSE distortion bound and minimizes the L2 norm of the residual.

To obtain an unbiased and low-distortion quantizer for inner products, we compose our quantizer with the recently developed Quantized Johnson-Lindenstrauss transform, which quantizes each corrdinate of the residual vector to a single bit.

##### MSE-quantizer: the first stage of TurboQuant

The first stage of TurboQuant focuses on the simpler problem of reconstruction. Given an original vector $x$, the goal is to compress it using $b$ bits per coordinate and then reconstruct an approximation $\tilde{x}$ that is as close as possible to $x$ in mean-squared error. This stage does not yet try to make inner products unbiased. Its job is to produce a strong geometric approximation and, just as importantly, to make the later residual small.

TurboQuant begins by applying a random rotation:

$$
y := \Pi x
$$

Here, $x$ is the original vector, $\Pi$ is a random rotation matrix, and $y$ is the rotated vector. The point of this rotation is to "spread out" the structure of the original vector so that no coordinate is too special. After rotation, each coordinate of $y$ follows the same distribution, and in high dimensions different coordinates become nearly independent. This turns a hard high-dimensional vector quantization problem into many easier one-dimensional quantization problems.

More precisely, if $x$ lies on the unit sphere, then each rotated coordinate behaves like a random variable with density

$$
f_X(t) = \frac{\Gamma(d/2)}{\sqrt{\pi}\,\Gamma((d-1)/2)}(1-t^2)^{(d-3)/2}, \quad t \in [-1,1]
$$

Here, $d$ is the vector dimension, and $\Gamma(\cdot)$ is the Gamma function. The important intuition is not the exact formula itself, but what it implies: every rotated coordinate has the same well-behaved distribution, and in high dimension this distribution is close to a normal distribution. That is why TurboQuant can quantize each coordinate independently with an optimal scalar quantizer.

For a bit-width of $b$ bits per coordinate, the scalar quantizer has $2^b$ centroids, written as $c_1, c_2, \dots, c_{2^b}$. The paper defines the optimal scalar quantization cost as

$$
C(f_X, b)
:=
\min_{-1 \le c_1 \le \cdots \le c_{2^b} \le 1}
\sum_{i=1}^{2^b}
\int_{(c_{i-1}+c_i)/2}^{(c_i+c_{i+1})/2}
|t-c_i|^2 f_X(t)\, dt
$$

This is a one-dimensional continuous k-means, or Lloyd-Max, problem. The centroids $c_i$ are chosen so that each rotated coordinate is approximated as accurately as possible in squared error. The interval boundaries are the midpoints between neighboring centroids, so each scalar value is assigned to its nearest centroid.

Once these centroids are precomputed, the actual quantization step is simple. For the $j$-th coordinate of the rotated vector, TurboQuant stores the index of the nearest centroid:

$$
\mathrm{idx}_j := \arg\min_{k \in [2^b]} |y_j - c_k|
$$

Here, $y_j$ is the $j$-th coordinate of the rotated vector, and $\mathrm{idx}_j$ is the stored $b$-bit code for that coordinate. To reconstruct the rotated vector, the algorithm looks up the centroid value for each stored index:

$$
\tilde{y}_j := c_{\mathrm{idx}_j}
$$

Then it rotates the quantized vector back into the original coordinate system:

$$
\tilde{x} := \Pi^\top \tilde{y}
$$

Here, $\Pi^\top$ is the transpose of the rotation matrix, which is also its inverse because $\Pi$ is orthogonal. So the MSE stage can be summarized as:

- rotate the vector into a random basis
- quantize each coordinate independently using an optimal scalar codebook
- reconstruct the rotated coordinates from their centroid indices
- rotate the result back

The paper's formal guarantee for this stage is that the reconstruction error decays exponentially with the bit-width:

$$
D_{\mathrm{mse}} \le \frac{\sqrt{3\pi}}{2}\cdot 4^{-b}
$$

For small bit-widths, the paper also reports the finer-grained values

$$
D_{\mathrm{mse}} \approx 0.36,\; 0.117,\; 0.03,\; 0.009
\quad \text{for } b = 1,2,3,4.
$$

This is why TurboQuant uses the MSE stage first. It gives a strong reconstruction of the original vector and makes the remaining residual error small. The next stage, QJL, does not need to quantize the whole vector anymore; it only needs to correct the remaining inner-product error on that small residual.

##### QJL: 1-bit inner product quantization

The reason TurboQuant needs a QJL stage is that a quantizer optimized only for mean-squared error (MSE) can reconstruct the vector well in Euclidean distance, but still produce biased inner products. In other words, a small reconstruction error does not automatically imply that the approximate dot product with a query vector will be correct on average. TurboQuant therefore splits the job into two parts: first approximate the vector itself well, then correct the remaining inner-product error.

Let $x$ denote the original vector, and let $x_{\text{mse}}$ denote the approximation produced by the first-stage MSE quantizer. The leftover error is the residual vector:

$$
r := x - x_{\text{mse}}
$$

This residual is the part of the vector that the first stage failed to capture. Instead of applying another ordinary quantizer to $r$, TurboQuant uses QJL because QJL is designed to preserve inner products rather than coordinate-wise reconstruction.

QJL starts with a random Gaussian matrix $S$. Multiplying a vector by $S$ means projecting it into a new random coordinate system. For the residual vector $r$, the QJL encoding keeps only the sign of each projected coordinate:

$$
Q_{\text{qjl}}(r) := \operatorname{sign}(Sr)
$$

Here, $Sr$ means "multiply the residual vector $r$ by the random matrix $S$", and $\operatorname{sign}(Sr)$ means we keep only whether each coordinate of the projected vector is positive or negative. So QJL is a 1-bit quantizer because each projected coordinate is stored as just one bit of sign information.

To turn these sign bits back into a usable estimate, the paper defines the following dequantization-style proxy:

$$
Q_{\text{qjl}}^{-1}(z) := \sqrt{\frac{\pi}{2d}}\, S^\top z
$$

Here, $S^\top$ is the transpose of the random matrix $S$, and $d$ is the original vector dimension. This expression should not be read as "perfectly reconstruct the residual vector." Instead, it is a convenient proxy that allows inner products to be estimated in an unbiased way.

The key identity behind TurboQuant-Prod is that the original inner product can be decomposed into two pieces:

$$
\langle y, x \rangle = \langle y, x_{\text{mse}} \rangle + \langle y, r \rangle
$$

Here, $\langle y, x \rangle$ means the inner product between the query vector $y$ and the data vector $x$. The first term, $\langle y, x_{\text{mse}} \rangle$, is handled by the MSE stage. The second term, $\langle y, r \rangle$, is the remaining correction term, and QJL is used to estimate it from the 1-bit encoding of the residual.

So the final TurboQuant inner-product estimator can be viewed as:

$$
\widehat{\langle y, x \rangle}
=
\langle y, x_{\text{mse}} \rangle
+
\left\langle y, Q_{\text{qjl}}^{-1}(Q_{\text{qjl}}(r)) \right\rangle
$$

This is the main intuition behind the two-stage design. The first stage makes the residual small by minimizing MSE, and the second stage applies QJL only to that small residual. As a result, TurboQuant gets the benefit of strong reconstruction from the MSE quantizer while using QJL to make the remaining inner-product estimate unbiased in the sense intended by the paper.

### Experiments

This section focuses only on the paper's headline experimental results. The goal is not to reproduce the entire evaluation section, but to record the key evidence for TurboQuant's two main claims: it works well for KV-cache compression in long-context generation, and it is also competitive for nearest-neighbor retrieval.

#### 1. KV-cache quantization

The paper evaluates TurboQuant on LongBench / LongBench-E using long-context generation models including Llama-3.1-8B-Instruct and Ministral-7B-Instruct. The main comparison is against KIVI and PolarQuant. TurboQuant is evaluated at effective 2.5-bit and 3.5-bit precision, where the non-integer bitwidth comes from assigning more bits to outlier channels and fewer bits to regular channels.

| Model | Method | KV Size | Average Score |
| --- | --- | ---: | ---: |
| Llama-3.1-8B-Instruct | Full Cache | 16 | 50.06 |
| Llama-3.1-8B-Instruct | KIVI | 3 | 48.50 |
| Llama-3.1-8B-Instruct | KIVI | 5 | 50.16 |
| Llama-3.1-8B-Instruct | PolarQuant | 3.9 | 49.78 |
| Llama-3.1-8B-Instruct | TurboQuant | 2.5 | 49.44 |
| Llama-3.1-8B-Instruct | TurboQuant | 3.5 | 50.06 |
| Ministral-7B-Instruct | Full Cache | 16 | 49.89 |
| Ministral-7B-Instruct | TurboQuant | 2.5 | 49.62 |

The paper's interpretation is that TurboQuant remains competitive with full precision even at very aggressive KV compression ratios.

![Figure 4 and LongBench tables](assets/turboquant-figure-4.png)

Caption: Figure 4 shows the paper's "Needle-In-A-Haystack" result, where TurboQuant matches the full-precision baseline visually while several compressed baselines degrade more clearly.

![Table 1 and Table 2](assets/turboquant-tables-1-2.png)

Caption: Page 20 contains the headline LongBench table and the quantization-time comparison table used by the paper to support both quality and efficiency claims.

- On Llama-3.1-8B-Instruct, the paper reports that TurboQuant at 3.5 bits reaches the same average score as the 16-bit full-cache baseline.
- Even at 2.5 bits, the reported average remains close to full precision on both listed models.
- The paper explicitly frames this as strong compression with little quality loss, rather than just a small efficiency tweak.

#### 2. Nearest-neighbor retrieval

For retrieval, the paper evaluates TurboQuant on DBpedia-based OpenAI embedding datasets with dimensions 1536 and 3072, and also on GloVe with dimension 200. The comparison is against Product Quantization (PQ) and RaBitQ. The reported metrics focus on recall at top-k together with quantization time.

| Approach | d = 200 | d = 1536 | d = 3072 |
| --- | ---: | ---: | ---: |
| Product Quantization | 37.04 s | 239.75 s | 494.42 s |
| RaBitQ | 597.25 s | 2267.59 s | 3957.19 s |
| TurboQuant | 0.0007 s | 0.0013 s | 0.0021 s |

The recall curves in Figure 5 are used by the paper to argue that TurboQuant also gives stronger recall than PQ and RaBitQ under the same bit budget.

![Figure 5 recall curves](assets/turboquant-figure-5.png)

Caption: Figure 5 shows the reported recall@1@k curves across three datasets. In the paper's plots, TurboQuant stays above PQ and RaBitQ for both 2-bit and 4-bit settings.

- The retrieval story is not only about compression quality, but also about quantization/indexing cost.
- Table 2 is especially striking because TurboQuant's reported quantization time is effectively zero compared with PQ and RaBitQ.
- The paper uses Figure 5 to support the claim that this speedup does not come from giving up recall.

#### 3. Main takeaways

- For KV-cache compression, the paper's headline claim is that TurboQuant stays near full-precision quality even at low effective bitwidths such as 2.5 and 3.5 bits.
- For retrieval, the paper claims a strong combination of recall and near-zero quantization time relative to PQ and RaBitQ.
- Across both tracks, TurboQuant is positioned as a method that tries to combine good geometric fidelity with online, data-oblivious, accelerator-friendly operation.
- These results are paper-reported evidence, not an independent reproduction in this vault.
