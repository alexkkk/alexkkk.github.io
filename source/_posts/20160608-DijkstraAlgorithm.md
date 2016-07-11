---
title: DijkstraAlgorithm
date: 2016-06-08 23:37:17
tags: 
 - algorithm
 - graph
 - greedy
---

## 问题描述
给定1个图（这里应该是无向图）和图中的1个源结点，找到这个图中从源点到其余各点的最短距离

## 算法
Dijkstra的算法和最小生成树的Prim算法非常相像。类似Prim的最小生成树(MinimumSpanningTree)，我们将给定的源点作为跟，生成了最小路径树(ShortestPathTree)。
算法中我们维护2个集合，
1) SptSet集合，用来保存最短路径树包含的节点
2）NotSptSet集合, 用来保存没有包含在最短路径树种的节点。
在算法的每一步，我们找出没有包含在最短路径树中跟源点距离最小的节点。

下面是我们用Dijkstra算法去找出给定的图中从1个源点到其余别的节点的最短路径。
算法：
1) 创建1个集合叫SptSet, 用来跟踪最短路径树种的所有节点的信息，里面保存了每个节点到源点的最短距离。初始的时候，这个集合是空的
2) 给图中每个节点设置一个距离的值。初始化的距离为无穷大。源点的距离是0，所以它是第1个被挑选出来的，假设源点是S。
3) 当SptSet没有包含所有的节点的时候:
….a) 从NotSptSet中选出那个和源点有最短距离的节点U
….b) 把U放入到SptSet中
….c) 更新节点U的邻接节点的距离。更新距离的时候，遍历所有的邻接节点，对每个邻接节点v来说，如果(S,U)的距离加上(U,V)的距离小于（S,V）的距     离，则更新(S,V)的距离

让我们用下面的图来理解问题:
///{% asset_img 0.jpg %} 这个引入图片的方法不好使，还要研究下
![graph](DijkstraAlgorithm/0.jpg)
SptSet集合被初始化为空，源节点到每个节点的距离初始化为{0, INF, INF, INF, INF, INF, INF, INF}。INF代表无穷大。
现在开始挑选距离最小的节点，所以首先节点0被挑中，放到SptSet中，所以SptSet变为{0}.
在把节点0加入到SptSet集合之后，更新它的邻接节点到源节点的距离。
节点0的邻接节点有节点1和7，所以节点1和7的距离分别更新为4和8.

下面的图显示了节点以及他们到源点的距离，只有不是无穷大的节点才显示。绿色节点表示已经在SptSet中。
![graph](DijkstraAlgorithm/1.jpg)
从不在SptSet中的节点挑选距离最小的。节点1被选中并加入到SptSet中。那么SptSet现在变成{0,1}。
更新节点1的邻接节点到源点的距离。节点1的邻接节点是2，它的距离变成12.
![graph](DijkstraAlgorithm/2.jpg)
继续从不在SptSet中的节点中挑选距离最小的，这次被选中的是节点7.所以现在SptSet={0, 1, 7}.
更新节点7的邻接节点的距离，节点7的邻接节点是6和8，他们的距离被更新为15和9
![graph](DijkstraAlgorithm/3.jpg)
继续从不在SptSet中的节点中挑选距离最小的，这次被选中的是节点6.所以现在SptSet={0, 1, 7, 6}.
更新节点6的邻接节点的距离，节点6的邻接节点是5和8，他们的距离被更新为11和15
![graph](DijkstraAlgorithm/4.jpg)
只要SptSet没有包含所有的节点，我们就一直重复上面的操作。最终我们得到如下的最短路径树。
![graph](DijkstraAlgorithm/5.jpg)

## 实现
```c++

#include <iostream>
#include <vector>

using namespace std;


typedef std::vector<std::vector<int>> Graph;

class Dijkstras {
    public:
        Dijkstras(const Graph& graph);
        ~Dijkstras() = default;

        void calculate_shortest_distance(int src);
        void print_shortest_distance();

    private:
        int get_min_distance_vertex(const std::vector<bool>& spt_set);

        Graph graph_;
        std::vector<int> distance_;
};

#include "dijkstras.h"
#include <limits>

Dijkstras::Dijkstras(const Graph& graph) : graph_(graph) {
    distance_ = std::move(std::vector<int>(graph_.size(), std::numeric_limits<int>::max()));
}

int Dijkstras::get_min_distance_vertex(const std::vector<bool>& spt_set) {
    int min_dist = std::numeric_limits<int>::max();
    int index = -1;
    for (size_t i = 0; i < spt_set.size(); ++i) {
        if (!spt_set[i] && distance_[i] < min_dist) {
            min_dist = distance_[i];
            index = i;
        }
    }
    return index;
}

void Dijkstras::calculate_shortest_distance(int src) {
    int vertexes = graph_.size();
    distance_[src] = 0;
    std::vector<bool> spt_set(vertexes, false);
    for (int i = 0; i < vertexes; i++) {
        int index = get_min_distance_vertex(spt_set);
        spt_set[index] = true;
        for (int j = 0; j < vertexes; ++j) {
            if (!spt_set[j] && graph_[index][j]
                    && distance_[index] != std::numeric_limits<int>::max()
                    && distance_[index] + graph_[index][j] < distance_[j]) {
                distance_[j] = distance_[index] + graph_[index][j];
            }
        }
    }
}


void Dijkstras::print_shortest_distance() {
    std::cout << "Vertex\tDistanceFromSource" << std::endl;
    for (size_t i = 0; i < distance_.size(); ++i) {
        printf("%d\t%d\n", i, distance_[i]);
    }
}

#include "dijkstras.h"

const int V = 9;

int main(int argc, const char *argv[])
{
    std::vector<vector<int>> graph = {
        {0, 4, 0, 0, 0, 0, 0, 8, 0},
        {4, 0, 8, 0, 0, 0, 0, 11, 0},
        {0, 8, 0, 7, 0, 4, 0, 0, 2},
        {0, 0, 7, 0, 9, 14, 0, 0, 0},
        {0, 0, 0, 9, 0, 10, 0, 0, 0},
        {0, 0, 4, 0, 10, 0, 2, 0, 0},
        {0, 0, 0, 14, 0, 2, 0, 1, 6},
        {8, 11, 0, 0, 0, 0, 1, 0, 7},
        {0, 0, 2, 0, 0, 0, 6, 7, 0}
    };
    Dijkstras d(graph);
    d.calculate_shortest_distance(0);
    d.print_shortest_distance();

    return 0;
}

```

输出：
```
Vertex  DistanceFromSource
0       0
1       4
2       12
3       19
4       21
5       11
6       9
7       8
8       14
```

## 小结
1. 这个代码只是计算了最短的距离，但是没有计算路径。我们可以创建1个父数组，每次更新距离的时候更新父数组（就像prim算法的实现），然后用它来展现从原点到各个节点的最短路径
2. 这个代码只是无向图的，同样也能被用于有向图
3. 这个代码找出了从源节点到所有节点的最短路径。如果我们只是想找出源节点到某个目标节点的最短距离，我们的循环在挑选出的最短距离节点为目标节点的时候就可以终止了。
4. 这个实现的时间复杂度是O(V^2).如果输入的图是用邻接链表表示的，时间复杂度可降低为O(ElogV)，具体可以参见dijkstras的邻接链表实现
5. Dijkstras算法对于图中有负数权重的情况不适用。对于有负数权重的图，Bellman-Ford算法适用。

PS：文章来自于http://www.geeksforgeeks.org/greedy-algorithms-set-6-dijkstras-shortest-path-algorithm/的翻译

