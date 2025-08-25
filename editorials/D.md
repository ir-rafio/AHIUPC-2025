<details>
<summary>Problem D - Disruptor's Incapacitated Capacitor</summary>

Problem Setter: [Syed Rifat Raiyan](https://codeforces.com/profile/Starscream-11813)  
Estimated Difficulty: 1400  
Tag(s): Geometry

<details>

<summary>Hint 1</summary>

All strings behave the same due to symmetry.

</details>

<details>
<summary>Hint 2</summary>

Use the formula for the chord length of circle and Pythagorean theorem in 3D.

</details>

<details>
<summary>Solution</summary>

The problem is essentially geometric. We are dealing with two circular plates of a capacitor, each with radius $r$, connected by several strings of equal length $L$.

Due to symmetry, all strings behave the same and you can focus on a single string. When the $+ve$ plate is rotated by an angle $\theta$, we need to compute the resulting distance $d$ between the plates. Each string connects two corresponding points on the edges of the two plates. Before rotation, the endpoints of a string align along the same radius. After rotating the $+ve$ plate by $\theta$, the two endpoints of a string on the two plates are no longer aligned but are separated by an angular difference of $\theta$.

![Figure-D](./images/Figure-D.png)

Consider the two attachment points on the edges after rotation. Both points lie on a circle of radius $r$ centered at the axis of rotation, but separated by angle $\theta$. Hence, the distance $x$ between these two points is the chord length of a circle which can be obtained by the cosine rule of triangles as follows,

$x = \sqrt{r^2 + r^2 - 2r^2 \cos\theta} = \sqrt{2r^2 (1 - \cos\theta)}$

Now, each string forms the hypotenuse of a right triangle whose legs are:

- the distance between the two points on the $+ve$ plate's edge, $x$, and
- the distance between the plates, $d$.

Thus, by Pythagoras' theorem, $L^2 = d^2 + x^2$.

Substituting $x^2 = 2r^2(1 - \cos\theta)$ gives,

$L^2 = d^2 + 2r^2(1 - \cos\theta)$

Rearranging for $d$, we get,

$d = \sqrt{L^2 - 2r^2(1 - \cos\theta)}$

This is the required distance between the plates.

Be careful with angle units. Most programming languages expect trigonometric functions to use radians, not degrees.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define SQR(a)                  ((a)*(a))
#define Godspeed                ios_base::sync_with_stdio(0);cin.tie(NULL)
#define urs(r...)               typename decay<decltype(r)>::type
#define REP(i,b)                for(urs(b) i=0;i<b;i++)
#define all(a)                  a.begin(),a.end()
#define Bye                     return 0
#define ll                      long long
#define LD                      long double
#define PI                      acos(-1.0)

int main()
{
    Godspeed;
    int Tests=1;
    cin>>Tests;
    while(Tests--)
    {
        ll r,L,theta;
        cin>>r>>L>>theta;
        LD theta_rad=theta*PI/180.0;
        LD res=sqrt(SQR(L)-(2.0*SQR(r)*(1.0-cos(theta_rad))));
        cout<<fixed<<setprecision(7)<<res<<endl;
    }
    Bye;
}
```

</details>
</details>

<details>
<summary>Alternate Solution</summary>

An easy way to think about this problem is with cylindrical coordinates (polar coordinates with z-axis). Set up the coordinate system to have the centers of the positive and negative plate in the points $(0, 0, 0)$ and $(0, 0, L)$ respectively.

Without loss of generality, let $A(r, 0, 0)$ and $B(r, 0, L)$ be two points connected by a string.  
After rotating the positive plate by $\theta$, $A$ will go to $A'(r, \theta, z)$.  
In cartesian coordinates, $A'$ will be $(r\cos\theta, r\sin\theta, z)$.  
The new distance between the plates is $d = L - z$.

Now, $A'B = AB$  
$\implies \sqrt{(r\cos\theta - r)^2 + r^2\sin^2\theta + (z - L)^2} = L$  
$\implies r^2\cos^2\theta + r^2 - 2r^2\cos\theta  + r^2\sin^2\theta + d^2 = L^2$  
$\implies 2r^2 - 2r^2\cos\theta  + d^2 = L^2$  
$\implies 2r^2(1 - \cos\theta)  + d^2 = L^2$  
$\therefore d = \sqrt{L^2 - 2r^2(1 - \cos\theta)}$

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
using LL = long long;

long double PI = acosl(-1);



void pre()
{
    fastio;

    cout << fixed << setprecision(7);
}

void solve(int tc)
{
    int r, L, theta;
    cin >> r >> L >> theta;

    long double thetaRad = theta * PI / 180;
    long double d = sqrtl(1.0L * L * L - 2.0L * r * r * (1 - cosl(thetaRad)));
    cout << d;
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
</details>
