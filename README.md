# Triangulate 3D polygon

Triangulate3DPolygon is a header-only file offering a template-based solution for triangulating three-dimensional, non-complex polygons. This solution is self-contained, requiring no external libraries except for the inclusion of `<vector>`.

## Polygon Complexity
This routine/algorithm does not support complex polygons, i.e., polygons that self-intersect. However, the solution works for both **convex** and **concave** polygons. The routine does not produce new vertices to triangulate the polygon.

## Solution Overview
The solution uses two techniques for triangulation:

**Convex Polygons:** Fan triangulation method is utilized for convex polygons, efficiently dividing the polygon into triangles originating from a central point.

**Concave Polygons:** Earcut triangulation algorithm is employed for concave polygons. This method effectively identifies and cuts triangles from the inward-curving parts of the polygon, resulting in a triangulated mesh. Additionally, the earcut algorithm includes an extended solution called `getOverlappingEar` to handle cases where one earcut overlaps another triangle.
<br><br>

### Example with embedded struct for Point and Triangle
```cpp
#include <iostream>
#include "TriangulatePolygon.h"

int main()
{
    std::vector<triangulate::Point> polygon;

    polygon.emplace_back(0.0f, 0.0f, 0.0f);
    polygon.emplace_back(2.0f, 0.0f, 0.0f);
    polygon.emplace_back(2.0f, 2.0f, 0.0f);
    polygon.emplace_back(0.0f, 2.0f, 0.0f);
    
    const auto triangles = triangulate::triangulate(polygon);

    std::cout << "Triangles " << triangles.size() << std::endl;
}
```
<br>

### Example with user point and triangle structures
```cpp
#include <iostream>
#include "TriangulatePolygon.h"

struct point
{
    point() : x(0.), y(0.), z(0.) {}
    point(float x, float y, float z) : x(x), y(y), z(z) {}
    float x, y, z;
};

struct triangle
{
    triangle(const point& p0, const point& p1, const point& p2) : p0(p0), p1(p1), p2(p2) {}
    point p0, p1, p2;
};

int main()
{
    std::vector<point> polygon;

    polygon.emplace_back(0.0f, 0.0f, 0.0f);
    polygon.emplace_back(2.0f, 0.0f, 0.0f);
    polygon.emplace_back(2.0f, 2.0f, 0.0f);
    polygon.emplace_back(0.0f, 2.0f, 0.0f);

    const auto triangles = triangulate::triangulate<point, triangle>(polygon);

    std::cout << "Triangles " << triangles.size() << std::endl;
}
```
### Usage with Custom Point and Triangle Structures
If the user chooses to use their own custom `point` structure, it is important to adhere to the following guidelines:

- The custom point structure must have public variables `x`, `y`, and `z`, which can be of type `float` or `double`.
  
- The point structure should have a constructor similar to the example provided above, allowing initialization of `x`, `y`, and `z` values.

- The triangle structure should have a constructor similar to the example provided above, allowing initialization of `p0`, `p1`, and `p2` values.

- The solution does not rely on any operators such as equality or other member functions within the user-defined struct. This design ensures the robustness and stability of the solution, independent of the user's struct implementation and other user-defined epsilon values.

  
## References
The following sources have been utilized in the development of this Wavefront MTL parser.

[Paul Bourke: The shortest line between two lines in 3D](https://paulbourke.net/geometry/pointlineplane/)

[OpenGL Wiki: Calculating a Surface Normal - Newell's Method](https://www.khronos.org/opengl/wiki/Calculating_a_Surface_Normal)

[Wikipedia: Fan triangulation](https://en.wikipedia.org/wiki/Fan_triangulation)

[Wikipedia: Two ears theorem](https://en.wikipedia.org/wiki/Two_ears_theorem)

## License
This software is released under the MIT License terms.
