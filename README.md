![Banner](banner.jpg)
# Triangulate 3D polygon

Triangulate3DPolygon is a header-only file offering a template-based solution for triangulating three-dimensional, non-complex polygons. This solution is self-contained, requiring no external libraries except for the inclusion of `<vector>` and `<cmath>`.

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

`TriangulatePolygon.h` is a single-header utility.

1. Copy `TriangulatePolygon.h` into your project
2. Include it where needed

```cpp
#include "TriangulatePolygon.h"
```

### Minimal example (quad)

A simple example that triangulates a 4-point polygon (a square):

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
Output:
```cpp
Triangles 2
```

## Practical example (larger concave polygon)

To demonstrate triangulation on a more challenging polygon, this repo includes a Wavefront OBJ file:
- falcon.obj — contains a single face/polygon representing a falcon silhouette
<img width="360" height="376" alt="image" src="https://github.com/user-attachments/assets/f3369d9d-010f-4122-a1b1-77eadb54c302" />


Download/copy falcon.obj to a location on your machine (e.g. C:\temp\falcon.obj) and run the examples below.

Loading the polygon from OBJ
The examples below use the WavefrontOBJ loader to read the polygon from falcon.obj.

- Dependency (separate repo): WavefrontOBJ
https://github.com/StefanJohnsen/WavefrontOBJ

Note: obj::loadPolygons(...) loads one or more polygons/faces into polygons.

```cpp
#include <iostream>
#include "WavefrontOBJ.h"
#include "TriangulatePolygon.h"

static std::string file = R"(C:\temp\falcon.obj)";

using namespace triangulate;

int main()
{
	std::vector<std::vector<Point>> polygons;

	if (obj::loadPolygons(file, polygons) == 0)
		return 1;

	const auto polygon = polygons[0];

	std::cout << "Polygon with " << polygon.size() << " points." << std::endl;

	const std::vector<Triangle<Point>> triangles = triangulate::triangulate(polygon);

	std::cout << "Polygon triangulated into " << triangles.size() << " triangles." << std::endl;

	return 0;
}
```
Output:
```bash
Polygon with 88 points.
Polygon triangulated into 86 triangles.
```

### Example B: Using your own point type (external struct/class)

You can triangulate polygons using your own point type T (struct/class).
Requirements:

- T must be constructible as: T(x, y, z)
- x, y, z can be float or double (match what you load / store)

Here is a minimal example point type:

```C++
struct TestPoint
{
	TestPoint() : x(0.f), y(0.f), z(0.f) {}
	TestPoint(const float& x, const float& y, const float& z) : x(x), y(y), z(z) {}
	float x;
	float y;
	float z;
};
```

Full example:

```C++
#include <iostream>
#include "WavefrontOBJ.h"
#include "TriangulatePolygon.h"

static std::string file = R"(C:\temp\falcon.obj)";

using namespace triangulate;

struct TestPoint
{
	TestPoint() : x(0.f), y(0.f), z(0.f) {}
	TestPoint(const float& x, const float& y, const float& z) : x(x), y(y), z(z) {}
	float x;
	float y;
	float z;
};

int main()
{
	std::vector<std::vector<TestPoint>> polygons;

	if (obj::loadPolygons(file, polygons) == 0)
		return 1;

	const auto polygon = polygons[0];

	std::cout << "Polygon with " << polygon.size() << " points." << std::endl;

	const std::vector<Triangle<TestPoint>> triangles = triangulate::triangulate(polygon);

	std::cout << "Polygon triangulated into " << triangles.size() << " triangles." << std::endl;

	return 0;
}

```
Output:
```bash
Polygon with 88 points.
Polygon triangulated into 86 triangles.
```
<br>

## Resulting Triangulation
<img width="426" height="426" alt="image" src="https://github.com/user-attachments/assets/85690a19-f115-42e7-8320-f274956f7c8a" />
  
## References
The following sources have been utilized in the development of this solution.

[Paul Bourke: The shortest line between two lines in 3D](https://paulbourke.net/geometry/pointlineplane/)

[OpenGL Wiki: Calculating a Surface Normal - Newell's Method](https://www.khronos.org/opengl/wiki/Calculating_a_Surface_Normal)

[Wikipedia: Fan triangulation](https://en.wikipedia.org/wiki/Fan_triangulation)

[Wikipedia: Two ears theorem](https://en.wikipedia.org/wiki/Two_ears_theorem)

## License
This software is released under the MIT License terms.
