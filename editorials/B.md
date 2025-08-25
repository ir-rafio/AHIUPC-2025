<summary>Problem B - Roman Empire</summary>

Estimated Difficulty: 900  
Tag(s): Implementation

<details>
<summary>Solution</summary>
The main observation required is: Each digit of a number directly maps to a substring of the Roman numeral representation.

The mapping depends on both the digit’s value and its positional place (ones, tens, hundreds, thousands, etc.). For example:
- The digit 9 in hundreds place maps to "CM".
- The digit 6 in tens place maps to "LX".

To form the complete Roman numeral, we simply concatenate the Roman substrings for each digit from left to right, according to their position.

<details>
<summary>Code</summary>
Recursive

```
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

string symbols[] = {"I", "V", "X", "L", "C", "D", "M", ""};

string getSymbol(int digit, int pos)
{
    string one = symbols[pos * 2], five = symbols[pos * 2 + 1];
    
    if(digit == 0) return "";
    if(digit == 9) return one + getSymbol(1, pos + 1);
    if(digit >= 5) return five + getSymbol(digit - 5, pos);
    if(digit == 4) return one + five;
    return one + getSymbol(digit - 1, pos);
}



void pre()
{
    fastio;

    
}

void solve(int tc)
{
    int n;
    cin >> n;

    cout << getSymbol(n / 1000, 3); n %= 1000;
    cout << getSymbol(n / 100, 2); n %= 100;
    cout << getSymbol(n / 10, 1); n %= 10;
    cout << getSymbol(n, 0);
}

int main()
{
    pre();

    int tc, tt = 1;
    cin >> tt;
    
    for(tc = 1; tc <= tt; tc++)
    {
        // cout << "Case " << tc << ": ";
        solve(tc);
        cout << '\n';
    }

    return 0;
}
```

Iterative

```
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

string digit[4][10] = {
    {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
    {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
    {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
    {"", "M", "MM", "MMM", "", "", "", "", "", ""}
};

int p10[4] = {1, 10, 100, 1000};



void pre()
{
    fastio;

    
}

void solve(int tc)
{
    int i, n, d;
    cin >> n;

    for(i = 3; i >= 0; i--)
    {
        d = (n / p10[i]) % 10;
        cout << digit[i][d];
    }
}

int main()
{
    pre();

    int tc, tt = 1;
    cin >> tt;
    
    for(tc = 1; tc <= tt; tc++)
    {
        // cout << "Case " << tc << ": ";
        solve(tc);
        cout << '\n';
    }

    return 0;
}
```

</details>
</details>