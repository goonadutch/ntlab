# Code :

```
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int bucket_size, output_rate, n;
    cout << "Enter bucket size: ";
    cin >> bucket_size;
    cout << "Enter output rate: ";
    cin >> output_rate;
    cout << "Enter number of packets: ";
    cin >> n;

    int storage = 0;

    for (int t = 1; t <= n; t++) {
        int pkt;
        cout << "\nEnter packet size for time " << t << ": ";
        cin >> pkt;

        cout << "Time " << t << ":\n";

        // Incoming packet handling && packet loss
        if (storage + pkt > bucket_size) {
            int dropped = (storage + pkt) - bucket_size;
            storage = bucket_size;
            cout << "  Packet loss = " << dropped << "\n";
        } else {
            storage += pkt;
            cout << "  Packet accepted\n";
        }

        cout << "  Buffer before leak = " << storage << "\n";

        // Leak at constant rate
        int leaked = min(storage, output_rate);
        storage -= leaked;

        cout << "  Leaked = " << leaked << ", Remaining = " << storage << "\n";
    }

    // Drain remaining packets after last input
    int time = n;
    while (storage > 0) {
        time++;
        int leaked = min(storage, output_rate);
        storage -= leaked;
        cout << "\nTime " << time << ":\n";
        cout << "  No new packets\n";
        cout << "  Leaked = " << leaked << ", Remaining = " << storage << "\n";
    }

    return 0;
}

```

# Output :

```
Enter bucket size: 10
Enter output rate: 3
Enter number of packets: 4

Enter packet size for time 1: 4
  Buffer before leak = 4
  Leaked = 3, Remaining = 1

Enter packet size for time 2: 6
  Buffer before leak = 7
  Leaked = 3, Remaining = 4

Enter packet size for time 3: 8
  Packet loss = 2
  Buffer before leak = 10
  Leaked = 3, Remaining = 7

Enter packet size for time 4: 5
  Packet loss = 2
  Buffer before leak = 10
  Leaked = 3, Remaining = 7

```

# Explanation : 

```

Bucket size (bucket_size) → maximum packets that can be stored.

Output rate (output_rate) → fixed leak per time unit.

Storage → current fill level of the bucket.

For each time step (loop):
  1. Add incoming packets (arrivals[t]).
  2. If overflow → drop excess packets.
  3. Leak packets at constant output_rate.
  4. Update remaining storage.

This matches the real leaky bucket behavior:
  - Controlled, constant output rate.
  - Packet drops if input exceeds capacity.
  - Leakage happens regardless of new arrivals.

```
