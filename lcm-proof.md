# Task

Prove the presented algorithm prints greatest common divisor followed by lowest common multiple of X, for X > 0, and Y, for Y > 0.

```text
x, y, u, v := X, Y, Y, X;
do
    x > y --> x, v := x-y, v+u []
    x < y --> y, u := y-x, u+v
od;
print((x+y)/2); print((u+v)/2);
```
