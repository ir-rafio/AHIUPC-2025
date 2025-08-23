<summary>Problem I - Crisis in Flatland</summary>

Estimated Difficulty: 1800  
Tag(s): DP

<details>
<summary>Hint</summary>
It is easier to think of the problem as a graph problem at first and then move on to the dynamic programming solution. The given constraints are a hint to what might be the states of the dynamic programming.

</details>

<details>
<summary>Solution</summary>
First consider this problem to be a 3 state shortest path problem.


States: row, column, remaining time <br>
Target: Minimize the cost (DP value returns minimum cost) <br>
Number of possible states = 10<sup>12</sup>  (This is bad)

But wait, we can do better. The current issue is that the remaining time can be huge. Instead we can swap this state with the return value and the meaning changes to -> minimizing cost under a fixed time. The relationship between the cost and time is monotonic (Lower time implies higher cost. Higher time implies lower cost).

So now,<br>
States: row, column, remaining cost <br>
Target: Minimizing the time (DP value returns minimum time) <br>
Number of possible states: 10<sup>7</sup> (This is reasonable)

To find the answer, we simply iterate over the bounds of the time and try to find the minimum cost.

Current issue: Each state has O(n+m) number of transitions to new states (since from one spot, we need to traverse full column and full row). This will exceed the limit.

How to tackle this issue: We introduce a new state which represents the movement. This state may have 5 values representing (static, moving up, moving down, moving left, moving right). This reduced the number of transitions to 4 per state. If we are moving, we can either keep moving in that direction or stop. If we are static, we can pick any direction and start moving at that direction.

Are there cycles in this dynamic programming solution? No because ticket cost is atleast '1'. This means even if it is possible to reach the same position of row-column in different times, it will never have the same cost. 

Alternate solutions: With the same states and same transitions, a Dijkstra solution can be made which will have a slightly higher complexity by a factor of O(log(n)). This is not our intented solution and was supposed to give TLE but it seems in some cases it does pass.

<details>
<summary>Code</summary>

```cpp

#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
 
using namespace std;
using namespace __gnu_pbds;
using LL = long long;
 
#ifdef LEL
#include "dbg.h"
#else
#define dbg(...)
#endif
 
template <typename T> using ordered_set = tree <T, null_type, less<T>, rb_tree_tag,tree_order_statistics_node_update>;
mt19937_64 rnd(chrono::steady_clock::now().time_since_epoch().count());
 
int n, m;
int r1, c1, r2, c2;
 
int enc(int r, int c) {
    return (r - 1) * m + (c - 1);
}
 
LL t[1003];
int cost[1003];
LL dp[10004][1003][5];
 
const LL INF = 1e9 + 7;
 
LL f(int cst, int r, int c, int dir) {
    if(r < 1 or r > n or c < 1 or c > m or cst < 0) return INF;
    if(r == r2 and c == c2 and dir == 0) return 0;
 
    int cell = enc(r, c);
    LL &ans = dp[cst][cell][dir];
    if(ans != -1) return dp[cst][cell][dir];
 
    ans = INF;
 
    if(!dir) {
        cst -= cost[cell];
        ans = min(ans, 1 + f(cst, r + 1, c, 1));
        ans = min(ans, 1 + f(cst, r - 1, c, 2));
        ans = min(ans, 1 + f(cst, r, c + 1, 3));
        ans = min(ans, 1 + f(cst, r, c - 1, 4));
    } else {
        ans = f(cst, r, c, 0);
 
        if(dir == 1)      ans = min(ans, 1 + t[cell] + f(cst, r + 1, c, 1));
        else if(dir == 2) ans = min(ans, 1 + t[cell] + f(cst, r - 1, c, 2));
        else if(dir == 3) ans = min(ans, 1 + f(cst, r, c + 1, 3));
        else if(dir == 4) ans = min(ans, 1 + f(cst, r, c - 1, 4));
    }

    return ans;
}
 
int main() {
    cin.tie(0) -> sync_with_stdio(0);
    
    int T;
    cin >> T;
    while(T--) {
        LL x;
        cin >> n >> m >> r1 >> c1 >> r2 >> c2 >> x;
 
        int max_cost = 0;
 
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                int cell = enc(i, j);
                cin >> cost[cell];
                max_cost += cost[cell];
            }
        }
 
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                int cell = enc(i, j);
                cin >> t[cell];
                for(int d: {0, 1, 2, 3, 4}) {
                    for(int cst = 0; cst <= max_cost; cst++) {
                        dp[cst][cell][d] = -1;
                    }
                }
            }
        }
 
        dp[0][enc(r2, c2)][0] = 0;
 
        int ans = -1;
        for(int cst = max_cost; cst >= 0; cst--) {
            if(f(cst, r1, c1, 0) <= x) ans = cst;
        }
 
        cout << ans << '\n';
    }
}
```

</details>
</details>

<!-- Hint:
It is easier to think of the problem as a graph problem at first and then move on to the dynamic programming solution. The given constraints are a hint to what might be the states of the dynamic programming.

Solution:
First consider this problem to be a 3 state shortest path problem. 
States: row, column, remaining time
Target: Minimize the cost (DP value returns minimum cost)
Number of possible states = 1e12 (This is bad)

But wait, we can do better. The current issue is that the remaining time can be huge. Instead we can swap this state with the return value and the meaning changes to -> minimizing cost under a fixed time. The relationship between the cost and time is monotonic (Lower time implies higher cost. Higher time implies lower cost).

So now,
States: row, column, remaining cost
Target: Minimizing the time (DP value returns minimum time)
Number of possible states: 1e7 (This is reasonable)
To find the answer, we simply iterate over the bounds of the time and try to find the minimum cost.

Current issue: Each state has O(n+m) number of transitions to new states (since from one spot, we need to traverse full column and full row). This will exceed the limit.

How to tackle this issue: We introduce a new state which represents the movement. This state may have 5 values representing (static, moving up, moving down, moving left, moving right). This reduced the number of transitions to 4 per state. If we are moving, we can either keep moving in that direction or stop. If we are static, we can pick any direction and start moving at that direction.

Are there cycles in this dynamic programming solution? No because ticket cost is atleast '1'. This means even if it is possible to reach the same position of row-column in different times, it will never have the same cost. 

Alternate solutions: With the same states and same transitions, a Dijkstra solution can be made which will have a slightly higher complexity by a factor of O(log(n)). This is not our intented solution and was supposed to give TLE but it seems in some cases it does pass.
 -->