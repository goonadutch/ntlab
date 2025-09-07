# Code

```
#include <bits/stdc++.h>
#define INF 1000
using namespace std;

class Router {
public:
    vector<int> distance;
    vector<int> from;

    Router(int n) {
        distance.resize(n, INF);
        from.resize(n, -1);
    }
};

int main() {
    int number_of_routers;
    cout << "Enter the number of routers: ";
    cin >> number_of_routers;

    vector<Router> routers;
    for (int i = 0; i < number_of_routers; i++) {
        routers.emplace_back(number_of_routers);
        routers[i].distance[i] = 0;
        routers[i].from[i] = i;
    }

    vector<vector<int>> cost_matrix(number_of_routers, vector<int>(number_of_routers, INF));
    for (int i = 0; i < number_of_routers; i++) {
        cost_matrix[i][i] = 0;
    }

    int u, v, w;
    while (true) {
        cout << "\nEnter source router (-1 to stop): ";
        cin >> u;
        if (u == -1) break;

        cout << "Enter destination router: ";
        cin >> v;
        cout << "Enter the cost: ";
        cin >> w;

        // convert to 0-based index
        u--; v--;

        routers[u].distance[v] = w;
        routers[v].distance[u] = w;
        routers[u].from[v] = v;
        routers[v].from[u] = u;

        cost_matrix[u][v] = w;
        cost_matrix[v][u] = w;
    }

    int count;
    do {
        count = 0;
        for (int i = 0; i < number_of_routers; i++) {
            for (int j = 0; j < number_of_routers; j++) {
                for (int k = 0; k < number_of_routers; k++) {
                    if (routers[i].distance[j] > cost_matrix[i][k] + routers[k].distance[j]) {
                        routers[i].distance[j] = routers[i].distance[k] + routers[k].distance[j];
                        routers[i].from[j] = k;
                        count++;
                    }
                }
            }
        }
    } while (count != 0);

    for (int i = 0; i < number_of_routers; i++) {
        cout << "\nRouting table for Router " << i + 1 << ":\n";
        for (int j = 0; j < number_of_routers; j++) {
            if (i == j) continue;
            cout << "To " << j + 1 << " via " << routers[i].from[j] + 1
                 << " with cost " << routers[i].distance[j] << "\n";
        }
    }

    return 0;
}

```
# Output

```
Enter the number of routers: 4

Enter source router (-1 to stop): 1
Enter destination router: 2
Enter the cost: 3

Enter source router (-1 to stop): 1
Enter destination router: 3
Enter the cost: 7

Enter source router (-1 to stop): 2
Enter destination router: 3
Enter the cost: 1

Enter source router (-1 to stop): 2
Enter destination router: 4
Enter the cost: 2

Enter source router (-1 to stop): 3
Enter destination router: 4
Enter the cost: 2

Enter source router (-1 to stop): -1

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
