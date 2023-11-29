# 问题集一

## Part II

### Turtle graphics and the Logo language

[Logo](https://en.wikipedia.org/wiki/Logo_%28programming_language%29) is a programming language created at MIT that originally was used to move a robot around in space. Turtle graphics, added to the Logo language, allows programmers to issue a series of commands to an on-screen "turtle" that moves, drawing a line as it goes. Turtle graphics have also been added to many different programming languages, [including Python](https://docs.python.org/2/library/turtle.html) , where it is part of the standard library.

In the rest of problem set 0, we will be playing with a simple version of turtle graphics for Java that contains a restricted subset of the Logo language:

-   `forward(units)`
    Moves the turtle in the current direction by *units* pixels, where units is an integer. Following the original Logo convention, the turtle starts out facing up.

-   `turn(degrees)`
    Rotates the turtle by angle *degrees* to the right (clockwise), where degrees is a double precision floating point number.

You can see the definitions of these commands in `Turtle.java`.

**Do NOT use any turtle commands other than `forward`and `turn`in your code for the following methods.**

#### Problem 4: drawSquare

Look at the source code contained in `TurtleSoup.java`in package `turtle`.

Your task is to implement `drawSquare(Turtle turtle, int sideLength)`, using the two methods introduced above: `forward`and `turn`.

Once you've implemented the method, run the `main`method in `TurtleSoup.java`. The `main`method in this case simply creates a new turtle, calls your `drawSquare`method, and instructs the turtle to draw. Run the method by going to *Run → Run As... → Java Application* . A window will pop up, and, once you click the "Run!" button, you should see a square drawn on the canvas.

利用 forward 和 turn 方法画出一个正方形。乌龟一开始面是朝上的，我思考觉得这句话的意思是乌龟处在正方形的下边，这里我从左下角开始绘制图形，步骤如下：

1. 在当前方向行进边长距离然后停止
2. 向右转90°
3. 在当前方向行进边长距离然后停止
4. 向右转90°
5. 在当前方向行进边长距离然后停止
6. 向右转90°
7. 在当前方向行进边长距离然后停止
8. 绘制完成

```java
    double degree = 90.00;
    for (int i = 0; i < 4; i++) {
        turtle.forward(sideLength);
        turtle.turn(degree);
    }
```

### Problems 5—10: Polygons and headings

**For detailed requirements, read the specifications of each function to be implemented above its declaration in `TurtleSoup.java `. Be careful when dealing with mixed integer and floating point calculations.**

You should not change any of the *method declarations* ( [what’s a declaration? ](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)) below. If you do so, you risk receiving **zero points** on the problem set.

#### Drawing polygons（多边形）

- Implement `calculateRegularPolygonAngle`
  There’s a simple formula for what the inside angles of a regular polygon should be; try to derive it before googling/binging/duckduckgoing.

- Run the JUnit tests in `TurtleSoupTest`
  The method that tests `calculateRegularPolygonAngle `should now pass and show green instead of red.

  If `testAssertionsEnabled `fails, you did not follow the instructions in the Getting Started guide. [Getting Started step 2 ](https://ocw.mit.edu/ans7870/6/6.005/s16/getting-started/#config-eclipse)has setup you must perform before using Eclipse.

- Implement `calculatePolygonSidesFromAngle`
  This does the inverse of the last function; again, use the simple formula. However, **make sure you correctly round** to the nearest integer. Instead of implementing your own rounding, look at Java’s [`java.lang.Math `](https://docs.oracle.com/javase/8/docs/api/?java/lang/Math.html)class for the proper function to use.

- Implement `drawRegularPolygon`
  Use your implementation of `calculateRegularPolygonAngle `. To test this, change the `main `method to call `drawRegularPolygon `and verify that you see what you expect.

### Calculating headings

- Implement `calculateHeadingToPoint`
  This function calculates the parameter to `turn `required to get from a current point to a target point, with the current direction as an additional parameter. For example, if the turtle is at (0,1) facing 30 degrees, and must get to (0,0), it must turn an additional 150 degrees, so `calculateHeadingToPoint(30, 0, 1, 0, 0) `would return `150.0 `.
- Implement `calculateHeadings`
  Make sure to use your `calculateHeadingToPoint `implementation here. For information on how to use Java’s `List `interface and classes implementing it, look up [`java.util.List `](https://docs.oracle.com/javase/8/docs/api/?java/util/List.html)in the Java library documentation. Note that for a list of *n* points, you will return *n-1* heading adjustments; this list of adjustments could be used to guide the turtle to each point in the list. For example, if the input lists consisted of `xCoords=[0,0,1,1] `and `yCoords=[1,0,0,1] `(representing points (0,1), (0,0), (1,0), and (1,1)), the returned list would consist of `[180.0, 270.0, 270.0] `.