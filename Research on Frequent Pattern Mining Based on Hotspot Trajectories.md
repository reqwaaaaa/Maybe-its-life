# Draft Research Proposal

## 轨迹热点挖掘的基础概念与公式

### 1. 关系（Relation）
给定集合 $A=\{a_1,a_2,...,a_n\}$ 和 $B=\{b_1,b_2,...,b_m\}$，如果元素 $a_i$ 和 $b_j$ 之间存在关系，则定义如下：

- **单向关系**：$a_i \to b_j$ 或 $a_i \leftarrow b_j$
- **双向关系**：$a_i \leftrightarrow b_j$

### 2. 并置模式（Co-location）
定义集合 $A,B$ 之间的关系集合 $R_{AB}$ 为并置模式：

$$
R_{AB} = \{a \to b \lor b \to a \lor a \leftrightarrow b \mid a\in A, b\in B\}
$$

### 3. 参与集（Participation Set）
集合 $A,B$ 之间的所有关系所含元素的集合：

$$
C=\{\forall a,\forall b \mid a\in A,b\in B,\exists a\to b \lor \exists b\to a\}
$$

### 4. 参与度（Participation Rate）
衡量集合 $A,B$ 之间关系的强度：

$$
\gamma_{AB}=\frac{|C|}{|A|+|B|}
$$

### 5. 频繁并置（Prevalent Co-location）
若参与度满足阈值 $\gamma_{min}$，则称为频繁并置模式集合：

$$
PR_{AB}=\{R_{AB}\mid\gamma_{AB}\geq\gamma_{min}\}
$$

### 6. 轨迹序列（Trajectory Sequence）
由物理空间中 $k$ 个坐标点按照时间顺序线性连接构成：

$$
R = \{N, E\}
$$

其中：

- $N=\{n_1,n_2,...,n_k\}$ （轨迹点集合）
- $E=\{e_1,e_2,...,e_{k-1}\}$ （轨迹边集合）

### 7. 轨迹序列集合（Trajectory Sequence Set）
如果集合 $G_a$ 由 $m$ 条轨迹序列构成，则定义为：

$$
G_a=\{R_1,R_2,...,R_m\}
$$

### 8. 轨迹序列并置（Trajectory Sequence Co-location）
如果 $G_b\subseteq G_a$，且 $G_b$ 中所有轨迹都经过节点 $n_i$，则称 $G_b$ 为轨迹序列并置集合，$n_i$ 为并置节点。

### 9. 轨迹序列频繁并置（Trajectory Sequence Prevalent Co-location）
假设轨迹序列并置集合 $G_c$ 中的轨迹数量为 $m$，且满足频繁度阈值：

$$
m \geq m_{min}, \quad m_{min} \geq 2
$$

则称 $G_c$ 为轨迹序列频繁并置集合。

### 10. 轨迹热点（Trajectory Hotspots）
在轨迹序列频繁并置集合中，如果存在 $k \geq 2$ 个连续的并置节点，且满足路径长度阈值：

$$
k \geq k_{min}, \quad k_{min} \geq 2
$$

则称 $G_d$ 上存在轨迹热点 $H$：

$$
H=\{n_1\to n_2\to ... \to n_k\}
$$

---
# 实验原理（Markdown 版）

> **课题**：基于热点轨迹的频繁模式挖掘研究  
> **目标**：在保证时空效率的前提下，体系化挖掘 Geo‑Trajectories 数据中的热点路径（Hotspot Trails），并为后续的行为预测与智能推荐奠定理论基础。

---

## 1. 轨迹数据建模

设移动对象的原始轨迹为离散点序列 \(R=\{p_1,\,p_2,\,\dots, p_m\}\)，其中
\[
 p_i=(x_i,\,y_i,\,t_i,\,\text{meta}_i),\qquad i=1,\dots,m.
\]
为了降低计算复杂度与噪声，采用**时间采样**\(\Delta t=15\text{min}\)与**空间网格化**（R&D 网格）将原始轨迹映射到 **节点集合** \(V\) 与 **有向边集合** \(E\)：

* **节点**：\(v\in V\iff\exists p_i\text{ 落入网格 }g_v\)
* **边**：\(e=(v_i\rightarrow v_j)\in E\iff p_{k}\in g_{v_i},\,p_{k+1}\in g_{v_j}\)

得到 **一阶路径表**（1‑degree path table）：
\[
T_1=\{\bigl( v_i, v_j, \text{traj\_set}_{ij}\bigr)\mid (v_i\to v_j)\in E\}.
\]
其中 \(\text{traj\_set}_{ij}\) 为经过该边的轨迹 ID 集合。

---

## 2. 并置模式与热点路径定义

### 2.1 并置模式（Co‑location Pattern）
给定两个对象集 \(A,B\) 与空间关系谓词 \(\mathcal{R}(\cdot)\)，其并置模式定义为
\[
\mathcal{C}_{A,B}=\bigl\{(a,b)\mid a\in A,\,b\in B,\,\mathcal{R}(a,b)=\text{true}\bigr\}.
\]

