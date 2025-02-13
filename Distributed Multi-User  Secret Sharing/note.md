# ABSTRACT 我们研究的是分布式多用户秘密共享（DMUSS）问题，涉及一个主节点、N 个存储节点和 K 个用户
索引词条--秘密共享、分布式存储、多用户保密、线性方案、容量区域
# I INTRODUCTION
以下是分布式多用户秘密共享的基本原理
在分布式多用户秘密共享（DMUSS）系统中，存储节点中存储的不是直接的秘密消息，也不是其他用户的密钥，而是编码后的份额。这些份额是秘密消息经过特定算法处理后的结果，它们被设计成只有特定的用户集合（通常是满足某个阈值条件的用户集合）能够联合起来恢复出原始的秘密消息。以下是具体的解释：

1. 秘密消息的分割：在DMUSS系统中，主节点会将每个秘密消息分割成多个份额（shares）。这些份额是通过对秘密消息进行编码得到的，编码过程通常涉及到数学运算，如多项式插值（在Shamir的秘密共享方案中）。

2. 份额的存储：这些编码后的份额被存储在不同的存储节点上。每个用户根据其权限可以访问一部分或全部的存储节点，但只有当他们访问的节点数量达到某个阈值时，才能恢复出原始的秘密消息。

3. 份额的组合：当用户收集到足够的份额时，他们可以使用特定的解码算法（如拉格朗日插值）来组合这些份额，从而恢复出原始的秘密消息。

4. 保密性：由于份额是编码后的，单个份额本身不包含任何可以直接解读的秘密消息信息，因此即使攻击者能够访问部分存储节点，他们也无法从中恢复出任何有用的信息，除非他们能够访问到足够的份额。

5. 密钥的角色：在某些秘密共享方案中，可能会使用密钥来增强安全性，例如，使用对称密钥或非对称密钥对份额进行加密。在这种情况下，存储节点中存储的可能是加密后的份额，而密钥则由主节点安全地分发给授权用户。
总结来说，在DMUSS系统中，存储节点中存储的是编码后的份额，而不是直接的秘密消息或密钥。这些份额被设计成只有授权用户才能恢复出原始的秘密消息，从而确保了系统的安全性和秘密消息的保密性。
---
名词解释
1. 秘密消息：不单单指密文，也可以是明文指原始的、未经加密的数据或信息，这些数据需要被安全地存储和传输，以防止未授权访问。
2. 中央用户（交易商）他与所有用户之间有直接、可靠和安全的通信渠道，可以视为控制一组存储节点的主节点
3. 秘密份额 主节点将每个秘密消息分割成多个份额。这个过程通常涉及到秘密共享算法，如Shamir的秘密共享或Blakley的阈值秘密共享。分割的目的是确保只有只有当一定数量的用户（达到预设的阈值）联合起来时，他们才能恢复出原始的秘密消息
4. 主节点 可以访问所有存储节点，但不能访问用户
5. 访问集 用户可以访问到的一组（几个）存储节点叫做该用户的访问集
6. 访问结构 所有访问集的集合，表征所有用户可以访问存储节点的拓扑结构
7. 正确性 当他们访问自己有权访问的存储节点时，应该能够准确地重建他们的秘密消息，而不会有任何错误或信息丢失
8. 保密性 保密性条件指的是每个用户对于其他用户的独立秘密消息一无所知。
9. 用户速率 其秘密消息的大小/存储节点的大小
10. 速率元组 由所有用户速率组成的向量
11. 容量区域 **_容量区域定义了在满足完美保密和正确性条件的前提下，所有可能的速率组合（即每个用户秘密消息大小与存储节点大小的比例）的集合。这些速率组合代表了系统在不同用户之间分配秘密消息时能够达到的最大效率。_**
12. - [] 编码函数？ 解码函数？
13. - [] 同构多播中继网络？ 中间节点，终端节点？[x]
---
讨论了通信复杂性和 l 门限保密条件，后者是对完全保密的概括。
我们证明，对通信最优 DMUSS 方案的研究可以转化为一个优化问题。

