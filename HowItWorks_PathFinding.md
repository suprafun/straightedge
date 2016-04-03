## How it works: Path finding ##

There are two major parts:
  1. Construct the 'navigation mesh' by linking the vertices of polygons together if they are straight-line reachable. This is done in the straightedge.geom.path.NodeConnector class.
  1. Find the shortest path between the start and end points using the [A-star algorithm](http://www.policyalmanac.org/games/aStarTutorial.htm) on the navigation mesh. This code is in the straightedge.geom.path.PathFinder class.

The major innovation of this project is in the construction of the navigation mesh by using **shrunk polygons**.

Every PathBlockingObstacle has two KPolygons and a list of Nodes. The first KPolygon is the outline of the obstacle which is called the outerPolygon. In the list of Nodes, there is one Node for every vertice of the outerPolygon.

There is also the innerPolygon, which is a slightly shrunk version of the outerPolygon. The Java Topology Suite has an excellent method polygon.buffer(double value) which is used to do the shrinking.

Each Node contains a list of other nodes that are straight line reachable. Two nodes are straight line reachable if a straight line between them is not intersected by the innerPolygon of any PathBlockingObstacle. Each node is tested for links to other nodes on the same obstacle, and to nodes on other obstacles. This is illustrated in the diagram below.

![http://www.keithwoodward.com/straightedge/PathBlockingObstacleDiagram.png](http://www.keithwoodward.com/straightedge/PathBlockingObstacleDiagram.png)

Note that the purple node poking inside the polygon is not connected. This is because it's a concave node and will not be useful for finding any path.

The links are calculated and stored once when NodeConnector.addObstacle(...) is called. This saves processor time so that the PathFinder doesn't need to always re-calculate the links between stationary obstacle nodes. However, the start and end points still need to be linked to the obstacle nodes every time since they are not stationary and could be anywhere. So these are tested for links to all nearby obstacle nodes on every call to PathFinder.calc(...).

Once all of these links are all calculated, there is a navigation mesh for the A-star algorithm to work on which is done in the PathFinder.calc(...) method.