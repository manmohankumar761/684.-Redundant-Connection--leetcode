# 684.-Redundant-Connection--leetcode
In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.

Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.

 

Example 1:


Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
Example 2:


Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
Output: [1,4]
# code
#include <iostream>
#include <vector>
using namespace std;

class Solution {
    vector<int> parent;
    vector<int> rank;

    int find(int i) {
        if (parent[i] != i)
            parent[i] = find(parent[i]); // Path compression
        return parent[i];
    }

    void join(int u, int v) {
        int rootU = find(u), rootV = find(v);
        if (rootU != rootV) {
            if (rank[rootU] > rank[rootV]) parent[rootV] = rootU;
            else if (rank[rootU] < rank[rootV]) parent[rootU] = rootV;
            else {
                parent[rootV] = rootU;
                rank[rootU]++;
            }
        }
    }

public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        parent.resize(n + 1);
        rank.resize(n + 1, 0);
        
        for (int i = 1; i <= n; i++) parent[i] = i;

        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            if (find(u) == find(v)) return edge; // Cycle detected!
            join(u, v);
        }
        
        return {}; // Unreachable for valid inputs
    }
};
