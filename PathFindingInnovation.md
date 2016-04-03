# Introduction #

Found a way to speed up NodeConnector.reconnectNode methods.


# Details #

This is just a quick draft.

When connecting nodes I used to just connect all nodes that were reachable. By reachable, I mean that a line from one node to another is not intersected by any obstacles.

This led to lots and lots of connections. The thing is that not all of these connections are actually useful for finding a path between obstacles.

I found an efficient way to determine if a connection is useful or not. By ignoring useless connections, the nav-mesh is a lot smaller and faster to calculate, and the path finding algorithm works more quickly because there are less connections to process. On the demo maps, there was a decrease of between 80 to 95% in the number of connections.

To see the impact, look at these before and after screenshots:

Before, with 26,444 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressLetters26444.png](http://www.keithwoodward.com/straightedge/screenshotProgressLetters26444.png)

After, with only 1,735 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressLetters1735.png](http://www.keithwoodward.com/straightedge/screenshotProgressLetters1735.png)

Before, with 15,577 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressStarField15577.png](http://www.keithwoodward.com/straightedge/screenshotProgressStarField15577.png)

After, with only 3,024 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressStarField3024.png](http://www.keithwoodward.com/straightedge/screenshotProgressStarField3024.png)

Before, with 162,956 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressStoneHenge162956.png](http://www.keithwoodward.com/straightedge/screenshotProgressStoneHenge162956.png)

After, with only 21,888 node connections

![http://www.keithwoodward.com/straightedge/screenshotProgressStoneHenge21888.png](http://www.keithwoodward.com/straightedge/screenshotProgressStoneHenge21888.png)

Notice that the number of connections is drastically reduced.

I've been meaning to fully explain how a connection is deemed useful or not, but haven't had time. If you're really interested, maybe these pictures will help and you can check out the code in straightedge.geom.path.NodeConnector.isConnectionPossibleAndUseful().

![http://www.keithwoodward.com/straightedge/ConnectionDiagram1.png](http://www.keithwoodward.com/straightedge/ConnectionDiagram1.png)

![http://www.keithwoodward.com/straightedge/ConnectionDiagram2.png](http://www.keithwoodward.com/straightedge/ConnectionDiagram2.png)

![http://www.keithwoodward.com/straightedge/ConnectionDiagram3.png](http://www.keithwoodward.com/straightedge/ConnectionDiagram3.png)

![http://www.keithwoodward.com/straightedge/ConnectionDiagram4.png](http://www.keithwoodward.com/straightedge/ConnectionDiagram4.png)