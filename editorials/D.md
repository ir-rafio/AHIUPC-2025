<summary>Problem D - Disruptor's Incapacitated Capacitor</summary>

Problem Setter: [Syed Rifat Raiyan](https://codeforces.com/profile/Starscream-11813)  
Estimated Difficulty: 1400  
Tag(s): Geometry

<details>

<summary>Hint 1</summary>

Focus on a single string; all strings behave the same due to symmetry.

</details>

<details>
<summary>Hint 2</summary>

After rotation by $\theta$, how far apart are the two attachment points around the $+ve$ plate? Think about the chord length of a circle of radius $r$.

</details>

<details>
<summary>Hint 3</summary>

Once you know this lateral distance, combine it with the unknown plate separation $d$ and the known string length $L$ using the Pythagorean theorem in 3D.

</details>

<details>
<summary>Hint 4</summary>
Be careful with angle units. Most programming languages expect trigonometric functions to use radians, not degrees.

</details>

<details>
<summary>Solution</summary>

The problem is essentially geometric. We are dealing with two circular plates of a capacitor, each with radius $r$, connected by several strings of equal length $L$. When the $+ve$ plate is rotated by an angle $\theta$, we need to compute the resulting distance $d$ between the plates. Each string connects two corresponding points on the edges of the two plates. Before rotation, the endpoints of a string align along the same radius. After rotating the $+ve$ plate by $\theta$, the two endpoints of a string on the two plates are no longer aligned but are separated by an angular difference of $\theta$.

![dic_pic](https://raw.githubusercontent.com/ir-rafio/AHIUPC-2025-Problemset/refs/heads/main/images/DIC_solution4.png)

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
