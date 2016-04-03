## How it works: Field of Vision / Lighting ##

The heavy-lifting code is in the straightedge.geom.vision.VisionFinder class.
This is how it works:

  1. Make an empty list of points
  1. Add all polygon points that have an unobstructed line to the eye (or light source)
  1. Add all polygon intersection points that have an unobstructed line to the eye
  1. Add all points on the boundary polygon (or field of view) that have an unobstructed line to the eye
  1. Sort the list by the angle of each point relative to the eye, from the x-axis
  1. Search through the points and find any polygon points that are 'end points' which would cast a shadow.
  1. Cast a ray from these points away from the eye, and find the intersection of this ray with the nearest polygon or the nearest edge of the boundary polygon.
  1. Add the intersection to the list, before or after the 'end point' depending on which side the end point is on.

It took a lot of trial and error to arrive at this method. I tried lots of other ways but they weren't stable. This method works fine except for the rare cases when the eye is collinear with 2 or more polygon vertices. To minimise this:
  * the eye is prevented from penetrating a slightly larger polygon, which is created using JTS's buffer method. This prevents the eye from sitting on the edges of the polygon.
  * if the eye position is controlled by the mouse, so its coordinates are always integer coordinates, its x and y coordinates are offset by a small amount (say 0.0001) which helps to prevent collinearity.