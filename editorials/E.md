<summary>Problem E - Queen of Borderland</summary>

Problem Setter: [Akib Haider](https://codeforces.com/profile/_akibhaider_)  
Estimated Difficulty: 1300  
Tag(s): Combinatorics, Math

<details>
<summary>Hint</summary>

Solve for each color separately.

</details>

<details>
<summary>Solution</summary>

You need to find the number of combinations where, for each color, an even number of bottles remain. Since the condition for one color does not affect any other, you can solve the problem independently for each color and then multiply the results at the end.

Let the number of bottles of the $i$-th color be $n_i$. In a valid combination, the number of bottles that remain must be even. The number of ways to choose an even number of bottles is: $\displaystyle\binom{n_i}{0} + \binom{n_i}{2} + \binom{n_i}{4} + ... + \binom{n_i}{m}$, where $m$ is the largest even number such that $m \le n_i$ (that is, $m = n_i$ if $n_i$ is even, and $m = n_i - 1$ if $n_i$ is odd).

Time Complexity Analysis:  
For counting the combinations of the $i$-th color, you need $\mathcal{O}(n_i)$ operations.  
So, for all colors, the total is $\mathcal{O}(n_1 + n_2 + ... + n_{26}) = \mathcal{O}(n)$.  
Thus, the overall time complexity for a single round is $\mathcal{O}(n)$.  
The precalculation of factorials and inverse factorials (modular inverses of factorials) can be considered $\mathcal{O}(1)$.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

const LL MOD = 1e9 + 7;
const int N = 2e6 + 5;
LL fct[N], inv[N], invFct[N];

void initFct()
{
    fct[0] = 1LL;
    for(LL i = 1; i < N; i++) fct[i] = (fct[i - 1] * i) % MOD;
}

void initModInv()
{
    LL i, m, r;

    inv[1] = 1LL;
    for(i = 2; i < N; i++)
    {
        m = MOD / i, r = MOD % i;

        inv[i] = -m * inv[r];

        inv[i] %= MOD, inv[i] += MOD, inv[i] %= MOD;
    }
}

void initInvFct()
{
    invFct[0] = 1LL;
    for(LL i = 1; i < N; i++) invFct[i] = (invFct[i - 1] * inv[i]) % MOD;
}

LL nCr(int n, int r)
{
    return fct[n] * invFct[r] % MOD * invFct[n - r] % MOD;
}



void pre()
{
    fastio;

    initFct();
    initModInv();
    initInvFct();
}

void solve(int tc)
{
    LL i, n;
    string s;
    cin >> n >> s;

    vector<int> freq(26);
    for(char c: s) freq[c - 'a']++;

    LL ans = 1, t;
    for(auto x: freq) if(x)
    {
        t = 0;
        for(i = 0; i <= x; i += 2)
        {
            t += nCr(x, i);
            t %= MOD;
        }

        ans *= t;
        ans %= MOD;
    }

    cout << ans;
}

int main()
{
    pre();

    int tc, tt = 1;
    cin >> tt;

    for(tc = 1; tc <= tt; tc++)
    {
        solve(tc);
        cout << '\n';
    }

    return 0;
}
```

</details>
</details>

<details>
<summary>Alternate Solution</summary>
Let's try to break the process of choosing an even number of bottles out of $n$ into two steps $(n > 0)$:

1. Choose any combination from the first $(n - 1)$ bottles.
2. If the number of chosen bottles is even, then remove the last bottle; otherwise, keep it.

Step 1 can be done in $2^{n - 1}$ ways.  
Step 2 has exactly $1$ valid choice given Step 1.  
Therefore, the total number of ways is $2^{n - 1}$.

From combinatorics, this agrees with the identity:  
$\displaystyle\binom{n}{0} + \binom{n}{2} + \binom{n}{4} + ... + \binom{n}{m} = 2^{n - 1}$ for any $n > 0$ where $m$ is the largest even number such that $m \le n$.

For finding the value of $2^x$, you can use binary exponentiation, precalculate the powers of $2$ untill $10^6$, or even run a loop (since the sum of $n$ over all test cases is within $2 \times 10^6$).

The time complexity is $\mathcal{O}(n)$ because you have to build the frequency array.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

const LL MOD = 1e9 + 7;

LL binExp(LL a, LL p)
{
    if(p == 0) return 1;
    if(p & 1) return a * binExp(a, p - 1) % MOD;
    return binExp(a * a % MOD, p / 2);
}

void pre()
{
    fastio;
}

void solve(int tc)
{
    LL i, n;
    string s;
    cin >> n >> s;

    vector<int> freq(26);
    for(char c: s) freq[c - 'a']++;

    LL ans = 1, t;
    for(auto x: freq) if(x)
    {
        ans *= binExp(2, x - 1);
        ans %= MOD;
    }

    cout << ans;
}

int main()
{
    pre();

    int tc, tt = 1;
    cin >> tt;

    for(tc = 1; tc <= tt; tc++)
    {
        solve(tc);
        cout << '\n';
    }

    return 0;
}
```

</details>
</details>

<details>
<summary>Alternate Solution</summary>

Let $k$ be the number of colors with at least one bottle.  
$2^{n_1 - 1} \times 2^{n_2 - 1} \times ... \times 2^{n_k - 1}$ simplifies to $2^{n - k}$.

The time complexity is still $\mathcal{O}(n)$ because you have to count the value of $k$.

<details>
<summary>Code</summary>
#include <bits/stdc++.h>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

const LL MOD = 1e9 + 7;
const int N = 1e6 + 5;
LL pow2[N];


void pre()
{
    fastio;

    pow2[0] = 1;
    for(int i = 1; i < N; i++) pow2[i] = 2 * pow2[i - 1] % MOD;
}

void solve(int tc)
{
    LL i, n;
    string s;
    cin >> n >> s;

    vector<int> freq(26);
    for(char c: s) freq[c - 'a']++;

    int k = 0;
    for(auto x: freq) if(x > 0) k++;

    cout << pow2[n - k];
}

int main()
{
    pre();

    int tc, tt = 1;
    cin >> tt;

    for(tc = 1; tc <= tt; tc++)
    {
        solve(tc);
        cout << '\n';
    }

    return 0;
}
```

</details>
</details>
