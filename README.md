# Antepavilion
Repository that contains a WIP for the [Antepavilion 2019 Commission](http://antepavilion.org/).
# Inspiration
We have been working on the computational design of the pavilion based on the work done by [Tomohiro Tachi](http://www.tsg.ne.jp/TT/)
![tachiinspiration](https://user-images.githubusercontent.com/21000020/51937630-1a710280-23d9-11e9-868e-15a2f83564ca.JPG)

Our research lead us to create from a grid of points a discretized pattern that resembles the one done by Tachi. From the pattern, we could derive what the 2D pattern created in 3D.

![tachisfloorplan](https://user-images.githubusercontent.com/21000020/51937270-37f19c80-23d8-11e9-99b8-46d1d8126d76.JPG)

(Here, the mountains are blue, and the valleys are red, this is an important step in setting the Kangaroo Solver for the origami folding.)

![tachis](https://user-images.githubusercontent.com/21000020/51936773-f14f7280-23d6-11e9-9de1-5f0f5e4f6234.png)

(Mesh baked from Grasshopper to Rhino, than exported to Octane Standalone for rendering)

This taught us how to modify the points that compose the grid to generate something that is visually appealing. At first we manually modified the grid to create patterns, later we developed an algorithm that could create similar results through the use of a heuristic evolutionary solver.
# Tools
For the project we used the visual scripting environment for Rhino, Grasshopper. For the software we took advantage of the Paneling Tools plugin for managing the grid and some of the deformations and we used the physics solver Kangaroo for the folding of the origami pattern. We also used multiple native components and some scripted C#.
# Fitness function
As I mentioned earlier, we created a function that produces non-uniform designs from deformations to the grid and their relationships, here is an explanation:
The fitness algorithm is based on the relations between the length of the cells to each other, we subtract the distances from one another and apply an absolute value operator, every time we exceed the minimum and maximum thresholds, we subtract from the fitness number. The area of each of the faces( when the physics solver has run ) also accounts as a penalty, if it is lower than the threshold specified, we also subtract from the fitness. The fitness value specified is the area of the floor plan that we want. This enables the solver to produce results that are visually appealing. 
The genomes are the deformations that attractors such as points and nurbs curves produce through the magnitudes and translations that are discretized through sliders, the minimum length of the curve is also part of the fitness function as it forces the heuristic solver to produce more deformation, we constrain the curve, by measuring its curvature and applying the same distances difference mentioned before.
