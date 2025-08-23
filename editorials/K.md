<summary>Problem K - Kaboom!</summary>

Estimated Difficulty: 1200  
Tag(s): Binary Search, Interactive

<details>
<summary>Hint 1</summary>
How can you solve the problem with 1000 queries, instead of 20?

</details>

<details>
<summary>Hint 2</summary>
Use [Binary Search](https://en.wikipedia.org/wiki/Binary_search) to improve the solution from Hint 1.

</details>

<details>
<summary>Solution</summary>
In interactive problems, it is generally a good idea to observe which queries, along with their answers, give inambiguous information.

If a query results in NoBoom, We don't know if the interval has 0 special chemical, or 1. But if the result is Kaboom, We know for sure that this inverval contains both the special chemicals.

This leads to our solution for Hint 1. We can start with all the elements, namely the interval 1 to n, and shrink the right end one by one. The moment the query results in a "NoBoom", we can mark that as the position of chemical Y; assuming it is to the right of chemical X wlog. Chemical X can be found in a similar way, incresing the position of the left endpoint of our queries one by one.

To improve this solution to 20 queries, We observe the monotonality of Query answers. While finding Y, we notice all right endpoints greater than or equal to the position of Y results in "Kaboom", and others does not. Thus, we can efficiently find the position of Y by binary searching the leftmost position which results in a Kaboom, while keeping the left endpoint of our queries fixed at the beginning. position of X can be found similarly; and each search is bound to 10 queries each.

<details>
<summary>Code</summary>
    
```cpp
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

int ask(int l, int r)
{
    int i;
    string str;

    cout << "? " << r - l + 1 << ' ';
    for(i = l; i <= r; i++) cout << i << ' ';
    cout << endl;

    cin >> str;
    return str == "Kaboom";
}

void ans(int x, int y)
{
    cout << "! " << x << ' ' << y << endl;
}



void pre()
{
    fastio;

    
}

void solve(int tc)
{
    int x, y, n;
    cin >> n;
    
    int lo, hi, mid;

    lo = 1, hi = n;
    while(lo <= hi)
    {
        mid = (lo + hi) / 2;

        if(ask(1, mid)) hi = mid - 1;
        else lo = mid + 1;
    }
    y = lo;

    lo = 1, hi = n;
    while(lo <= hi)
    {
        mid = (lo + hi) / 2;

        if(ask(mid, y)) lo = mid + 1;
        else hi = mid - 1;
    }
    x = hi;

    ans(x, y);
}

int main()
{
    pre();

    int tc, tt = 1;
    // cin >> tt;
    
    for(tc = 1; tc <= tt; tc++)
    {
        // cout << "Case " << tc << ": ";
        solve(tc);
        // cout << '\n';
    }

    return 0;
}
```
</details>
</details>