# II. SYSTEM MODEL
符号定义
1. K个秘密消息在有限域上均匀分布，用户数也是K，存储节点数为N，K个秘密消息要被分为N个秘密份额，所以一个节点正好存储一个份额
2. A 访问结构
3. $A_k$ 用户k的访问集
4. $W_k$ 第k个秘密消息
5. $Y_n$ 第n个存储节点的内容，也即第n个秘密份额
6. $Y_{A_K}$ ≜ {Yi : i ∈ Ak} 第k个用户从他的访问集中获得的所有存储节点的内容的综合
7. L 每个存储节点的大小
8. $R_k$ 第k个密文的速率 $R_k$ ≜ \frac{H (Wk)}{L}，表示秘密信息被分成了多少个shares，每个shares大小和存储节点大小一样，这些shares被存放在几个存储节点中，用户k的访问集是大于等于Rk的。大于是因为，他可以访问其他秘密消息的部分shares
   这两种表示都成立，并不冲突，在分割时进行填充使得秘密消息总是被分割为整数个share
10. E is a collection of N encoding functions En将主节点已知的 K 个密文映射到存储在第 n 个存储节点中的共享 Yn。编码函数用于将秘密消息转换为存储在各个节点上的份额，而解码函数则用于从这些份额中恢复出秘密消息。
11. D 解密函数，与编码函数正好相反

对于单个秘密消息的恢复：每个用户可以独立恢复其被分配的秘密消息，这是因为秘密共享方案确保了每个用户获得的份额足以恢复其对应的密文 wk
对于秘密消息的安全性：尽管每个用户可以恢复自己的密文，但他们没有足够的信息来恢复其他用户的秘密消息，除非他们联合起来达到预设的阈值。这个阈值通常高于单个用户恢复自己密文所需的份额。
 - [] K个秘密消息，分割，编码后形成N个秘密份额放在N个存储节点中这个过程
定义3 最佳密文速率
# III. MAIN RESULT
本文的主要成果是描述了完美保密条件下 DMUSS 问题容量区域的特征。在某些限制条件下，给出秘密消息的大小，或者秘密份额应该分为多大，极限值是多少？
定理 1：给定访问结构 A，容量区域 RA 是一个凸区域，其非负速率元组 r ≜ (R1, ... , RK ) 满足条件：证明在五六两节
有点难理解，再看
在可实现性证明中，我们提供了一种算法来说明，当给定访问结构时，任何满足容量公式（4）的合理速率元组都对应于 DMUSS 方案，这也就得到了对于该访问结构的一个容量区域
∑  k∈[1:K ] Rk在这个式子中，rk表示wk被分成几个shares，也即第 k 个用户为了恢复其秘密消息所需的最小份额数量
# IV. COMPARISON WITH RELATED RESULTS
# VI. THE ACHIEVABILITY PROOF
在本节中，我们首先介绍 DMUSS 问题线性方案的定义，其中编码和解码函数都是线性的。然后，我们给出可实现性算法的细节，并讨论其可行性和正确性。结果证明，给定访问结构，任何满足容量公式（4）的合理速率元组都对应于线性方案。此外，(4) 所描述的容量区域的角点都是有理点，因此可实现性证明是完整的。\\
线性方案是指编码和解码函数都是线性的方案\\
随机性M被用来确保每个份额包含一定程度的随机性\\
Yn =  [wT mT ] En根据这个式子可知，，一个编码函数En得到一个Yn存入存储节点，因此编码函数又N个\\
每个用户 k 执行线性解码，这意味着他们使用一个解码矩阵 Dk来从他们可以访问的份额中恢复其秘密消息，因此解码函数又K个\\
为什么子矩阵行向量线性无关，就能得出用户k无法从他们可以访问的份额中获得任何关于其他用户秘密消息的信息?\\
子矩阵 是满秩的，这意味着用户 k无法通过线性组合的方式从他们无法访问的份额中推断出其他用户的信息。然而，这并不意味着用户k 可以从他们可以访问的份额中推断出其他用户的信息。\\
在DMUSS系统中，我们需要选择适当的标量值来确定不定元，使得这个乘积多项式的值不为零。这可以通过选择标量值来实现，使得每个 
det(Hk)都不为零。如果有限域的大小q≥K，则存在这样的标量值。通过这种方法，我们可以确保每个Hk是满秩的，从而满足系统的完美保密性条件
行列式多项式乘积 的度数最多为 （K-1）∑i∈[1:K]Ri
