# GPU-Programming
The code that you create will take the simple examples we provide and modify them to carry out a set of tasks. Here is a list of the four tasks:

Circles

The first task is to take the translucent blue polygon and modify it so that it has several circles punched through it. You will achieve this by modifying the alpha value on a per-fragment basis, using a fragment program.  You should start by figuring out how to make a single circular hole that is in the center of the square.  Once you know how to create a single circle, you should then create several circles of varying radii that go outwards towards the edge of the square.  Finally, figure out how to replicate these circular "spokes" six times.

Here are the items we will look for in your square pattern:

19 circles.
One central circle.
Six "spokes" of circles, evenly arrayed around the center.
The circles in a spoke should have a smaller radius as they become further from the center.
Four-Fold Fractal

The second task is to draw a fractal that is based on forth powers of complex numbers. You will take one of the squares from the example code and modify it so that you display a white fractal set on some colored background (red in the example below). The colors (or possibly color bands) for the background are for you to decide. Let z_new = z^4 + c, where z and c are both complex numbers, and where ^ denotes exponentiation.  Recall that the equation for complex multiplication of two numbers (a + bi) and (c + di) is:  (a + bi) * (c + di) = ac - bd + i (ad + bc).  Let us denote the complex number z by (a + bi).  Then using the rules of complex multiplication, you should be able to calculate z^2 = z * z = (a + bi) * (a + bi).  Then you can calculate z * z * z = z^3 and z * z * z * z = z^4.

We will be making a map of what happens when using one value for c, but different starting values for z (which correspond to different locations in the plane). For some given value for c, let z_0 = some position in the complex plane, and look at the values z_1 = z_0^4 + c,  z_2 = z_1^4 + c, and so on. Plugging the result of a function back into itself is called an iteration. If these iterated values stay near zero (never leave a circle of radius 20), then draw a white pixel at the location for z_0. If the values do leave the circle, color the pixel something else (e.g. red). The resulting pattern is the fractal set that we want.  Do this for all the values for values of z such that z.real is in the range [-1.25, 1.25] and z.imaginary is in [-1.25, 1.25]. Use the texture coordinates as a starting point for setting the values for z.  Use at least 20 iterations of the function to create your fractal set.

The value of c will be handed to your program through these two lines of Java (already present in the example code):

symmetry_four_shader.set ("cx", cx);
symmetry_four_shader.set ("cy", cy);

Since the value of "time" will change, this means your fractal set will change over time.

Note that this fractal is not the classical Mandelbrot or Julia set from the equation z_new = z ^2 + c.  We are using a fourth order equation, not a second order equation.  You will get no credit for creating this more common quadratic fractal.

Edge Detection

The third task you will perform is edge detection of an image.  You will determine the "edginess" at a given pixel by calculating the Laplacian of a gray-scale version of the texture.

The example code for this task demonstrates how to display the texture of a color image.  Your first task will be to be able to calculate a gray-scale value for any given fragment.  You can do this by calculating the average of the red, green and blue colors of the pixel's texture.

Once you know how to calculate a gray scale value at any texture coordinate, you should then use this to calculate the Laplacian at a given texture coordinate.  The Laplacian of a scalar value that is stored on a grid (like a grid of texels) can be calculated based on the value at a central cell and its four neighboring cells.  Let v_center represent the value at a central cell, and let v00, v01, v10, v11 be the values at the four neighbors in s and t. You will need to step a small distance in texture space to find the colors at neighboring texels.  Then the Laplacian is L = 0.25 * (v00 + v01 + v10 + v11) - v_center.  The value of the Laplacian will be centered around zero and will sometimes be negative, but you should adjust its value so that a zero Laplacian is displayed as middle gray.  You should also "boost" the contrast of the intensities so that the Laplacian is more pronounced.

Finally, create a moving circle that shows the Laplacian values only inside the circle.  You can look at the code for the fractal that shows how you can pass coordinates (cx, cy) from your Java code to your shader.

Waves

The final task is to write a vertex program to modify the geometry of a collection of polygons. Your task is to replace one of the quads with a “wave creator”. You will need to subdivide the original quad into many tiny quads by modifying the Java code in p4_code.pde. Note that you will need to subdivide the quad in both x and y directions.  You should subdivide the quad into at least a 40x40 grid. Then your vertex program will displace these vertices along the normal vector using a sinusoidal pattern that you should create using the distance from the center of the quad.  You can determine how far a given fragment is from the center using the fact that the center of the quad has texture coordinates (s, t) = (0.5 0.5).

Do not move the vertices out of the plane in the p4_code.pde file -- this must be done in the vertex shader! You should pass a collection of equal area, planar quads into the shader.

If you just move the quads according to their distance from the center, the edges of your waves will cause cracks where the small polygons meet up with the other faces of the cube.  You must get rid of these cracks by dampening the waves when they become close to the edges of the cube.

Finally, be sure that your final geometry gets its color from your ripple pattern. To make the color match the geometry better, modify the color so that the peaks of the ripples are white and more distant portions are black.  You can do this by passing an "offset" value between the vertex and the fragment shader.  Such an "offset" parameter has already been defined in the two shaders, you just need to set it in one and use it in the other.

