# Task

Prove the presented algorithm prints greatest common divisor followed by lowest common multiple of $X$, for $X > 0$, and $Y$, for $Y > 0$.

```text
x, y, u, v := X, Y, Y, X;
do
    x > y --> x, v := x-y, v+u []
    x < y --> y, u := y-x, u+v
od;
print((x+y)/2); print((u+v)/2);
```

# Remarks

The algorithm is clearly an extension to the classic GCD algorithm, what can be seen after removing the $u$- and $v$- related parts, see below.

```text
x, y := X, Y
do
    x > y --> x := x-y []
    x < y --> y := y-x
od;
print(x)
```

Since I've seen proofs of the GDC algorithm already I decided to focus on proving the LCM part only, especially I haven't seen this version ever before, taking the GDC part for granted, e.i. that x = y = GDC(X,Y) when the algorithm terminates.


# Solution

Let's start at the end, so with the expressions:

$$
(u + v)/2 = LCM(X,Y)
$$