### 2.2 频繁并置
参与度：  \(\gamma_{A,B}=\dfrac{|\mathcal{C}_{A,B}|}{|A|+|B|}\).
若 \(\gamma_{A,B}\ge \gamma_{\min}\)，称 \(A,B\) 存在**频繁并置**。

### 2.3 热点路径（Hotspot Trail）
* **长度阈值** \(k_{\min}\)
* **支持阈值** \(m_{\min}\)

若存在有向节点序列 \(H = (v_1\to\dots\to v_k)\) 满足：
1. \(k\ge k_{\min}\)
2. 经过该序列的轨迹集合 \(\mathrm{SG}_H\) 满足 \(|\mathrm{SG}_H|\ge m_{\min}\)

则称 \(H\) 为热点路径。

---

## 3. 三类热点挖掘算法

| 类别 | 代表 | 适用场景 | 核心复杂度 |
|------|------|-----------|-------------|
| Apriori‑Join | **NDTTJ** | 轨迹稀疏 | \(\mathcal{O}(n^2)\) 时间，\(\mathcal{O}(n^2)\) 空间 |
| Pattern‑Growth | **NDTTT** | 轨迹密集 | \(\mathcal{O}(n\log n)\) 时间，\(\mathcal{O}(n)\) 空间 |
| Graph‑Traversal | **TTHS** | 大规模图结构明显 | \(\mathcal{O}(n)\) 时间，\(\mathcal{O}(n)\) 空间 |

### 3.1 NDTTJ — N‑Degree Trajectory Table *Join*
1. **初始队列**：筛出满足 \(m_{\min}\) 的一阶边。  
2. **连接规则**：若 \(p_1=(v_1\dots v_r), p_2=(v_r\dots v_{r+1})\)，则
\[
\text{new}\_\text{path}=p_1\cup p_2, \quad \text{new}\_\text{sg}=\text{sg}_{p_1}\cap \text{sg}_{p_2}.
\]
3. **剪枝**：\(|\text{new}\_\text{sg}|<m_{\min}\) 直接丢弃。  
4. 迭代至无新路径或达到 \(\text{max\_depth}\)。

### 3.2 NDTTT — N‑Degree Trajectory Table *Traversal*
深度优先，以尾节点为锚增长，不产生候选集，适合稠密路径。

### 3.3 TTHS — Trajectory‑Traversal Hotspot Search
利用 Neo4j / JanusGraph **免索引邻接**优势，按边权“度”剪枝。

---

## 4. 多维特征工程

### 4.1 时间特征
* **平均起始时刻** \(\bar{h}=\dfrac{1}{|\mathrm{SG}|}\sum_i h_i\)
* **时间熵** \(H_t=-\sum_{b} p_b\log_2 p_b\)

### 4.2 空间特征
* **欧氏路径长**：\(L=\sum_{i=1}^{k-1}\|\mathbf{v}_{i+1}-\mathbf{v}_{i}\|_2\)
* **空间熵**：对经纬度各做 1D 熵并求和。

### 4.3 语义特征
* **主导 POI**：\(\arg\max_{c} \text{freq}(c)\)
* **POI 熵** \(H_{poi}\)，衡量类型多样性。

---

## 5. 数据流程汇总

```mermaid
flowchart TD
    %% ========== ① 数据清洗 ==========
    A["Geolife .plt<br/>Raw trajectories"] -->|"15-min<br/>Sampling"| B["Cleaned CSV"]

    %% 拆分
    B --> NODES["nodes.csv"]
    B --> EDGES["edges.csv"]
    B --> META["traj_metadata.csv"]

    %% ========== ② 热点挖掘 ==========
    %% ---- Path-table line ----
    META --> PT1["1-degree<br/>Path Table"]
    PT1 --> HT1["NDTTJ / NDTTT<br/>Hotspots"]:::algo

    %% ---- Graph line ----
    NODES & EDGES --> GDB["Neo4j / JanusGraph"]:::store
    GDB --> HT2["TTHS<br/>Hotspots"]:::algo

    %% 合并
    HT1 & HT2 --> M["Merged hotspot set"]

    %% ========== ③ 三维特征 ==========
    M -->|时间映射| TF["Temporal features"]
    M -->|空间投影| SF["Spatial features"]
    M -->|POI映射| PF["Semantic features"]
    NODES --> PF
    META  --> TF

    TF & SF & PF --> EH["Enhanced hotspot table"]

    %% ========== ④ 质量清洗 ==========
    EH -->|"IQR-trim<br/>(length)"| CQ{"滤除极端值"}
    CQ --> FP["Final cleaned paths.csv"]

    %% ---------- 样式 ----------
    classDef algo  fill:#ffe2e2,stroke:#d44,stroke-width:1.3px,color:#000;
    classDef store fill:#e1ecff,stroke:#4682ff,stroke-width:1.3px,color:#000;
```
---

# 高阶语义热点轨迹频繁模式挖掘方案

## 背景说明
本方案以 `cleaned_paths.csv` 为核心数据基础，构建在三类轨迹热点挖掘算法（**NDTTJ / NDTTT / TTHS**）之上，结合已构造的时空、POI 语义、轨迹结构等多维特征，提出一个可落地、可解释的**频繁语义路径模式挖掘系统**，其目标是：

