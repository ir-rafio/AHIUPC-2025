

<summary>Problem A - Pacman vs. Vampire</summary>

Problem Setter: [Irfanur Rahman Rafio](https://codeforces.com/profile/Rafio)  
Estimated Difficulty: 1500  
Tag(s): Graph

<details>
<summary> Hint 1</summary>

The vampires know Pacman's moves.

</details>

<details>
<summary> Hint 2</summary>

Pacman should exit as soon as possible.

</details>

<details>
<summary> Hint 3</summary>

Find a way to count how many vampires can bite Pacman.

</details>

<details>
<summary> Hint 4</summary>

This problem has a crazy edge case.

</details>

<details>
<summary> Hint 5</summary>

What is the role of walls in this problem?

</details>

<details>
<summary>Solution</summary>

Since the vampires know Pacman's initial location and all moves (thus, the full path), there is no point for Pacman in trying to evade them. So Pacman's optimal strategy is to exit the game as soon as possible.

Each vampire is independent and can bite Pacman at most once. To bite Pacman, a vampire needs to occupy the same cell as Pacman at some point before Pacman exits the game. Now, consider the set of possible meeting points: the food cell (the cell containing the food), Pacman's position just before the food, the one before that, and so on, up to Pacman's starting cell.

If a vampire can meet Pacman at an earlier cell, then by following the same moves as Pacman, it can also meet Pacman at the food cell. The contrapositive is also true: if a vampire cannot reach the food cell in time, then it cannot meet Pacman at any earlier cell either. Therefore, the question "Can a vampire meet Pacman before it exits?" is equivalent to the simpler question "Can a vampire reach the food cell in time?"

This observation reduces the problem to comparing distances. Since both Pacman and the vampires move one cell per turn, a vampire can bite Pacman if and only if its distance to the food cell is less than or equal to Pacman's distance to the food cell.

To check this efficiently, you can run a BFS from the food cell. This gives the distance from the food to every other cell in the grid. Let $dP = \text{distance from Pacman's starting cell to the food}$ and $dV_i = \text{distance from the starting cell of the $i$-th vampire to the food}$. The $i$-th vampire can successfully bite Pacman if and only if $dV_i \le dP$.

Finally, Pacman always gains $+500$ points for eating the food, and loses $10$ points for each vampire that manages to bite. Let the number of vampires that can bite Pacman be $B$. The optimal score is: $500 - 10B$.

The following is a compact formulation of the inference:  
Optimal Score $= 500 - 10 \times |\\{ \text{Vampires that can bite Pacman in an optimal play} \\} |$  
 $= 500 - 10 \times |\\{ \text{Vampires that can meet Pacman before he exits} \\} |$  
 $= 500 - 10 \times |\\{ \text{Vampires that can meet Pacman at the food cell} \\} |$  
 $= 500 - 10 \times |\\{ \text{Vampires that can reach the food cell before Pacman or on the same turn} \\} |$  
 $= 500 - 10 \times |\\{ i : dV_i \le dP \\} |$

However, this is an **evil problem**. Every statement written till now had a hidden assumption: Pacman can reach the food and exit the game.  
However, because of the walls, this may not be true. It is possible that Pacman and the food are in separate components. In that scenario, all the vampires that are in the same component as Pacman can bite Pacman as the game won't end untill $10^7$ turns. The optimal score will then be: $-10B$.  
To find $B$, you can run DFS/BFS from Pacman's initial cell and count how many vampires are in the same component.

Time Complexity = $\mathcal{O}(nm)$

<details>

<summary>Code</summary>

```
Code
```

</details>
</details>
