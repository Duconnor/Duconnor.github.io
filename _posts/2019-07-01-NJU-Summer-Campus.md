---
layout: post
title: 夏令营算法复习整理
date: 2018-07-10 16:19
catagories: algorithm
---

# 分类别复习
## 动态规划
Especially useful for **optimization problem**  
* General idea: recursion + memorization (memorize & reuse solutions to *subprolems* that help solve the problem)  
* **Time complexity**: number of subprolems times the number of calls in each subprolems to other subprolems(i.e. the time of each subprolems)  
* Smart DP: compute via the topological order of dependency graph  
* DP: recursion + memorization + guessing (at each subprolems, we do not know which one is the right/best choice, the number of guess we made dominate the running time)  
* Also, DP is like replicating the nodes in the original graph to a DAG which takes time into consideration of time. Therefore, finding the 'shortest' path in the original graph = finding the 'shortest' path in the new DAG
* DP: Divide into reasonable number of subprolems whose solutions are related acyclicly.  

### 5 Easy Steps to DP
1. Define subprolems
2. Guess
3. Relate subprolems solutions
4. Recurse + memorization / build DP table bottom up
5. Solve original problems

### Parent Pointers
If we want to have the actual solutions in addition to the minimum/maximum value, what should we do?  
We can use the memorization table to memorize each guessing steps' choice. And at the final step when we restoring the solutions, we iterate through the memorization table to get the result.

### Additional Subprolems
Sometimes there just no enough information when we simply define the subprolems. For example, in knapsack problem, if we define the subprolems according to the item size, there is no way to find out whether we can still hold the i'th item or not, so we need to define the subprolems as the combination of the index i and the pacakage size.  
Another example is in the Piano/Guitar Figering problem, where we will need to determine the finger we use on different notes. If we define the subprolems simply on the input size (i.e. the index of the notes), we won't be able to capture the difficulty of moving from this note to the next note. Thus, the proper subprolems should be the combination of figures we used now and the index of the notes.

### Commonly Used Subprolems Representation
Suffix, Prefix, Substring

### 背包问题：knapsack problem
1. 0-1背包问题，其问题描述为：给定一个容量为n的背包，有n个物体，其质量由数组m给出，价值由数组v给出，我们需要找到一种放置策略，使得装入背包的物体有着最大的价值。  
	* 转移方程为：d[i][j] = max(d[i - 1][j], (d[i - 1][j - m[i]]) + v[i])——（这里的d[i][j]表示的是只考虑前i个物品，容量为j的背包能够获得的最大价值）
	* 初始条件为：d[i][0] = 0, d[0][j] = 0（即当没有物品可用或者装不下任何物品时，价值为0，这是十分符合直观的）
	* 空间优化：可以看到dp时我们的转移方程第i次的取值只与第i-1次有关，因此可以只使用一维数组，但是需要注意，计算时应该从大到小计算，不然d[j - m[i]]表示的将是第i次的值而不是第i-1次
2. 完全背包问题，与0-1背包问题相同，只不过此时每种物品有无限多个  
	* 转移方程为：d[i][j] = max(d[i - 1][j - m[i]\*k]+v[i]\*k)(对于所有可行的k，从1开始)——也即我们每一步决定放多少个第i个物品
	* 初始条件为：d[i][0] = 0, d[0][j] = 0
	* 完全背包问题的优化：上述的转移方程显然需要三层循环，但是完全背包可以转化为0-1背包，即每次只需要决定第i个物品取还是不取，同时由于物品的是无限的，因此新的转移方程为：d[i][j] = max(d[i - 1][j], (d[i][j - m[i]]) + v[i])
	* 空间优化：上述问题同样可以压缩为一维数组进行存储，同时由于第i次访问时需要的内容是i-1次时的相同位置以及i次的前面位置，因此需要从小往大计算