- 发挥三类热点挖掘算法生成路径序列的结构优势（稳定、高覆盖）；
- 基于熵加权构建支持度体系（SU-Support），挖掘更有区分度和语义代表性的热点路径模式；
- 借助自适应算法调度与剪枝，提升效率和表达质量；
- 支持后续 Web 可视查询与 API 接口输出。

---

## 建模与支持度构建

### 三维熵加权支持度定义（SU-Support）

传统频繁模式支持度为：
\[ \text{Support}(H) = |\mathcal{T}_H| \]

我们引入三维稳定性加权（空间、时间、语义）：

\[
\mathrm{SU}(H) = |\mathcal{T}_H| \cdot \left(1 - \lambda \cdot H_s'(H)\right) \cdot \left(1 - \mu \cdot H_t'(H)\right) \cdot \left(1 - \nu \cdot H_{poi}'(H)\right)
\]

- \( H_s', H_t', H_{poi}' \in [0,1] \)：分别为空间、时间、POI 熵归一化后值；
- \( \lambda, \mu, \nu \)：三维惩罚系数，控制熵惩罚强度；
- 当三个熵值越高，路径越不稳定，SU 值越小。

### 差分进化优化权重系数
使用差分进化（Differential Evolution）寻找三元最优权重参数：

- **目标函数**：

\[
\max_{\lambda,\mu,\nu} \left[ \alpha \cdot \text{Coverage}(\theta) - (1-\alpha) \cdot \text{Redundancy}(\theta) \right]
\]

- \(\text{Coverage}\)：SU 前 K 模式覆盖所有轨迹的比例；
- \(\text{Redundancy}\)：前 K 模式之间的 Jaccard 平均相似度；
- \(\theta\)：SU 阈值，保留高质量候选集；
- \(\alpha\)：权衡因子，推荐值为 0.7。

---

## 自适应算法调度机制

### 核心思想：
- NDTTJ 适合稀疏轨迹结构（连接型 Join）
- NDTTT 适合中等密度（深度优先 Path-Growth）
- TTHS 适合稠密区域（图结构下 DFS 遍历）

### 实现方式：
1. 对每条路径计算空间密度：

\[
\rho(H) = \frac{1}{H_s'(H) + \varepsilon}
\]

2. 设置分界阈值 \(\rho_l, \rho_h\)：

| 密度区间           | 调用算法 |
|--------------------|-----------|
| \( \rho < \rho_l \)      | NDTTJ    |
| \( \rho_l \le \rho < \rho_h \) | NDTTT    |
| \( \rho \ge \rho_h \)     | TTHS     |

3. 可训练决策树/聚类划分 \(\rho\) 区间，动态微调

---

## 模式构建与双层消歧流程

### 步骤一：SU-FP-Tree 构建
- 将 `path` 序列构建 FP-Tree，使用 SU 值作为浮点计数；
- 前缀遍历生成候选模式集 \(\mathcal{P}_1\)

### 步骤二：PrefixSpan 补充
- 使用 Spark MLlib 执行 PrefixSpan，补充遗漏的序列模式 \(\mathcal{P}_2\)
- 最终模式集 \(\mathcal{P} = \mathcal{P}_1 \cup \mathcal{P}_2\)

### 步骤三：超图 k-Truss 精炼
- 将模式转为超边图结构 \(\mathcal{G}\)，节点为热点格点；
- 执行 \(k\)-truss 分解（\(k = \lceil |H|/2 \rceil\)）保留强连通模式

### 步骤四：信息密度增益剪枝

定义模式信息增益：

\[
\text{Gain}(H) = \frac{\mathrm{SU}(H)}{|H| \cdot \log_2(|\text{POI}_{H}|+1)}
\]

- $|\text{POI}_H|$：该路径涵盖的 POI 类别数目
- 使用阈值 \(\tau_G\) 保留高 Gain 模式

---

## 实验流程与模块落地

| 模块             | 工具/方法                  | 输入列                             | 说明                     |
|------------------|-----------------------------|-------------------------------------|--------------------------|
| 特征归一化       | pandas/sklearn              | `time_entropy`, `spatial_entropy`, `poi_entropy` | 归一化到 [0,1]            |
| SU 计算 & DE调参 | numpy + deap                | `frequency`, 上述熵列               | 输出 SU 值和 (λ,μ,ν)最优组合 |
| 调度标签学习     | sklearn DecisionTreeClassifier | `source`, `spatial_entropy`         | 预测当前热点适用算法类别   |
| FP-Tree 构建     | 自定义 FP-Tree 类             | `path`, `SU`                       | 构建序列频繁模式树         |
| PrefixSpan       | Spark MLlib                 | `path`                             | 规则匹配追加候选           |
| k-truss 剪枝     | NetworkX or custom impl     | `path`节点                         | 保留强结构模式             |
| Gain 计算        | numpy                       | SU值, `path_length`, `poi_types`   | 信息密度排序               |

---