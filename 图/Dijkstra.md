# Dijkstra算法
### Dijkstra算法的实现策略
Dijkstra算法用来解决单源最短路径问题， 为了具体实现该算法，设置集合S存放已被访问的节点（即已访问的节点），然后执行n次下面的步骤（n为顶点个数）：
- 每次从集合V—S中选择与起点s的最短距离最小的一个顶点（记为u)，访问并加入集合S（即令其被访问）；
- 之后，以顶点u为中介点，优化起点s与所有从u能到达的顶点v之间的最短距离。
### Dijkstra算法的具体实现
- 算法的实现主要关注于两个东西，即集合S的实现、起点s到达顶点Vi(i<= i <=n)的最短距离的实现。
- 伪代码：
```c++
// G为图，一般设置为全局变量，数据d为源点到达各点的最短路径长度，s为起点
Dijkstra(G, d[], s)
{
    初始化;
    for(循环n次){
        u = 使d[u]最小并且还未访问的顶点的标号;
        记u已被访问;
        for(从u出发能够到达的所有顶点v){
            if(v未被访问&&以u为中介点使s到v的距离d[v]更优){
                优化d[v];
            }
        }
    }
}           
```  

由于图可以使用邻接表和邻接矩阵来实现，所以根据上述伪代码，可以有两种写法。  
写出具体函数之前，首先定义MAXV为最大顶点数，INF为一个很大的数。  
const int MAXV = 1000; //最大顶点数  
const int INF = 1000000000; //INF为一个很大的数  
- 邻接矩阵版实现  
适用于点数不大（如不超过1000的情况），代码如下
```c++
//邻接矩阵版
int n, G[MAXV][MAXV]; //n为顶点数，MAXV为最大顶点数
int d[MAXV]; //起点到达各点的最短路径长度
bool vis[MAXV] = { false }; //标记数组，vis[i]==true表示已访问，初值均为false

void Dijkstra(int s) {//s为起点
    fill(d, d + MAXV, INF); //fill函数将整个d数组赋值为INF
    d[s] = 0; //起点s到达自身的距离为0
    for (int i = 0; i < n; i++) {
        int u = -1, MIN = INF; //u使d[u]最小，MIN存放该最小的d[u]
        
        for (int j = 0; j < n; j++) {//找到未访问的顶点中d[]最小的
            if (vis[j] == false && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        }
        //找不到小于INF的d[u]，说明剩下的顶点和起点s不连通
        if (u == -1)
            return;
        vis[u] = true; //标记u已被访问
        for (int v = 0; v < n; v++) {
            //如果v未访问 && u能到达v && 以u为中介点可以使d[v]更优
            if (vis[v] == false && G[u][v] != INF && d[u] + G[u][v] < d[v])
                d[v] = d[u] + G[u][v]; //优化d[v]
        }
    }
}
```
- 邻接表版
```c++
struct Node {
    int v, dis; //v为边的目标顶点，dis为边权
};
vector<Node> Adj[MAXV]; //图G，Adj[u]存放从顶点u出发可以到达的所有顶点
int n; //n为顶点数，图G使用邻接表实现，MAXV为最大顶点数
int d[MAXV]; //起点到达各点的最短路径长度
bool vis[MAXV] = { false }; //标记数组，vis[i]==true表示已访问，初值均为false
//s为起点
void Dijkstra(int s) {
    fill(d, d + MAXV, INF); //fill函数将整个d数组赋值为INF
    d[s] = 0; //起点s到达自身距离为0
    for (int i = 0; i < n; i++) {
        int u = -1, MIN = INF; //u使d[u]最小，MIN存放该最小的d[u]
        //找到未访问的顶点中d[]最小的
        for (int j = 0; j < n; j++) {
            if (vis[j] == false && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        }
        //找不到小于INF的d[u]，说明剩下的顶点和起点s不连通
        if (u == -1)
            return;
        vis[u] = true; //标记u已被访问
        //只有下面这个for与邻接矩阵的写法不同
        for (int j = 0; j < Adj[u].size(); j++) {
            int v = Adj[u][v].v; //通过邻接表直接获得u能到达的顶点v
            //如果v未访问 && 以u为中介点可以使d[v]更优
            if (vis[v] == false && d[u] + Adj[u][j].dis < d[v])
                d[v] = d[u] + Adj[u][j].dis; //优化d[v]
        }
    }
}
```

