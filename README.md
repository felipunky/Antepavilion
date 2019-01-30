# Antepavilion
Repository that contains a WIP for the [Antepavilion 2019 Commission](http://antepavilion.org/).
# Inspiration
We have been working on the computational design of the pavilion based on the work done by [Tomohiro Tachi](http://www.tsg.ne.jp/TT/)
![tachiinspiration](https://user-images.githubusercontent.com/21000020/51937630-1a710280-23d9-11e9-868e-15a2f83564ca.JPG)
![4115805713_f9bd253432_o](https://user-images.githubusercontent.com/21000020/51990496-24494300-2477-11e9-8141-68683ca12bb2.jpg)
![4115805815_8ff71a24e4_o](https://user-images.githubusercontent.com/21000020/51990497-24494300-2477-11e9-82eb-4e56050a1e57.jpg)
![4115805159_af0db68fbd_o](https://user-images.githubusercontent.com/21000020/51990498-24494300-2477-11e9-9c79-ef02929f84d2.jpg)
![4116574802_3f3e2dfd46_o](https://user-images.githubusercontent.com/21000020/51990499-24494300-2477-11e9-8f7b-a33d7066764f.jpg)

Our research lead us to create from a grid of points a discretized pattern that resembles the one done by Tachi. From the pattern, we could derive what the 2D pattern created in 3D.

![tachisfloorplan](https://user-images.githubusercontent.com/21000020/51937270-37f19c80-23d8-11e9-99b8-46d1d8126d76.JPG)

(Here, the mountains are blue, and the valleys are red, this is an important step in setting the Kangaroo Solver for the origami folding.)

![tachis](https://user-images.githubusercontent.com/21000020/51936773-f14f7280-23d6-11e9-9de1-5f0f5e4f6234.png)

(Mesh baked from Grasshopper to Rhino, than exported to Octane Standalone for rendering)

This taught us how to modify the points that compose the grid to generate something that is visually appealing. At first we manually modified the grid to create patterns, later we developed an algorithm that could create similar results through the use of a heuristic evolutionary solver.
# Tools
For the project we used the visual scripting environment for Rhino, Grasshopper. For the software we took advantage of the Paneling Tools plugin for managing the grid and some of the deformations and we used the physics solver Kangaroo for the folding of the origami pattern. We also used multiple native components and some scripted C#.
# Fitness function
As I mentioned earlier, we created a function that produces non-uniform designs from deformations of the grid and their relationships, here is an explanation:
The fitness algorithm is based on the relations between the length of the cells to each other, we subtract the distances from one another and apply an absolute value operator, every time we exceed the minimum and maximum thresholds, we subtract from the fitness number. The area of each of the faces( when the physics solver has run ) also accounts as a penalty, if it is lower than the threshold specified, we also subtract from the fitness. The fitness value specified is the area of the floor plan that we want. This enables the solver to produce results that are visually appealing. 
The genomes are the deformations that attractors such as points and nurbs curves produce through the magnitudes and translations that are discretized through sliders, the minimum length of the curve is also part of the fitness function as it forces the heuristic solver(Galapagos) to produce more deformation, we constrain the curve, by measuring its curvature and applying the same distances difference mentioned before.
# Iterations
Through the process we created multiple designs here are some examples:

This example can be found in this repository by the name PavilionOne.gh

This is the 2D floor plan before the origami folding:

![paviliononefloorplan](https://user-images.githubusercontent.com/21000020/51992645-61afcf80-247b-11e9-9ef5-bb1b944068fd.JPG)

In this iteration, we used the distances between the border branches to the central ones, we can see that we took the distances from point 0,... to point 1,... of the red and green lines, and we compared them against each other, if the difference between the two(absolute value) was smaller than 3.49 and bigger than 9.21, the function will not penalize(by penalize we mean that a counter will return -10000 and not zero).

![paviliononeexplanation](https://user-images.githubusercontent.com/21000020/51992643-61173900-247b-11e9-891d-bf4ddb9d1c0b.JPG)

We also compared the difference between the distances of the central "spine" points.

![paviliononeexplanation1](https://user-images.githubusercontent.com/21000020/51993394-c28bd780-247c-11e9-8954-7db7ce348572.JPG)

The minimum area of each of the faces was also taken into account, if it was lower than 23.6, we penalized the counter.

![paviliononeexplanation2](https://user-images.githubusercontent.com/21000020/51993659-480f8780-247d-11e9-818a-232eb337689d.JPG)

To force the genomes to create a curve(used as an attractor) that created more visually appealing results(more curvy) we defined a function that penalized a length lower than 50.

![paviliononeexplanation3](https://user-images.githubusercontent.com/21000020/51993932-c5d39300-247d-11e9-8fee-48fb3502ff12.JPG)

And for our last part of the fitness function, we took the area of the mesh in floor plan

![paviliononeexplanation4](https://user-images.githubusercontent.com/21000020/51994500-e9e3a400-247e-11e9-84fc-ec3f87a2f899.JPG)

This last step is really important as this is the number we will try to aproximate with the heuristic solver, in this case it was 1000.

(Images rendered with Redshift for Houdini)
![whatsapp image 2019-01-28 at 7 04 21 pm 1](https://user-images.githubusercontent.com/21000020/51989931-029b8c00-2476-11e9-8ede-dfe7fe9f9c43.jpeg)
![whatsapp image 2019-01-28 at 7 04 21 pm 2](https://user-images.githubusercontent.com/21000020/51989933-029b8c00-2476-11e9-95b1-1f91afcf959e.jpeg)
![whatsapp image 2019-01-28 at 7 15 03 pm](https://user-images.githubusercontent.com/21000020/51989935-03342280-2476-11e9-892c-ba065dd2ed1d.jpeg)

