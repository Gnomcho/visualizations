# Catmull-Rom Splines

**Author:** Llew Mason  
**Source:** comp.graphics.algorithms, 30 Jun 1994

---

> _Does anyone know how draw a curve line when a set of given points. I use the spline but it did not go through these given points. Does anyone have an example I can look at?_

The easiest way I can think of would be to use a **Catmull-Rom spline**.

## Matrix Form

For a point `q` on the curve at `t`:

```
q(t) = 0.5 * [ t³  t²  t  1 ] * [ -1   3  -3   1 ]   [ p1 ]
                                [  2  -5   4  -1 ] * [ p2 ]
                                [ -1   0   1   0 ]   [ p3 ]
                                [  0   2   0   0 ]   [ p4 ]
```

Where `p1`, `p2`, `p3`, `p4` are points along the curve.

> **Note:** The curve drawn only passes through points `p2` and `p3`.

_(That was a constant × 1×4 matrix × 4×4 matrix × 4×1 matrix)_

## Expanded Form

This becomes:

```
q(t) = 0.5 * ((-p1 + 3*p2 - 3*p3 + p4) * t³
            + (2*p1 - 5*p2 + 4*p3 - p4) * t²
            + (-p1 + p3) * t
            + 2*p2)
```

## Properties

- For values of `t` between 0 and 1, the curve passes through `p2` at `t=0` and through `p3` at `t=1`.
- The tangent vector at a point `P` is parallel to the line joining `P`'s two surrounding points.

## Drawing Multiple Points

To draw more than two points, step through the array of points using:

- The **previous point**
- The **current point**
- The **next two points**

as the four points for the spline. For each segment, draw a curve for `0 < t < 1`. This curve will be between the current point and the next point.

## Performance Note

This method is not particularly fast. A faster method for display of this class of splines is given in:

> Barry, P. and R. Goldman. _"A Recursive Evaluation Algorithm for a Class of Catmull-Rom Splines."_ SIGGRAPH 89.
