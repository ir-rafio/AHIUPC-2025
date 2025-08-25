<details>
<summary>Problem B - Roman Empire</summary>

Estimated Difficulty: 900  
Tag(s): Implementation

<details>
<summary>Solution</summary>

To convert a number to a Roman numeral, you first need to understand its structure. The key is that Roman numerals, much like the natural number system, are based on **postional value**. If you take a number like $1248$, you think of it as $1000 + 200 + 40 + 8$. The Roman numeral system treats it the same way. The representation for $1000$ is M, for $200$ is CC, for $40$ is XL, and for $8$ is VIII. To get the final Roman numeral, you simply join these parts together: MCCXLVIII.

Each digit in a number corresponds to a specific Roman numeral substring. The substring for a digit '2' in the hundreds place (CC) is different from a '2' in the tens place (XX), so the digit's position is just as important as its value. The final result is always the **concatenation** of these substrings, from the largest postional value to the smallest.

Interestingly, the **pattern** for forming the numeral for any digit from 1 to 9 is universal. It just uses a different set of symbols depending on the postional value. For any given position (ones, tens, hundreds), you can identify a "one" symbol (I, X, C), a "five" symbol (V, L, D), and the "next one" symbol (X, C, M). This consistent pattern is what you can exploit in an algorithm.

The algorithm can process the input number one digit at a time, from left to right (thousands, then hundreds, and so on). The core of this process is a reusable procedure that can convert any single digit from 0-9 into its Roman numeral substring, given its postional value.

The logic for converting a single digit can be broken down into three distinct types of cases.

1. **The Subtractive Cases**: Digits 4 and 9 are special. They are formed by placing a smaller unit symbol before a larger one. For example, 4 is the "one" symbol before the "five" symbol (like IV or XL), and 9 is the "one" symbol before the "next one" symbol (like IX or XC). These are straightforward, one-step rules.

2. **The Additive Cases**: Digits 1, 2, and 3 are the simplest. They are formed by repeating the "one" symbol for that postional value one, two, or three times (e.g., III or XXX).

3. **The Additive Cases with Five**: Digits 5, 6, 7, and 8 all start with the "five" symbol. The number 5 is just the "five" symbol itself. For 6, 7, and 8, you start with the "five" symbol and then append the Roman numeral for the remainder (1, 2, or 3).

This algorithm can be cleanly implemented using recursion.

<details>
<summary>Code</summary>

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

</details>
</details>

<details>
<summary>Alternate Solution</summary>

A great alternative strategy for this problem is to use a precomputed **lookup table**. Instead of building the logic to figure out Roman numerals on the fly, you do the work ahead of time and store all the possible answers in a simple table. Since $n \le 1316$, you only need to work with at most $4$ digits.

With your lookup table ready, the algorithm to convert a number becomes very straightforward. You can write a simple loop that processes the input number from the highest place value down to the lowest.

The primary advantage of this method is its _simplicity_ and _reliability_. The program at runtime isn't 'thinking' about Roman numeral rules; it's just retrieving an answer that you've already prepared. Your runtime code becomes incredibly clean --- just a loop and an array lookup. This makes your code far less prone to bugs compared to a recursive function or a complex series of if-else statements.

<details>
<summary>Code</summary>

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


<details>
<summary>Bonus Problem</summary>

Try to solve the reverse problem. Given a roman numeral representation of a number, determine its value.

</details>
</details>
