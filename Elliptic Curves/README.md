#Elliptic Curve Cryptography

Public key cryptography is based on the idea that factoring very large numbers is extremely difficult and time consuming.  **Elliptic curve cryptography** takes that a step further, and is based the idea that it is near impossible to solve the elliptic curve discrete logarithm problem (ECDLP), or finding solutions to the elliptic curve.  Unfortunately, this is where things get EXTREMELY mathy, and the amount of hand-waving I need to do to explain this is much larger than before.  However this subject is also important today because this is the type of algorithm that we discovered the NSA had a secret backdoor for.  For more information, I would highly recommend watching [this video](https://www.youtube.com/watch?v=ulg_AHBOIQU).

The basic problem is this:  Suppose E is an elliptic curve and Q is a point on E.  The discrete log problem to the base Q is the problem: Given P is in E, find k such that P = kQ if such k exists.  However, addition, and therefore multiplication, works differently in elliptic curves, so we'll need to define that.

For example, if we wanted to solve the equation k(2,1)=(2,4) over mod 5, we're essentially asking how many times the point (2,1) needs to be added to itself to becomes (2,4) over mod 5.  Once you get into very large numbers, this problem becomes incredibly difficult.  Just try solving that problem by hand after reading the sections below on adding points.  It's more cumbersome than you'd think.

* [Elliptic Curves](https://github.com/MovieStiles/Cryptography/tree/master/Elliptic%20Curves#elliptic-curves)
   * [Finding the Points on an Elliptic Curve](https://github.com/MovieStiles/Cryptography/tree/master/Elliptic%20Curves#finding-the-points-on-an-elliptic-curve)
   * [Adding Points (Geometrically)](https://github.com/MovieStiles/Cryptography/tree/master/Elliptic%20Curves#adding-points-geometrically)
   * [Adding Points (Equations)](https://github.com/MovieStiles/Cryptography/tree/master/Elliptic%20Curves#adding-points-equations)
* [Diffie - Hellman using Elliptic Curves](https://github.com/MovieStiles/Cryptography/tree/master/Elliptic%20Curves#diffie---hellman-using-elliptic-curves)

##Elliptic Curves

Without going into concepts such as fields and rings, an **elliptic curve** is a function of the form: ![equation](http://latex.codecogs.com/gif.latex?y^2%3Dx^3&plus;Ax&plus;B).  The elliptic curve is made of both the set of points that satisfy this as well as the point at infinity.  These points tend to be under the mod of some number.

It is required that this function has no repeated zeroes, and the statement ![equation](http://latex.codecogs.com/gif.latex?4A^3&plus;27B^2\\neq%200) must hold true.

[Wolfram Mathworld](http://mathworld.wolfram.com/EllipticCurve.html) has some great examples on what these elliptic curves can look like:

![image](http://i.imgur.com/cfBJZst.png)

####Finding the Points on an Elliptic Curve

Let's take the equation ![equation](http://latex.codecogs.com/gif.latex?y^2%3Dx^3-x) mod 5.  Let's start by figuring out what the square roots are under mod 5:

![image](http://i.imgur.com/Fsb2DIq.png)

This knowledge makes things easier on us for finding the points:

![image](http://i.imgur.com/ZybNYRO.png)

####Adding Points (Geometrically)

There are three cases to consider when adding points P and Q on an elliptic curve.  The tutorial on [Certicom](https://www.certicom.com/index.php/21-elliptic-curve-addition-a-geometric-approach) has excellent pictures if it helps you.

1. Adding two points where ![equation](http://latex.codecogs.com/gif.latex?x_1\\neq%20x_2) and ![equation](http://latex.codecogs.com/gif.latex?P\\neq%20Q)

   Draw a line intersecting these two points.  It can be proven that this line will always intersect the curve at one more point.  Find that third point, then reflect that point across the x-axis.  That new point is your answer.

2. Adding point P and -P, or two points with the same x coordinate.

   The sum is simply infinity, though some say it is zero.

3. Adding P to itself.

   Draw the line tangent to P on the graph and find the next point that intersects the curve.  The reflection of this point across the x-axis is the answer.

####Adding Points (Equations)

Given an elliptic curve, E, defined by ![equation](http://latex.codecogs.com/gif.latex?y^2%3Dx^3&plus;Ax&plus;B) and let ![equation](http://latex.codecogs.com/gif.latex?P_1%3D%28x_1%2Cy_1%29%2C%20P_2%3D%28x_2%2Cy_2%29%2C%20and%20P_3%3D%28x_3%2Cy_3%29%3DP_1&plus;P_2)

There are four cases to consider:

1. If ![equation](http://latex.codecogs.com/gif.latex?x_1\\neq%20x_2)

   ![equation](http://latex.codecogs.com/gif.latex?x_3%3Dm^2-x_1-x_2)

   ![equation](http://latex.codecogs.com/gif.latex?y_3%3Dm%28x_1-x_3%29-y_1)

   Where ![equation](http://latex.codecogs.com/gif.latex?m%3D\\frac{y_2-y_1}{x_2-x_1})

2. If ![equation](http://latex.codecogs.com/gif.latex?P_1%3DP_2%2C%20y_1\\neq%200)

   ![equation](http://latex.codecogs.com/gif.latex?x_3%3Dm^2-2x_1)

   ![equation](http://latex.codecogs.com/gif.latex?y_3%3Dm%28x_1-x_3%29-y_1)

   Where ![equation](http://latex.codecogs.com/gif.latex?m%3D\\frac{3x_1^2&plus;A}{2y_1})

3. If ![equation](http://latex.codecogs.com/gif.latex?x_1%3Dx_2%2C%20y_1\\neq%20y_2), then the answer is infinity.

4. If ![equation](http://latex.codecogs.com/gif.latex?P_1%3DP_2%2C%20y_1=%200), then the answer is infinity.

##Diffie - Hellman using Elliptic Curves

Let's say Alice and Bob want to calculate a shared key.  They would do the following:

1. Fix an elliptic curve E, and let P be some fixed point in E.  This information is public.

2. Alice and Bob choose private integer keys, a and b respectively.

3. Alice calculates Q = aP and Bob calculates R = bP and exchange them.

4. Alice calculates aR = abP and Bob calculates bQ = baP = abP.  abP is the shared key.

####Example: ![equation](http://latex.codecogs.com/gif.latex?E:%20y^2=x^3+4x+8) over mod 23

Suppose we choose P = (2, 1)

Alice chooses a = 2, and Bob chooses b = 3

Calculating 2P:

![equation](http://latex.codecogs.com/gif.latex?m%3D\\frac{3x_1^2&plus;A}{2y_1}%3D%283x_1^2&plus;A%29*%282y_1%29^{-1}%3D%283*2^2&plus;4%29*%282*1%29^{-1}mod%2823%29%3D%2816*12%29mod%2823%29%3D192mod%2823%29%3D8)

![equation](http://latex.codecogs.com/gif.latex?x_3%3Dm%5E2-2x%3D8%5E2-2*2%3D32%20mod%20%2823%29%3D14)

![equation](http://latex.codecogs.com/gif.latex?y_3%3Dm%28x_1-x_3%29-y%3D8%282-14%29-1%3D-97mod%2823%29%3D18)

So 2P = (14, 18) = Q

Now we have to calculate 3P:

3P = 2P + P = (2, 1) + (14, 18)

![equation](http://latex.codecogs.com/gif.latex?m%3D%5Cfrac%7By_2-y_1%7D%7Bx_2-x_1%7D%3D%28y_2-y_1%29*%28x_2-x_1%29%5E%7B-1%7D%3D%2818-1%29*%2814-2%29%5E%7B-1%7Dmod%2823%29%3D%2817*2%29mod%2823%29%3D11)

![equation](http://latex.codecogs.com/gif.latex?x_3%3Dm%5E2-x_1-x_2%3D11%5E2-2-14%3D%28121-16%29mod%2823%29%3D13)

![equation](http://latex.codecogs.com/gif.latex?y_3%3Dm%28x_1-x_3%29-y_1%3D11%282-13%29-1%3D-122mod%2823%29%3D16)

Doing the above steps again with the new numbers comes out to 3P = (13, 16) = R

Hopefully you can see from this how this can become an unwieldy process, even with small numbers.
