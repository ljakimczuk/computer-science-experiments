# Task

Prove the presented algorithm prints greatest common divisor followed by lowest common multiple of $X$, for $X > 0$, and $Y$, for $Y > 0$.

```go
x,y,u,v := X,Y,Y,X
do
    x > y → x,v := x-y,v+u ▯
    x < y → y,u := y-x,u+v
od;
print((x+y)/2);
print((u+v)/2);
```

# Remarks

The algorithm is clearly an extension to the classic GCD algorithm, what can be seen after removing the $u$- and $v$- related parts, see below.

```go
x,y := X,Y
do
    x > y → x := x-y ▯
    x < y → y := y-x
od;
print((x+y)/2);
```

Since I've seen proofs of the GDC algorithm already I decided to focus on proving the LCM part only, especially I haven't seen this version ever before, taking the GDC part for granted, e.i. that $x = y = GCD(X,Y)$ when the algorithm terminates.


# Towards the Solution

My general idea was to find the loop invariant and prove the correctness with it.

For that reason I started at the end, so with the expression I needed to satisfy. Obviously, since $u = Y$ and $v = X$ at the beginning, and on the account that the only permitable operation is addition, both must be a mix of $X$ and $Y$ when the algorithm terminates, so as the sum $u + v$. Since we divide this sum by 2 at the end, this sum is expected to be equal to $2LCM(X,Y)$. Since the sum is combination of proportions of $X$, say $a$, and proportions of $Y$, say $b$, both $aX$ and $bY$ must be equal to each other and to $LCM(X,Y)$. Let's capture this by conditions $R1$ and $R2$.

$$
R1: {(u+v) \over 2} = LCM(X,Y) \Leftrightarrow (u+v) = 2 \times LCM(X,Y) = aX + bY
$$

$$
R2: LCM(X,Y) = aX = bY
$$

For the reason of $R1$ and $R2$ being a very concrete, or strong, I thought of weakening them, or making them dependant on the variable and hence hopefully easier to prove, so that these weakened forms on termination, when $x=y$, would be equal to the $R1$ and $R2$ respectively. My idea was to introduce arbitrary variable, say $t$, show that it can be combined into $R1$ and/or $R2$, and then to show, on assumption of this variable decreasing down to 0 on termination, the $R1$ and $R2$ are met.

$$
R1': (u+v) = kX + lY
$$

$$
R2': kX = lY + t
$$

This would show that at the end, when the algorithm terminates, we get $kX = lY$, and so the common multiple. What it does not show is that $kY$ is actually the lowest one possible.

This is where I met the first obstacle. Under the assumption of the algorithm working correctly, I couldn't wrap my head around to build an intuition of why it works. It looks like LCM is computed as co-product of GCD calculations, and I felt in order to find the suitable $t$ and prove the found multiple is the lowest, I need to understand what is going on, so I started dissecting $u$ and $v$.

On account on the previous observation, we know that both $u$ and $v$ are mix of $X$s and $Y$s through the course of algorithm.

$$
u + v = kX + lY
\land
v = k''X + l''Y \land u = k'X + l'Y
\land
k = k'' + k' \land l = l'' + l'
$$

At the beginning we obviously have $k'' = 1$, $k' = 0$, $l'' = 0$ and $l' = 1$. When the algorithm progresses we gradually increase four variables $k''X$, $k'X$, $l''Y$ and $l'Y$, so that at the end we have $kX = lY$. It still however does not tell of why the variables are manipulated the way they are.

After inpsection, it can be noticed that both, $x$ and $y$, are related to the newly identified variables.

$$
x = k''X - l''Y \land y = l'Y - k'X \Leftrightarrow kX = lY + x - y
$$

With this observation, the algorithm given can be rewritten a bit, dropping the GCD part entirely, to show of why it works for LCM.

```text
k",k',l",l' := 1,0,0,1
do
    (k"X - l"Y) > (l'Y - k'X) → k", l" := k"+k', l"+l' ▯
    (k"X - l"Y) < (l'Y - k'X) → k', l' := k'+k", l'+l"
od;
print((((k"+k')*X) + ((l"+l')*Y))/2);
```

So, intuitively the algoritm works by keeping track of differences $k''X - l''Y$ and $l'Y - k'X$. If first is greater than the other, the $k''$ and the $l''$ are increased by the $k'$ and the $l'$ respectively, making it smaller, because $l'Y - k'X$ is smaller, see below.
When it is smaller already, the process is reversed, i.e. the $k'$ and $l'$ are increased by the $k''$ and the $l''$ respectively, making the other one smaller. The process stops when both are euqal, meaning the $kX = lY$.

$$
k''X - l''Y = n > m = l'Y - k'X
$$

$$
(k'' + k')X - (l'' + l')Y = n - m < n
$$

This observation also find the $t$ I was looking for.

$$
t = x - y
$$

With this $R2'$ can be rewritten.

$$
R2': kX = lY + x - y
$$

This ensures that when the alrogithm terminates, i.e. when $x = y$, the common multiple is found, i.e. $kX = lY$. What it does not tell the common multiple is the lowest one. Same as before, I decided to try to find an invariant relation that holds through the course of algorithm, and somehow binds $X$, $Y$, $x$, $y$, $k$ and $l$ together, so that when the algorithm terminates it ensure what I am looking for.

First, I looked at properties of $LCM(X,Y)$ and $GCD(X,Y)$, which are taken here for granted without proving.

$$
aX = bY = LCM(X,Y)
$$

$$
{X \over b} = {Y \over a} = GCD(X,Y)
$$

$$
bGCD(X,Y) = X \land  a*GCD(X,Y) = Y
$$
