# Code

```
#include <bits/stdc++.h>
using namespace std;

#define INF 1000

int minDistance(vector<int>& dist, vector<bool>& visited, int n) {
    int minVal = INF, minIndex = -1;
    for (int v = 0; v < n; v++) {
        if (!visited[v] && dist[v] <= minVal) {
            minVal = dist[v];
            minIndex = v;
        }
    }
    return minIndex;
}

void dijkstra(int src, vector<vector<int>>& graph, int n, vector<int>& dist, vector<int>& parent) {
    dist.assign(n, INF);
    parent.assign(n, -1);
    vector<bool> visited(n, false);
    dist[src] = 0;

    for (int count = 0; count < n - 1; count++) {
        int u = minDistance(dist, visited, n);
        if (u == -1) break;
        visited[u] = true;
        for (int v = 0; v < n; v++) {
            if (!visited[v] && graph[u][v] && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }
}

int getNextHop(int src, int dest, vector<int>& parent) {
    if (parent[dest] == -1) return -1;
    int hop = dest;
    while (parent[hop] != src) {
        hop = parent[hop];
        if (hop == -1) return -1;
    }
    return hop;
}

int main() {
    int number_of_routers, number_of_links;
    cout << "Enter the number of routers: ";
    cin >> number_of_routers;

    vector<vector<int>> graph(number_of_routers, vector<int>(number_of_routers, 0));

    cout << "Enter the number of links: ";
    cin >> number_of_links;

    cout << "Enter each link in the format <source> <destination> <cost>\n";
    for (int i = 0; i < number_of_links; i++) {
        int u, v, w;
        cout << "Link " << i + 1 << ": ";
        cin >> u >> v >> w;
        u--; v--;
        graph[u][v] = w;
        graph[v][u] = w;
    }

    for (int src = 0; src < number_of_routers; src++) {
        vector<int> dist, parent;
        dijkstra(src, graph, number_of_routers, dist, parent);
        cout << "\nRouting table for Router " << src + 1 << ":\n";
        for (int dest = 0; dest < number_of_routers; dest++) {
            if (src == dest) continue;
            int nextHop = getNextHop(src, dest, parent);
            cout << "To " << dest + 1 << " via ";
            if (nextHop == -1) cout << "-";
            else cout << nextHop + 1;
            cout << " with cost " << dist[dest] << "\n";
        }
    }
    return 0;
}

```

# Output

```
Enter the number of routers: 4
Enter the number of links: 5   
Enter each link in the format <source> <destination> <cost>
Link 1: 1 2 3
Link 2: 1 3 7
Link 3: 2 3 1
Link 4: 2 4 2
Link 5: 3 4 2

Routing table for Router 1:
To 2 via 2 with cost 3
To 3 via 2 with cost 4
To 4 via 2 with cost 5

Routing table for Router 2:
To 1 via 1 with cost 3
To 3 via 3 with cost 1
To 4 via 4 with cost 2

Routing table for Router 3:
To 1 via 2 with cost 4
To 2 via 2 with cost 1
To 4 via 4 with cost 2

Routing table for Router 4:
To 1 via 2 with cost 5
To 2 via 2 with cost 2
To 3 via 3 with cost 2

```
