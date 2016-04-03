### Purpose ###
A 2D polygon library aiming to provide path-finding and lighting using **polygons** rather than **tiles**.

### Demos ###
[Java Applet Demo](http://www.keithwoodward.com/straightedge/)

[Java Web Start Demo](http://www.keithwoodward.com/straightedge/straightedge.jnlp)

### Description ###
Includes 2 main parts:
  * Path finding through 2D polygons using the A star algorithm and navigation-mesh generation
  * Field of vision / shadows / line of sight / lighting

The basic polygon and point classes are the KPolygon and KPoint. KPolygon contains a list of KPoints for vertices as well as a center (centroid), area, and radius (circular bound or distance from center to furthest point). KPolygon was born out of the need for a more game-oriented and flexible polygon class than the [Path2D](http://download.oracle.com/javase/6/docs/api/java/awt/geom/Path2D.html) class in the standard Java library. KPolygon implements java.awt.geom.Shape so it can be easily drawn and filled by Java2D's [Graphics2D](http://download.oracle.com/javase/6/docs/api/java/awt/Graphics2D.html) object.

This API provides path-finding and field-of-vision. For other complex geometric operations such as buffering (fattening and shrinking) and constructive area geometry (intersections and unions) it is recommended to use the excellent [Java Topology Suite (JTS)](http://tsusiatsoftware.net/jts/main.html). The [standard Java2D library](http://java.sun.com/products/java-media/2D/index.jsp) also provides the [Area](http://download.oracle.com/javase/6/docs/api/java/awt/geom/Area.html) class which can be used for some constructive area geometry operations. Note that there is a utility class PolygonConverter that can quickly convert KPolygons to JTS polygons and vice versa.

### Source code ###
Note that the source code is only available as a zip file in the [Downloads](https://code.google.com/p/straightedge/downloads/list) tab. The demo code is also included.

### About ###
StraightEdge was written by Keith Woodward as an independent hobby project. Project contributors are welcome.

### Feedback and Discussion ###
To discuss the project, ask questions, or provide feedback, please post a comment in the [discussion page](https://code.google.com/p/straightedge/wiki/DiscussionAndFeedback) or post a message in the [javagaming.org forum thread about this project](http://www.java-gaming.org/index.php/topic,23427.0.html). If you find a bug or have a feature request you can also [file an issue](https://code.google.com/p/straightedge/issues/list).

### Usage ###
Here are some code snippets to show you how to use the path-finding and lighting code. Click on the headings to see a complete self-contained example.

[Path-finding usage](https://code.google.com/p/straightedge/wiki/PathFindingExample):
```
// Make the obstacles which block our path
ArrayList<PathBlockingObstacle> stationaryObstacles = new ArrayList<PathBlockingObstacle>();
KPolygon polygon = KPolygon.createRectOblique(new KPoint(-1, 100), new KPoint(-1, 200), 50);
PathBlockingObstacle pathBlockingObstacle = PathBlockingObstacleImpl.createObstacleFromInnerPolygon(polygon);
stationaryObstacles.add(pathBlockingObstacle);

// Make the NodeConnector connect the obstacles' nodes (nodes are just the obstacle polygon's points).
NodeConnector nodeConnector = new NodeConnector();
double maxConnectionDistanceBetweenObstacles = 1000;
for (int k = 0; k < stationaryObstacles.size(); k++){
  nodeConnector.addObstacle(stationaryObstacles.get(k), stationaryObstacles, maxConnectionDistanceBetweenObstacles);
}

// Initialise the PathFinder
PathFinder pathFinder = new PathFinder();
KPoint startPoint = new KPoint(0, 0);
KPoint endPoint = new KPoint(0, 300);

// This is used to store the result:
PathData pathData = new PathData();

// Calculate the shortest path
double maxConnectionDistanceFromStartAndEndPointsToObstacles = maxConnectionDistanceBetweenObstacles;
pathData = pathFinder.calc(startPoint, endPoint, maxConnectionDistanceFromStartAndEndPointsToObstacles, nodeConnector, stationaryObstacles);

// Draw the result which is stored in pathData
ArrayList<KPoint> pathPoints = pathData.points;
Graphics2D g = (Graphics2D)someComponent.getGraphics();
g.setColor(Color.RED);
if (pathPoints.size() > 0){
  KPoint p = pathPoints.get(0);
  for (int i = 1; i < pathPoints.size(); i++) {
    KPoint p2 = pathPoints.get(i);
    g.draw(new Line2D.Double(p.x, p.y, p2.x, p2.y));
    float d = 5f;
    g.fill(new Ellipse2D.Double(p2.x - d / 2f, p2.y - d / 2f, d, d));
    p = p2;
  }
}
```

[Field-of-vision or lighting usage](https://code.google.com/p/straightedge/wiki/FieldOfVisionExample):
```
// Make the field of vision polygon
int numPoints = 20;
float radius = 250;
KPolygon boundaryPolygon = KPolygon.createRegularPolygon(numPoints, radius);

// Make the eye which is like the light-source
KPoint eye = new KPoint(0, 0);

// The VisionData just contains the eye and boundary polygon,
// and also the results of the VisionFinder calculations.
VisionData visionData = new VisionData(eye, boundaryPolygon);
VisionFinder visionFinder = new VisionFinder();

// Make the occluders which block our vision
ArrayList<Occluder> occluders = new ArrayList<Occluder>();
KPolygon polygon = KPolygon.createRectOblique(new KPoint(0, 100), new KPoint(0, 200), 50);
Occluder occluder = new OccluderImpl(polygon);
occluders.add(occluder);

// Calculate what's visible
visionFinder.calc(visionData, occluders);

// Draw the result which is stored in visionData
Graphics2D g = (Graphics2D)someComponent.getGraphics();
if (visionData.visiblePolygon != null){
  g.setColor(Color.RED);
  g.fill(visionData.visiblePolygon);
}
```

### Screenshots ###
Screen shots of StraightEdge in action:

![http://www.keithwoodward.com/straightedge/screenshotRandomPolygons.png](http://www.keithwoodward.com/straightedge/screenshotRandomPolygons.png)

![http://www.keithwoodward.com/straightedge/screenshotStoneHenge.png](http://www.keithwoodward.com/straightedge/screenshotStoneHenge.png)

![http://www.keithwoodward.com/straightedge/screenshotLetters.png](http://www.keithwoodward.com/straightedge/screenshotLetters.png)

![http://www.keithwoodward.com/straightedge/screenshotShapes.png](http://www.keithwoodward.com/straightedge/screenshotShapes.png)

![http://www.keithwoodward.com/straightedge/screenshotAll.png](http://www.keithwoodward.com/straightedge/screenshotAll.png)

![http://www.keithwoodward.com/straightedge/screenshotClipped.png](http://www.keithwoodward.com/straightedge/screenshotClipped.png)

![http://www.keithwoodward.com/straightedge/screenshotFog.png](http://www.keithwoodward.com/straightedge/screenshotFog.png)

![http://www.keithwoodward.com/straightedge/screenshotFogClipped.png](http://www.keithwoodward.com/straightedge/screenshotFogClipped.png)

![http://www.keithwoodward.com/straightedge/screenshotWireFrame.png](http://www.keithwoodward.com/straightedge/screenshotWireFrame.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldEnclosedKochCurve_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldEnclosedKochCurve_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldEnclosedKochCurve_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldEnclosedKochCurve_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldHexagonalGosperCurve_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldHexagonalGosperCurve_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldHexagonalGosperCurve_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldHexagonalGosperCurve_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldKochIsland_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldKochIsland_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldKochIsland_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldKochIsland_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldKochSnowflake_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldKochSnowflake_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldKochSnowflake_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldKochSnowflake_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldQuadraticGosperCurve_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldQuadraticGosperCurve_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldQuadraticGosperCurve_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldQuadraticGosperCurve_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldSierpinskiGasket_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldSierpinskiGasket_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldSierpinskiGasket_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldSierpinskiGasket_0.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldVogelSpiral_1.png](http://www.keithwoodward.com/straightedge/screenshotWorldVogelSpiral_1.png)

![http://www.keithwoodward.com/straightedge/screenshotWorldVogelSpiral_0.png](http://www.keithwoodward.com/straightedge/screenshotWorldVogelSpiral_0.png)

### Similar Projects ###

[Python pathfinding](https://github.com/CBCJVM/python-pathfinding): A pathfinding system, currently written in Jython compatible Python, with a goal to eventually be ported to Java and integrated with CBCJVM.
