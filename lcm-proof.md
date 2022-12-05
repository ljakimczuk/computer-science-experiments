# Task

Prove the presented algorithm prints greatest common divisor followed by lowest common multiple of $X$, for $X > 0$, and $Y$, for $Y > 0$.

```go
x, y, u, v := X, Y, Y, X
for x != y {
    if x > y {
        x, v = x - y, v + u
    }

    if x < y {
        y, u = y - x, u + v
    }
}
fmt.Println((x + y) / 2)
fmt.Println((u + v) / 2)
```

# Remarks

The algorithm is clearly an extension to the classic GCD algorithm, what can be seen after removing the $u$- and $v$- related parts, see below.

```go
x, y := X, Y
for x != y {
    if x > y {
        x = x - y
    }
    if x < y {
        y = y - x
    }
}
fmt.Println((x + y) / 2)
```

Since I've seen proofs of the GDC algorithm already I decided to focus on proving the LCM part only, especially I haven't seen this version ever before, taking the GDC part for granted, e.i. that $x = y = GCD(X,Y)$ when the algorithm terminates.


# Towards the Solution

My general idea was to find the loop invariant and prove the correctness with it.

For that reason I started at the end, so with the expression I needed to satisfy.

$$
R: {(u+v) \over 2} = LCM(X,Y) \Leftrightarrow (u+v) = 2LCM(X,Y) = aX + bY
$$

$$
LCM(X,Y) = aX = bY
$$

So when the algorithm terminates the sum of $u+v$ must be equal to the $2LCM(X,Y)$. For the reason of $R$ being a very concrete, or strong, I thought of weakening it, or making it dependant on the variable and hence hopefully easier to prove, so that this weakened form $R'$ at termination, when $x=y$ would be equal to the $R$.

$$
R': kX = lY + t, t >= 0
$$

$$
R' \land t=0 \Leftrightarrow R
$$

This is where I met the first obstacle. Under the assumption of algorithm working correctly, I couldn't wrap my head around to build an intuition of why it works. It looks like LCM is computed as co-product of GCD calculations, and I felt in order to weaken the $R$ I need to understand what is going on, so I started dissecting $u$ and $v$.

Obviously, since $u = Y$ and $v = X$ at the beginning, both must be a mix of $X$ and $Y$ when the algorithm progresses as well, so as the sum $u + v$.

$$
u + v = kX + lY
\land
v = k''X + l''Y \land u = k'X + l'Y
\land
k = k'' + k' \land l = l'' + l'
$$

At the beginning we have $k'' = 1$, $k' = 0$, $l'' = 0$ and $l' = 1$. When the algorithm progresses we gradually increase four variables $k''X$, $k'X$, $l''Y$ and $l'Y$, so that at the end we have $kX = lY$. It still however does not tell of why the variables are manipulated the way they are.

After working out some examples, I noticed that both, $x$ and $y$, are related to the newly identified variables.

$$
x = k''X - l''Y \land y = l'Y - k'X \Rightarrow kX = lY + x - y
$$

With this observation, the algorithm given can be rewritten a bit, dropping the GCD part entirely, to show of why it works for LCM.

```go
/*
    kv = k''
    lv = l''
    ku = k'
    lu = l'
*/
kv, ku, lv, lu := 1, 0, 0, 1
for (kv*X - lv*Y) != (lu*Y - ku*X) {
    if (kv*X - lv*Y) > (lu*Y - ku*X) {
        kv, lv = kv+ku, lv+lu
    }
    if (kv*X - lv*Y) < (lu*Y - ku*X) {
        ku, lu = ku+kv, lu+lv
    }
}
fmt.Println(((kv + ku) * X + (lu + lv) * Y) / 2)
```

So, intuitively the algoritm works by keeping track of differences $k''X - l''Y$ and $l'Y - k'X$. If first is greater than the other, the $k''$ and the $l''$ are increased by the $k'$ and the $l'$ respectively, making it smaller, because $l'Y - k'X$ is smaller. When it is smaller already, the process is reversed, i.e. the $k'$ and $l'$ are increased by the $k''$ and the $l''$ respectively, making the other one smaller. The process stops when both are euqal, meaning the $kX = lY$.

$$
k''X - l''Y = n > m = l'Y - k'X
$$
$$
(k'' + k')X - (l'' + l')Y = n - m < n
$$
