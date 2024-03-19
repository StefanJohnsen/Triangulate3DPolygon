# Triangulate 3D polygon

Triangulate3DPolygon is a header-only file offering a template-based solution for triangulating three-dimensional, non-complex polygons. This solution is self-contained, requiring no external libraries except for the inclusion of `<vector>`.

### Compatibility and Dependencies
- C++ 11 Standard and above
- Standard Template Library (STL)

### Supported polygons
The solution works for all kinds of 3D non-complex polygons, concave or convex, open or closed. The routine does not produce new vertices to triangulate the polygon.

### OS Support
- Windows
- Linux
- macOS

## Solution
The solution uses two techniques for triangulation:

**Convex Polygons:** Fan triangulation method is utilized for convex polygons, efficiently dividing the polygon into triangles originating from a central point.

**Concave Polygons:** Earcut triangulation algorithm is used for concave polygons. This method identifies and cuts triangles from the inward-curving parts of the polygon, resulting in a triangulated mesh. 

## Usage
Copy `TriangulatePolygon.h` to your project and include the file.

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
  
## References
The following sources have been utilized in the development of this solution.

[Paul Bourke: The shortest line between two lines in 3D](https://paulbourke.net/geometry/pointlineplane/)

[OpenGL Wiki: Calculating a Surface Normal - Newell's Method](https://www.khronos.org/opengl/wiki/Calculating_a_Surface_Normal)

[Wikipedia: Fan triangulation](https://en.wikipedia.org/wiki/Fan_triangulation)

[Wikipedia: Two ears theorem](https://en.wikipedia.org/wiki/Two_ears_theorem)

## License
This software is released under the MIT License terms.
