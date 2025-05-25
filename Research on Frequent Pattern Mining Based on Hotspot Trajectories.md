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
## 基础概念补充

### 热点轨迹序列的POI映射（扩展定义）

在第一层挖掘中，热点轨迹 \( H = \{n_1 \to n_2 \to \dots \to n_k\} \) 已通过空间聚类和并置模式提取。利用高德地图API，每个热点节点 \( n_i \) 被映射为POI类型（如“科教文化服务”、“休闲”），生成POI类型序列：
\[
S_H = \{ \text{POI}_1 \to \text{POI}_2 \to \dots \to \text{POI}_k \}.
\]

- **有向性**：POI序列严格保留访问顺序，即 \( \text{POI}_i \to \text{POI}_{i+1} \) 表示从 \( \text{POI}_i \) 到 \( \text{POI}_{i+1} \) 的单向转移。
- **时段性**：基于时间戳 \( t_i \)，将序列按时间窗口分割（如上午、下午）。定义时间窗口 \( W(t) \)：
  \[
  W(t) =
  \begin{cases} 
  \text{上午}, & \text{if } 6:00 \leq t < 12:00, \\
  \text{下午}, & \text{if } 12:00 \leq t < 18:00, \\
  \text{晚上}, & \text{if } 18:00 \leq t < 24:00.
  \end{cases}
  \]
  每个 \( S_H \) 被标注为 \( S_H^w \)，表示在时间窗口 \( w \) 内的POI序列。

#### 新定义：POI转移概率
为量化POI类型间的转移倾向，定义转移概率 \( P(\text{POI}_j \mid \text{POI}_i, w) \)，表示在时间窗口 \( w \) 内从 \( \text{POI}_i \) 转移到 \( \text{POI}_j \) 的条件概率：
\[
P(\text{POI}_j \mid \text{POI}_i, w) = \frac{\text{count}(\text{POI}_i \to \text{POI}_j, w)}{\sum_{\text{POI}_k} \text{count}(\text{POI}_i \to \text{POI}_k, w)},
\]
其中 \( \text{count}(\text{POI}_i \to \text{POI}_j, w) \) 是 \( S_H^w \) 中 \( \text{POI}_i \to \text{POI}_j \) 的出现次数。

### 母路径的定义（扩展）

母路径 \( T_{\text{mother}} \) 是包含热点轨迹 \( H \) 的原始轨迹集合，由`traj_ids`（格式为`<uid, traj_id>`）标识：
\[
T_{\text{mother}} = \{ T \mid T \in G_a, H \subseteq T \},
\]
其中 \( T = \{p_1 \to p_2 \to \dots \to p_m\} \)，每个点 \( p_i \) 映射为POI类型，生成母路径的POI序列 \( S_T \)。

#### 新定义：母路径的扩展子序列
定义母路径中包含热点轨迹 \( H \) 后的扩展子序列 \( S_T^{\text{ext}} \)，即从 \( H \) 的最后一个POI类型开始的后续序列：
\[
S_T^{\text{ext}} = \{ \text{POI}_{k+1} \to \text{POI}_{k+2} \to \dots \to \text{POI}_m \mid S_T = S_H \cup S_T^{\text{ext}} \}.
\]

### 频繁模式的定义（统计严格化）

#### 序列模式（改进）
给定POI序列集合 \( S \)，频繁子序列 \( S_f \) 满足支持度阈值：
\[
\text{support}(S_f) = \frac{|\{S_i \mid S_f \subseteq S_i, S_i \in S \}|}{|S|} \geq \text{sup}_{\text{min}}.
\]

##### 加权支持度
为考虑时段性和POI转移概率，引入加权支持度：
\[
\text{support}_w(S_f) = \frac{\sum_{S_i \in S, S_f \subseteq S_i} w(S_f, S_i)}{\sum_{S_i \in S} w(S_i)},
\]
其中 \( w(S_f, S_i) = \prod_{(\text{POI}_a \to \text{POI}_b) \in S_f} P(\text{POI}_b \mid \text{POI}_a, w) \cdot e^{-\lambda |\Delta t|} \)，结合转移概率和时间衰减。

#### 图模式（改进）
构建POI类型图 \( G(V, E) \)，节点为POI类型，边为转移关系，边权为转移概率 \( P(\text{POI}_j \mid \text{POI}_i, w) \)。频繁子图 \( G_f \) 满足：
\[
\text{support}(G_f) = \frac{|\{G_i \mid G_f \subseteq G_i, G_i \in G \}|}{|G|} \geq \text{sup}_{\text{min}}.
\]