3. 多重背包问题，与0-1背包也很类似，只不过此时每种物品的数量是有限个（但可能不止一个）  
	* 转移方程为：d[i][j] = max(d[i - 1][j - m[i]\*k]+v[i]\*k)(对于所有可行的k，从1开始)——也即我们每一步决定放多少个第i个物品，与完全背包的相同
	* 初始条件：d[i][0] = 0, d[0][j] = 0
	* 优化：多重背包同样可以优化为0-1背包，主要思路是将第i类物品，假设其数量为n[i]个，拆分为1+2+4+...+rest，然后对拆分后的问题按照0-1背包的思路求解（转移方程也相同），对于任何一个数，显然其都可以被拆分为2的幂次，因此对于取任意数量的第i类物品，都可以被表示
	* 空间优化：转化为0-1背包后即可转化为一维数组进行存储
4. 动态规划问题的基本步骤
	* 建立dic
	* 初始化
	* 根据转移方程，迭代或者递归计算dic中各项的值
	* 根据dic返回要求的值

### Fibonacci Number
传统的Fibonacci数的计算可以抽象为一颗树（根节点为Fn），计算需要花费指数级别的时间，但是可以发现计算时许多节点会被重复计算，于是如果将这些节点存储起来可以加速

### Shortest Path
状态转移方程为：dp[i][j]=min(dp[i][u]+p[u][j] | for all u that is adjacent to j)  
上述算法如果直接递归DP，那么对于有环图，算法将陷入死循环（有向无环图算法可以进行）——死循环的原因：subprolems之间可能存在环形的依赖关系——仔细观察上述转移方程，子问题可以用第二个下标来标示，如果问题j依赖于u，代表原图中存在从u到j的边，因此如果j依赖于u，u依赖于k，k又依赖于j，此时即原图中存在环，便会陷入无限循环  
解决无限循环的问题：新的子问题！上面出现无限循环是因为我们只考虑到达关系，不考虑中间的经过节点，因此考虑增加子问题的个数（常用的使其无环的方法），定义dp_k[i][j]为经过小于等于k条边从i到达j的距离，于是新的状态转移方程为：dp_k[i][j]=dp_{k-1}[i][u]+w[u][j]，显然之前陷入循环是因为又会再次调用同样的函数，但是此时因为每次递归k都会减一，因此不会再有循环。上述的初始条件为：dp_0[i][j]=0/infty、dp_k[i][i]==0，最后答案在dp_{|V|-1}[i][j]中  
时间复杂度：# of subprolems = |V| times |V|, time of each subprolems = indegrees(j)，因此总的time = O(V sum of indegrees of all vertices) = O(VE)

### Text Justification
定义text justification的badness为：b[i,j] = (length of the line - total length)^3 and infty if if exceeds the line length。于是DP的目标是最小化这一badness，定义DP[i]表示从第i个词开始，最小的badness，于是转移方程为：DP[i] = min(badness(i, j) + DP[j + 1]) for all avaliable j，初始条件为：DP[n] = 0。分析可以发现，每个DP依赖于其后面的值，于是应该从后往前计算，结论在DP[0]中。

### Parenthesization
1. Subprolems: multiplying sequence from i to j
2. Guessing: determine the last multiple position
3. Relation: dp[i][j] = min(cost(k) + dp[i][k] + dp[k][j]), dp[i][i + 1] = 0
4. Recusion/topological order: the length of the substr, from 1 to n
5. Original problem: dp[0][n]

