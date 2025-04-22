This is a collection of distance metrics that along with stable polar well and a few others will be used to devise a fast cheating GJK
distance. Likely, this will combine the concepts of key corruption from soft heaps with partial centroids  (centroids of Tris)

This means that it will be a key corrupted iterative barycentric L1 estimator, in all likelihood, and may god have mercy on my soul.

As the center of a tri is the 0 for its bary centric coordinate, the points of the tri can be discarded in favor of the distance from this
point at the cost of structural accuracy. Add a metric for preventing "long" triangles from being discarded, and you have a way to produce what
is effectively an inscribed surface with 1/3rd the complexity. 

Then, from there, repeat the process as needed. Next, draw the 3d voronoi.

Is this useful? This feels VERY useful.

I keep coming back to the idea that the centroid of each tri is its 0,0,0 in barycentric coordinates.
that feels really powerful. can we use this to build an alternative to the AABB tree? it feels like
there's a REALLY fast query structure in here.

so each "barry" layer is a layer in the tree. this gets you a... perfect query representation
with log(n) (base 3) performance? that should be impossible. I don't think it's wrong, though. it's sort of like a VP tree.

```c++
// Compute barycentric coordinates (u, v, w) for
// point p with respect to triangle (a, b, c)
void Barycentric(Point p, Point a, Point b, Point c, float &u, float &v, float &w)
{
Vector v0 = b - a, v1 = c - a, v2 = p - a;
float d00 = Dot(v0, v0);
float d01 = Dot(v0, v1);
float d11 = Dot(v1, v1);
float d20 = Dot(v2, v0);
float d21 = Dot(v2, v1);
float denom = d00 * d11 - d01 * d01;
v = (d11 * d20 - d01 * d21) / denom;
w = (d00 * d21 - d01 * d20) / denom;
u = 1.0f - v - w;
}
```
