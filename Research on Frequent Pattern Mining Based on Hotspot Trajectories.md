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

## 轨迹热点挖掘算法：

### 1. NDTTJ 算法（N-Degree Trajectory Table Join）
- 适用于 **数据稀疏** 情况
- 采用 **有条件连接** 方式逐步生成路径表

### 2. NDTTT 算法（N-Degree Trajectory Table Traversal）
- 适用于 **数据分布密集** 情况
- 采用 **轨迹序列遍历** 方式扩展路径表

### 3. TTHS 算法（Trajectory Traversal Hotspots Search）
- 适用于 **明显图结构** 的轨迹数据
- **利用图数据库（如 Neo4j）** 存储轨迹节点
- **基于遍历搜索+剪枝优化** 发现热点路径

---

## 三种算法的适用场景：

| 算法     | 数据结构    | 数据分布特点  | 适用情况    |
|----------|-------------|---------------|-------------|
| NDTTJ    | 非图结构    | 数据稀疏      | 数据较稀疏 |
| NDTTT    | 非图结构    | 数据分布密集  | 数据密集时  |
| TTHS     | 图结构明显  | 数据量大、关系明显| 最佳选择 |

---

## DataPreprocess
```
import pandas as pd
from sklearn.cluster import DBSCAN
import json

# DBSCAN算法参数（空间聚类，用于确定热点节点）
EPS = 50            # 空间聚类半径（米），推荐30-100米
MIN_SAMPLES = 5     # 最少轨迹点数以形成一个热点节点，论文中常取5

# 数据输入与输出文件名
INPUT_FILE = 'trucks_rev_pos.txt'
OUTPUT_FILE = 'traj_sequences.csv'

# ==================== Step 1 数据加载与清洗 ====================

df = pd.read_csv(INPUT_FILE)

# 删除坐标完全重复的数据点
df = df.drop_duplicates(subset=['traj_id', 'x', 'y'])

# 数据按轨迹ID和时间排序
df = df.sort_values(by=['traj_id', 't']).reset_index(drop=True)

# ==================== Step 2 热点节点聚类生成 ====================

# 使用DBSCAN生成热点节点，论文推荐DBSCAN
db = DBSCAN(eps=EPS, min_samples=MIN_SAMPLES)
df['node'] = db.fit_predict(df[['x', 'y']])

# 移除DBSCAN中的噪声节点（标记为-1）
df = df[df['node'] != -1].reset_index(drop=True)

# ==================== Step 3 形成节点序列与边序列 ====================

trajectory_sequences = []
for traj_id, group in df.groupby('traj_id'):
    nodes = group['node'].tolist()

    # 去除轨迹序列中连续重复的节点（只保留首次进入节点）
    nodes_clean = [nodes[0]]
    for n in nodes[1:]:
        if n != nodes_clean[-1]:
            nodes_clean.append(n)

    # 论文明确要求：至少2个节点构成路径，长度不足则忽略
    if len(nodes_clean) < 2:
        continue

    # 根据节点序列构造边序列
    edges = [(nodes_clean[i], nodes_clean[i + 1]) for i in range(len(nodes_clean) - 1)]

    trajectory_sequences.append({
        'traj_id': traj_id,
        'nodes': nodes_clean,
        'edges': edges
    })

# ==================== Step 4 输出CSV文件 ====================

# 为确保文件格式清晰，节点序列与边序列使用json格式导出
output_df = pd.DataFrame({
    'traj_id': [t['traj_id'] for t in trajectory_sequences],
    'nodes': [json.dumps(t['nodes']) for t in trajectory_sequences],
    'edges': [json.dumps(t['edges']) for t in trajectory_sequences]
})

# 输出CSV文件，符合NDTTJ/NDTTT算法输入要求
output_df.to_csv(OUTPUT_FILE, index=False)

print(f"数据预处理完成，结果保存至文件：{OUTPUT_FILE}")

```