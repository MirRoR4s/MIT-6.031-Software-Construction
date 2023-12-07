# 问题集一

## Part II

### 乌龟图和 Logo 语言

实现以下两个方法控制乌龟的前进和转向：

- `forward(units)` 将乌龟在当前方向上移动 units 步，其中 units 是一个整数。根据 Logo 语言的惯例，乌龟的初始方向朝上。

- `turn(degrees)` 将乌龟的移动方向向右转 degress 角度（顺时针），其中 degress 是一个双精度浮点数。

两个方法的具体定义在 `Turtle.java`。**在接下来的方法中不要使用除了 `forward` 和 `turn` 以外的任何 turtle 包中的命令。**

#### Problem 4: drawSquare

查看 `turtle` 包下的 `TurtleSoup.java`，然后使用以上的 `forward` 和 `turn` 方法实现 `drawSquare(Turtle turtle, int sideLength)`。实现完毕之后运行 TurtleSoup.java 中的 main 方法绘制乌龟图。

朴素想法：利用 forward 和 turn 方法画出一个正方形。乌龟一开始面是朝上的，我思考觉得这句话的意思是乌龟处在正方形的下边，这里我从左下角开始绘制图形，步骤如下：

1. 在当前方向行进边长距离然后停止
2. 向右转 90°
3. 在当前方向行进边长距离然后停止
4. 向右转 90°
5. 在当前方向行进边长距离然后停止
6. 向右转 90°
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

- Run the JUnit tests in `TurtleSoupTest`
  The method that tests `calculateRegularPolygonAngle `should now pass and show green instead of red.

  If `testAssertionsEnabled `fails, you did not follow the instructions in the Getting Started guide. [Getting Started step 2 ](https://ocw.mit.edu/ans7870/6/6.005/s16/getting-started/#config-eclipse)has setup you must perform before using Eclipse.

- Implement `calculatePolygonSidesFromAngle`
  This does the inverse of the last function; again, use the simple formula. However, **make sure you correctly round** to the nearest integer. Instead of implementing your own rounding, look at Java’s [`java.lang.Math `](https://docs.oracle.com/javase/8/docs/api/?java/lang/Math.html)class for the proper function to use.

- Implement `drawRegularPolygon` Use your implementation of `calculateRegularPolygonAngle`. To test this, change the `main`method to call `drawRegularPolygon `and verify that you see what you expect.

### Calculating headings

<!-- - Implement `calculateHeadingToPoint` This function calculates the parameter to `turn` required to get from a current point to a target point, with the current direction as an additional parameter. For example, if the turtle is at (0,1) facing 30 degrees, and must get to (0,0), it must turn an additional 150 degrees, so `calculateHeadingToPoint(30, 0, 1, 0, 0)`would return `150.0`. -->
- 实现 calculateHeadingToPoint 函数，该函数计算从当前坐标到目的坐标需要转动的角度。举个例子，如果乌龟位于 (0,1) 且面向 30 度角，并且想要去到 (0,0)，那么乌龟必须转动 150 度，所以 `calculateHeadingToPoint(30, 0, 1, 0, 0)` 会返回 150.0。


思考：已知了初始方向，我们只需要把目的方向求出来，然后两方向（直线）的夹角就是所求的角度了。为了求目的方向，需要连接起点和终点
1. 当两条直线
2. 

- Implement `calculateHeadings`
  Make sure to use your `calculateHeadingToPoint ` implementation here. For information on how to use Java’s `List `interface and classes implementing it, look up [`java.util.List `](https://docs.oracle.com/javase/8/docs/api/?java/util/List.html)in the Java library documentation. Note that for a list of *n* points, you will return *n-1* heading adjustments; this list of adjustments could be used to guide the turtle to each point in the list. For example, if the input lists consisted of `xCoords=[0,0,1,1] `and `yCoords=[1,0,0,1] `(representing points (0,1), (0,0), (1,0), and (1,1)), the returned list would consist of `[180.0, 270.0, 270.0] `.