##### 子图置信度
定义子图的置信度，衡量其预测能力：
\[
\text{confidence}(G_f) = \frac{1}{|E_f|} \sum_{(\text{POI}_i \to \text{POI}_j) \in E_f} P(\text{POI}_j \mid \text{POI}_i, w),
\]
其中 \( E_f \) 是子图 \( G_f \) 的边集。

## 研究思路（续）

### 阶段1：热点轨迹特性挖掘

#### 目标
提取热点轨迹的“种子”模式，结合统计概率和时段性。

#### 输入
热点轨迹的POI序列集合 \( S_H \)，包含时段信息。

#### 方法
1. **时段分割与转移概率计算**：
   - 按时间窗口 \( W(t) \) 分割 \( S_H \)，生成 \( S_H^w \)。
   - 计算每个时间窗口内的转移概率 \( P(\text{POI}_j \mid \text{POI}_i, w) \)。
2. **序列模式挖掘（基于Apriori和PrefixSpan改进）**：
   - **Apriori-based方法**：首先生成1阶频繁子序列（如“科教文化服务 -> 休闲”），通过连接生成高阶候选模式，计算加权支持度 \( \text{support}_w \)，筛选满足 \( \text{sup}_{\text{min}} \) 的模式。
   - **PrefixSpan-based方法**：以模式增长方式挖掘频繁子序列，加入时段权重 \( w_t = e^{-\lambda |\Delta t|} \)，减少候选生成，提高效率。
3. **图模式挖掘**：
   - 构建POI类型图 \( G(V, E) \)，边权为 \( P(\text{POI}_j \mid \text{POI}_i, w) \)。
   - 使用模式增长算法（如ESGROWTH），挖掘频繁子图，筛选支持度和置信度均满足阈值的子图。

#### 输出
种子模式（如“科教文化服务 -> 休闲”，加权支持度0.8，置信度0.7）。

#### 创新点
- **加权支持度**：结合转移概率和时间衰减，增强模式的时空相关性。
- **双算法融合**：Apriori适合稀疏数据，PrefixSpan适合稠密数据，结合两者提升挖掘效率和准确性。

### 阶段2：母路径扩展挖掘（创新）

#### 目标
基于种子模式，挖掘母路径中的扩展模式，预测后续活动。

#### 输入
种子模式 + 母路径的POI序列集合 \( T_{\text{mother}} \).

#### 方法
1. **序列扩展（基于PrefixSpan改进）**：
   - 以种子模式为前缀，提取 \( S_T^{\text{ext}} \)，生成扩展序列。
   - 使用PrefixSpan挖掘频繁扩展子序列，计算加权支持度 \( \text{support}_w \)。
   - 引入**语义约束**：若扩展子序列中出现不合理转移（如“科教文化服务 -> 科教文化服务”），降低其权重或剔除。
2. **图扩展（基于图模式增长）**：
   - 将 \( T_{\text{mother}} \) 的POI序列融入热点轨迹图，更新边权。
   - 使用模式增长算法挖掘频繁子图，计算支持度和置信度。
   - **动态边权调整**：根据时段和用户群体（基于`uid`）动态调整边权，例如学生群体的“科教 -> 休闲”边权更高。
3. **用户分组**：
   - 基于`uid`分组，分别挖掘不同用户群体的扩展模式，分析行为差异。

#### 输出
扩展模式（如“科教文化服务 -> 休闲 -> 娱乐”，加权支持度0.6，置信度0.65）。

#### 创新点
- **语义约束**：引入POI类型间的语义合理性，减少无效模式。
- **动态边权**：结合时段和用户特性，增强图模式的个性化。

### 阶段3：行为趋向分析与预测（创新）

#### 目标
利用挖掘的模式，预测后续行为，分析用户行为差异。

#### 方法
- **预测任务**：
  - 基于扩展模式，预测母路径的后续POI类型。
  - 使用置信度最高的模式进行预测：
    \[
    \text{predicted POI} = \arg\max_{\text{POI}_j} \text{confidence}(S_f \to \text{POI}_j).
    \]
- **行为分析**：
  - 通过`uid`分组，识别不同用户群体的行为趋向。
  - **统计检验**：使用卡方检验验证群体间模式差异的显著性：
    \[
    \chi^2 = \sum \frac{(\text{observed} - \text{expected})^2}{\text{expected}},
    \]
    其中 \( \text{observed} \) 为实际模式频率，\( \text{expected} \) 为均匀分布假设下的期望频率。
- **评估指标**：
  - **支持度**：模式出现频率（加权支持度）。
  - **置信度**：后续POI的条件概率。
  - **预测准确率**：预测结果与实际的匹配比例。
  - **显著性**：卡方检验的p值，验证模式差异的统计显著性。

#### 创新点
- **统计检验**：引入卡方检验，确保行为分析的统计严谨性。
- **置信度预测**：基于概率模型提升预测可靠性。

---