### Edit Distance
1. Subprolems: suffix string starting from i and j (O(mn) # subprolems)
2. Guessing: for all non-same element, we choose to edit string x, and we can:
	1. Delete x[i]
	2. Insert y[j]
	3. Replace x[i] into y[j]
3. Relation: dp[i][j] = min
		(dp[i+1][j] + delete cost,
		dp[i][j+1] + insert cost,
		dp[i+1][j+1] + replace cost)
	dp[m - 1][n - 1]=1/0
4. Recursion/topological order: dependency coming from right, below, and the right down corner, so we compute from the bottom line, right to left
5. Original problem: dp[0][0]  

注意这个Edit Distance有几点需要注意，第一是对于relation中，replace cost可能是0（此时对应word1[i] == word2[j]），第二是在初始化时，dp[m - 1][n - 1]要根据word[m - 1]和word[n - 1]的情况初始化为1或0，第三是在迭代时，与其余DP不同的是，当迭代出DP的数组后，其也是一个可行的解（此时相当于只需要删除，因此花费就是delete cost \* 剩余的字符数）

### Piano/Guitar Figering
1. Subprolems: index of notes we are on and the figure we are using
2. Guessing: the next figer we will use
3. Relation: dp[i][f] = min(dp[i + 1][g] + cost(f,g)), dp[n-1][f]=0(for all f, this is the same as saying we have no difficulty moving our figers to play the first note because we can just play it!)
4. Recursion/topological order: the dependency coming from below, so we go bottom up
5. Original problem: min(dp[0][f], for all f)

## 搜索
### a^2+b^3+c^4=target
这种类型的题目目前为止没有看到有效的解决方法，直接几层for循环遍历即可。同时注意选取适当的遍历顺序可能可以进行加速（fancy word：剪枝）
剪枝方法：
* 根据取值跳过一部分搜索，或者根据要求选取适当的遍历顺序（只是要求求出一个解，或满足条件的某个解——例如字母序最大的解）
* 对于这类题目，一般会套很多层for循环，如果题目要求求出所有可能的解，那么可以采用meet in the middle的策略，即将式子对半划分，对于左半部分先遍历出其可能的所有取值，将取值与对应的出现次数放在一个HashMap中，然后再遍历右边部分，每得到一个结果（sum）时，看target-sum是否在刚刚的HashMap中，如果有，则直接加上出现次数（具体见Eqs这道题）

### 每次走一步/每次改数组或字符串中一个位置的元素
这类一步一步走的问题非常适合使用BFS/DFS来进行求解，尤其是问最少需要多少步时

### 搜索的范围是一个矩阵
如果搜索范围是个矩阵，那么天然的可以将其看作是一个图，在图上使用DFS/BFS是再正常不过的了

## 图
### Graph Search Application
1. Web Crawling
2. Social Network
3. Network Broadcast Routing
4. Garbage Collection
5. Model Checking
6. Checking Mathmatical Conjectures
7. Solving Puzzles and Games

### Graph Representation
1. Adjacent list: list[i] is all the vertices that connects to i
2. OOP: Vertex is a class, which has fields like its number and all the number of its neighbors
3. Incidence list: store all edges coming from vertex i

### BFS
不用队列的实现：使用两个数组，一个frontier、一个next，每次遍历frontier中的点，然后把其所有没有被访问过的节点加入next并**设置level**，然后将next拷贝到frontier中  
用队列的实现：使用队列，但是由于所有的节点都杂在一起了，我们需要同时将level和节点压入队列中  
时间复杂度：O(E)，如果要遍历所有节点，那么复杂度为O(V+E)
#### BFS的应用——最短路径
对于不带权的单源最短路径，可以直接使用BFS解决，即最短路径=level。如果要获得这一最短路径，采用上面说过的parent pointer思想，对于每个节点，在遍历到它时，将这一节点和其对应的父节点一起保存下来
### DFS
从某个节点开始，如果其neighbor节点未被访问过，则递归访问  
Tree edge, back edge, forward edge, cross edge  
时间复杂度：与BFS类似，如果只是一个节点则为O(E)，如果要遍历所有节点则为O(V+E)
#### DFS的应用——Cycle Detection
存在back edge——或者说在某个节点的访问仍在进行的时候再次访问到了它
#### DFS的应用——Job Scheduling/Topological Sort
DFS finish time的reverse  
正确性：对于u到v的有向边：
* u比v先访问到，那么显然，v应该先于u结束
* v比u先访问到，由于无环，显然无法从v到u，因此v先结束

#### DFS和BFS的应用——搜索
DFS和BFS除了上面提到的用法外，另用法是**进行搜索**。需要注意的是，如果题目中提到了要搜索出最短（或类似表达），则只能使用BFS进行搜索

#### DFS和BFS的应用——判断连通性
有时候我们需要知道某些节点是否与另外一些节点连同，或者是否与边节点等连通，可以使用DFS和BFS进行判断

#### DFS和BFS的应用——模拟
由于DFS和BFS都是每次搜索一步，因此对于“走路/移动”一类的题目，可以利用DFS和BFS进行系统的枚举

## 最短路径
This is an algorithm for **weighted graph** because for unweighted case, we can use BFS.

### Definition
* Shortest path weight from *u* to *v*:  
    * if exist such path, it is the minimum weight sum of all such path
    * otherwise, it is infinity

* Single source shortest path (s is the source):
    * d[v] (initially):
        * 0 if v=s
        * infinity otherwise
    * d[v] (finally):
        * min_d[s, v] (the weight of the shortest path from s to v)
    * at ant time, d[v] will be >= min_d[s, v], it will decrease when we find a shorter path to v

* Negative weight edges:
    * Natural in some application: logarithms are used as weight
    * Some algorithm may fail: Dijkstra
    * Negative weight cycle: undefined shortest path (we iterate through the negative weight cycle for as many time as we want)

### General Structure of S.P Algorithm
* Initialize: for all v, d[v] = infinity, and p[v] = NULL
* Main:
    * Let: d[s] = 0
    * Loop:
        * (Somehow) Select one edge (u,v), if d[v] > d[u] + w(u,v), we let d[v] = d[u] + w(u,v) and p[v] = u (this is called *relaxation operation*)
    * Until for all v, for all u, we have d[v] < d[u] + w(w,v)

### Complexity
Poor choice of edges may result in an exponential algorithm

### Optimal Substructure
The subpath of the shortest path is still the shortest (proved by contradiction)

### Triangle Inequality
For any u and v, for any x, we have shortest_path[u,v] <= shortest_path[u,x] + shortest_path[x,v]

### Shortest Path in DAG
* Topologically sort the DAG (if there is cycle, there is topological order)
* Do the relaxation operation on each vertex by their own outcoming edges in the topological order

### Dijkstra Algorithm (Assuming no negative edges)
* Maintain two sets, one for vertices we do not know the shortest path yet *U*, and one for vertices that we have already know the shortest path *S*.
* Loop:
    * Extract one *v* with the minimum shortest path estimate from *U*
    * Insert it into S
    * For all outcoming edges of *v*, select it to do relaxation operation

#### Dijkstra Complexity
* O(V) insert into queue
* O(V) extract min from queue
* O(E) relaxation operation

For array based implementation:
* O(1) for insert
* O(V) for extract min
* O(1) for relaxation
* Total: O(V+V*V+E)=O(V+E)

For heap based implementation:
* O(lgV) for insert and extract
* **O(lgV)** relation (need to adjust the heap)
* Total: O(V lgV+E lgV)

### Exponential v.s. Polynominal
* C(n) = C1 + C2 * (n - C3) and if C2 > 1, then we will get C(n) = nC1 + C2^n, which is an exponential algorithm (divide & explode)
* C(n) = C1 + C2 * (n / C3) and even if C2 > 1, if we have C3 > 1, it will an okay polynominal algorithm

### Bellman-Ford Algorithm
* For i = 1 to **|V| - 1**
    * For each edge (u,v), do relaxation operation
* For each edge (u,v)
    * If d[v] > d[u] + w(u,v) (not converage), then report a negative loop

#### Proof of Correctness
* If there is no negative weighted loop, after the |V| - 1 loop, every vertex will converage
    * Consider any vertex vk, and the shortest path to vk is [s,v1,v2,...,vk]. Because there are k edges within this path and k must be less than |V| - 1 since there is no negative weight loop. We will definetly find this path at the k'th pass.
    * 在第k轮被relax了的点，从源点到其的路径一定有k个边
* If there exist non-converage vertex after the |V| - 1 passes, there is sure to have negative weighted loop

### The Longesr Simple Path and The Shortest Simple Path Problem
Both are known to be NP-hard

### Speed Up Dijkstra

#### Bi-Directional Search
Will not change the worst case, but will reduce the number of visited vertices in practice

## Minimum Spanning Trees
### Kruskal
每次在所有边中，选取一条权值最小且不与当前的生成树形成回路的边加入

### Prim
每次在与当前的生成树连接，但不在生成树的节点中，选取一个与当前生成树连接的边权值最小的加入  
注意最好不要使用priority_queue，使用数组即可，这是因为尽管priority_queue可以在O(1)时间内选出最小，其无法更新内部元素的值，因此对于稠密图，可能最后队列会变得异常大，速度下降很快

### Prim和Kruskal的选择
稀疏图选择Kruskal，稠密图选择Prim

## Connected Component & Strong Component
### Connected Component
DFS/BFS进行遍历即可，每一次被遍历到的一组节点即属于同一个connected component

### Strong Component
#### Kosaraju-Sharir Algorithm
* DFS遍历图G的转置，求出各个节点的结束时间
* 根据上一步中各个节点的结束时间，从大到小（实际就是以图G的转置的topological order）DFS/BFS遍历图G

#### Tarjan Algorithm
对于某个顶点v，以其为起始点进行遍历，维护两个数组：dfn和low，维护一个栈
* 先令dfn[v]=low[v]=time，其中time每遍历一个顶点加一，并把v压入栈
* 对于v的所有邻接节点u
    * 如果u未被访问，则递归访问，在返回后取low[v]=min(low[v], low[u])
    * 如歌u被访问且在栈中，则low[v]=min(dfn[u], low[v])
    * 其它情况不进行操作
* 上述DFS步骤完成后，查看当前节点的low和dfn，如果相等，则从栈顶开始一直弹出元素，直到遇到v（v也弹出），弹出的所有元素组成一个strong component

## Maximum Matching
### 匈牙利算法
思想：不断寻找增广路径  
函数find为处于左边的顶点v寻找匹配
* 遍历右边的节点u
    * 如果u与v存在连线，且u没有请求更改匹配
        * 请求更改u的匹配，接下来判断
            * 如果u没有对应的匹配，直接匹配给v然后返回
            * 否则递归为u原来的匹配寻找新的匹配，如果找到则把u匹配给v，然后返回
            * 其余情况继续寻找

### Bipartite Graph
二分图一定是**无向图**  
判断是否为二分图：DFS+黑白染色  
判断二分图常常被用在需要将所有元素划分为两个部分的问题中，这时根据问题的具体描述，将不符合条件的两者之间连上边，符合条件的不连，然后再判断是否为二分图即可

## Maximum Flow
### 网络流的定义
一个源点，一个汇点，源点到汇点之间存在需要“水管”，需要考虑如何使得最终流入汇点的流量最大  
增广路径：一条从源点到汇点的

### Ford-Fulkerson算法
本质思想：不断寻找增广路径，扩大流  
* 将原图转化为Flow Network（即对于每一条有向边uv，将其权值设为c（容量），同时添加一条反向边，其权值为0（当前的flow））
* While(hasArgumentPath)——hasArgumentPath会修改数组last，其中last[v]表示从s到v的增广路径上的上一个点
    * 初始化bottle=infinity
    * 从汇点t开始，一直到源点s，取bottle为last[t]->t的权重与当前bottle的最小值
    * 从汇点t开始，一直到源点s，将last[t]->t的权重减小bottle，同时将t->last[t]的权重加bottle

#### hasArgumentPath的实现
使用BFS来实现  
* 初始化队列，将源点入队
* While(队列非空)
    * 取出队首元素v
    * 对于该元素的所有邻接顶点u，如果u未被访问且v->u的权重大于0，将u入队、修改last[u]=v并**将其标记为访问过（注意！这里以及其余利用了BFS的地方，一定都是先标记访问过再入队，否则的话可能造成节点重复入队，DFS则无所谓）**
* 如果汇点t被标记为已访问，那么代表找到了增广路径，返回true，否则，返回false
