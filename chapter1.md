# No.1- leetcode 310.Minimum Height Trees

#### **Description**

For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees \(MHTs\). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

#### **Format**

The graph contains`n`nodes which are labeled from`0`to`n - 1`. You will be given the number`n`and a list of undirected`edges`\( each edge is a pair of labels\).

You can assume that no duplicate edges will appear in`edges` .Since all edges are undirected,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in`edges`.

#### **Example**

```
Input:n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]


     0  1  2
      \ | /
        3
        |
        4
        |
        5 


Output: [3, 4]
```

#### **Note**

* According to the [definition of tree on Wikipedia](https://en.wikipedia.org/wiki/Tree_%28graph_theory%29): “a tree is an undirected graph in which any two vertices are connected by
  exactly one path. In other words, any connected graph without simple cycles is a tree.”
* The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

#### Solution 1 

```
vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
    // if only one node, root is 0
    if (n == 1) return {0};

    // build a map marking the connections of nodes
    vector<set<int>> neighbors(n, set<int>({}));
    for (auto e: edges) {
        neighbors[e[0]].insert(e[1]);
        neighbors[e[1]].insert(e[0]);
    }

    // pick up all the leaves from the map
    vector<int> leaves;
    for (int i = 0; i < n; i++) {
        if (neighbors[i].size() == 1) leaves.push_back(i);
    }

    // remove "leaves" layer by layer, similar to BFS
    while (n > 2) {
        vector<int> newLeaves;
        for (int i: leaves) {
            // remove every leaf, with its connections
            int j =*(neighbors[i].begin());
            neighbors[j].erase(i);
            // find new leaves and store them for next action
            if (neighbors[j].size() == 1) newLeaves.push_back(j);
        }
        n -= leaves.size();
        // update leaves
        leaves = newLeaves;
    }
    return leaves;
}
```



