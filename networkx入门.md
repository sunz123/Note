# networkx简介

networkx主要包含五个模板：

1、图对象的类；

2、创建标准图的生成器；

3、在现有数据集中读取的IO接口（比如从csv中读取边和节点的数据）；

4、经典的传统图算法（不包含图神经网络系列算法）；

5、一些方便的画图函数

————————

networkx支持创建的四种图类型

```python
import networkx as nx
# Graph 无向图，允许节点与自身形成闭环；
G = nx.Graph()
# DiGraph 有向图
G = nx.DiGraph()
# MultiGraph 多重无向图，一种灵活的图类，允许节点对之间有多个无向边。额外的灵活性导致性能的一些下降，不过通常不显著；
G = nx.MultiGraph()
# MultiDiGraph多重有向图
G = nx.MultiDiGraph
```

所有图类都允许**任何哈希对象作为节点**。哈希对象包括字符串，元组，整数或是另一个图结构等等。

————————

图内数据结构**基于邻接列表表示**，并使用python字典数据结构实现。

图邻接结构使用python的dict来实现；外部字典由节点键控到本身是字典的值，

——————————

## Nodes and Edges

指定节点和边的类型，如果用户关心的是网络的拓扑结构，那么使用整数或字符串作为节点是有意义的，不需要考虑边数据。如果已经有了描述节点的数据结构，可以简单地使用该结构作为节点，只要它是hashable的，如果不可散列，则可以使用唯一标识符来表示节点，并将数据分配为节点属性。

边通常有与它们相关的数据。任意数据可以作为边属性与边相关联。如果数据是数字的，其意图是表示加权图，则使用‘weight’作为其属性的关键词。一些图形算法，如Dijkstra的最短路径算法，默认情况下使用这个属性名‘weight‘来获得每个边缘的权重。

属性可以通过在添加边时使用key/value来分配给边。可以使用任何关键字来命名属性，然后可以使用该属性关键字查询边数据。

## Graph Creation

networkx图对象可以以下三种方式创建：

1、图形生成器 Graph generators创建网络拓扑的标准方法；

2、从预先存在的（通常是文件）来源导入数据；

3、显式添加边缘和节点

```python
import networkx as nx
G = nx.Graph()
# 自动生成节点1和2
G.add_edge(1,2) # default edge data=1
# 自动生成节点3，并且带权值0.9
G.add_edge(2, 3, weight=0.9) # specify edge data
```

```python
import math
# 边属性可以是任何hashable的对象
G.add_edge('y', 'x', function=math.cos)
G.add_node(math.cos) # any hashable can be a node
```

```python
# 可以同时添加很多边
elist = [(1, 2), (2, 3), (3, 4), (4, 5)] # 无权值边
G.add_edges_from(elist)
elist = [('a', 'b', 5.0),('b', 'c', 3.0), ('a', 'c', 1.0), ('c', 'd', 7.3)] # 有权值边
G.add_edges_from(elist)
```

## Graph Reporting

类视图提供节点，邻居，边和度的基本报告。这些视图提供了属性的迭代以及成员查询和数据属性的方法。视图是指图形数据结构，因此对图行的更改反映在视图中。这类似于python3中的字典视图。如果你想在迭代时更改图形，则需要使用例如：`for e in list(G.edges):`的方法来处理，视图提供类似集合的操作，例如：联合和交叉，以及类似dict的数据属性的查找和迭代，`G.edges[u, v]['color'] and for e, datadict in G.edges.items()`,方法G.edges.items()和G.edges.values()是熟悉python Dict api的形式。

边的基本图形关系可以通过两种方式得到：寻找节点的邻居，也可以寻找边，我们可以开玩笑地定义：关注节点/邻居的人是以节点为中心的，关注边缘的人是以边缘为中心的，networkx的设计者倾向于以节点为中心，并将边视为节点之间的关系。可以通过选择查找符号来看到这一点，例如：`G[u]`提供邻居（邻接），而边查找是`G.edges[u, v]`。稀疏图的大多数数据结构本质上是邻接列表（adjacency lists），因此符合这一观点。最后，当然用哪种方法检查图表并不重要。`G.edges`重复表示无向边（无向边等价于双向边，通过这种方式来统一两种边类型），而neighbor reporting显示节点的两个方向的所有邻节点。

任何比边、邻居和度更复杂的属性都是由函数提供的，例如`nx.triangles(G, n)，返回在图G上，n节点所在的三角关系的数量。这些函数都统一在networkx的algorithms模块。