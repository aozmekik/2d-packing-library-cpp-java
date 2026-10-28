# 2D Shape Packing in C++ and Java

Two versions of the same program. Given a container shape and a smaller shape, it places as many copies of the small shape inside the container as its placement rules allow. Shapes are rectangles, circles and equilateral triangles, and all nine container/small-shape pairs are handled. The C++ version writes SVG files; the Java version draws in a Swing window.

The C++ code dates from October 2018 and the Java port from January 2019, according to the source headers.

<table>
<tr><th></th><th>Rectangles</th><th>Circles</th><th>Triangles</th></tr>
<tr><th>in a rectangle</th><td><img src="Cpp-Implementation/ComposedShape05.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape01.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape02.svg" width="200"></td></tr>
<tr><th>in a circle</th><td><img src="Cpp-Implementation/ComposedShape03.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape00.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape04.svg" width="200"></td></tr>
<tr><th>in a triangle</th><td><img src="Cpp-Implementation/ComposedShape07.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape08.svg" width="200"></td><td><img src="Cpp-Implementation/ComposedShape06.svg" width="200"></td></tr>
</table>

## How it works

`ComposedShape` holds a main (container) shape and a small shape. `optimalFit()` calls one of nine placement functions (`fitRinR`, `fitCinR`, `fitTinR`, `fitRinC`, `fitCinC`, `fitTinC`, `fitRinT`, `fitCinT`, `fitTinT`) and keeps the placed copies.

- **In a rectangle.** Rectangles go in a grid, in whichever orientation fits more copies; then the code tries to fill leftover space at the right or bottom edge with copies turned 90 degrees. Circles go in a square grid. Triangles go in rows, with a second set turned 180 degrees in the gaps.
- **In a circle.** The circle is cut into horizontal strips as tall as the small shape (its short side, for rectangles). Each strip's width comes from the circle equation, and the strip is then filled as if it were a rectangular container.
- **In a triangle.** Triangles go in rows that get shorter toward the top, again with a turned set in the gaps. Rectangles and circles use horizontal strips, each narrower than the one below, filled as rectangular containers.

Positions are the top-left corner for a rectangle, the center for a circle and the top corner for a triangle. Each shape class keeps running totals of area and perimeter. In C++ the shape classes also overload `++`/`--` (move by one unit in x and y) and the comparison operators (by area).

## C++ version

```sh
cd Cpp-Implementation
make        # builds ./hw with g++ -std=c++11
make run    # builds and runs ./hw
```

`main.cpp` fits circles of radius 50, 47x38 rectangles and triangles of side 37 into a circle of radius 500, an 800x600 rectangle and a triangle of side 600. It prints the results of the shape class tests and writes these files to the working directory:

- `ComposedShape00.svg` to `ComposedShape08.svg`: the nine pairs shown above
- `ComposedShape+=10.svg`: circles in the triangle, plus one more circle added with `operator+=`
- `Rectangle.svg`, `Triangle.svg`: single shapes

Each SVG holds the container in white and the placed shapes in cornflower blue.

## Java version

```sh
cd Java-Implementation
make        # compiles the sources with javac -g
make run    # java Main
```

`Main` prints the areas of five shapes before and after sorting them, then opens a 1000x1000 window. As committed, the window (`DrawPanel`) draws those five shapes; the `ComposedShape` block in `DrawPanel.paintComponent` is commented out. To draw a packing, enable that block and pass it the container and the small shape. Screenshots of packings:

<table>
<tr>
<td><img src="Java-Implementation/Image01.png" width="160" alt="Rectangles in a rectangle"></td>
<td><img src="Java-Implementation/Image02.png" width="160" alt="Circles in a rectangle"></td>
<td><img src="Java-Implementation/Image03.png" width="160" alt="Triangles in a rectangle"></td>
<td><img src="Java-Implementation/Image04.png" width="160" alt="Circles in a circle"></td>
</tr>
<tr>
<td><img src="Java-Implementation/Image05.png" width="160" alt="Rectangles in a circle"></td>
<td><img src="Java-Implementation/Image06.png" width="160" alt="Rectangles in a triangle"></td>
<td><img src="Java-Implementation/Image07.png" width="160" alt="Circles in a triangle"></td>
<td><img src="Java-Implementation/Image08.png" width="160" alt="Triangles in a triangle"></td>
</tr>
</table>

The Java code adds a `Shape` interface (area, perimeter, `compareTo` by area, `draw(Graphics)`) that every shape and `ComposedShape` implement; an abstract `Polygon` with `PolygonDyn` (points in an array) and `PolygonVect` (points in an `ArrayList`), where a circle becomes a 100-point polygon; helpers in `Global` (`drawAll`, `convertAll`, `sortShape`); and a `BadSize` exception for zero or negative sizes. The class diagram is [Java-Implementation/UML_PROJ/output.png](Java-Implementation/UML_PROJ/output.png).

## Notes

- `Java-Implementation/JavaDocs/` is generated javadoc output (javadoc 11.0.1). Its `jquery/` folder holds the jQuery, jQuery UI and JSZip files that come with it.

## License

MIT, see LICENSE.
