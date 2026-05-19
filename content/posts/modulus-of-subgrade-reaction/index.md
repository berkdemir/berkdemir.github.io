---
title: "Modulus of Subgrade Reaction"
date: 2022-12-16T00:00:00+00:00
draft: false
---

# Introduction
By now, everything should have been settled about modulus of subgrade reaction. We all know some typical statements about it:
- It depends on the soil properties
- It depends on the foundation size
- It depends on loading type, temperature, bitcoin prices and others.

However, let's think about a weathered rock with E=500 MPa and a foundation to be built on top of this rock with 20 x 50 m dimensions. You can say it depends on many factors as much as you like, everybody has a rough idea already: 100,000 kN/m3. So, if you are a fancy engineer and dare to make some calculations, you can find much lower values. Will they believe you or will they think that you are being too conservative (if lowering the subgrade reaction means being conservative)?
Let's start with simple terms. The equation that everybody knows and nobody wants to use:
$$
K=\frac{q}{s}
$$
So, the subgrade reaction is equal to a spring stiffness distributed under the foundation. If you divide the pressure by the settlement, you will find the subgrade reaction, amount of deformation for unit pressure.
If we think about how we calculate settlement (how it depends on **many** factors), we can see actually how complex this modulus is.
# Using Settlement Calculations
Use any method you like. If you calculate the settlement of a foundation (say for 200 kPa uniform loading) with dimensions of 20 x 50 m on a weathered rock with elasticity modulus of 500 MPa, you will find something around 7-8 mm (or someting close) that will result in 25000 kN/m3 modulus of subgrade reaction
You know the easiest method without bothering with any influence factor:
$$
s=\frac{qB(1-v^2)}{E}
$$
If you do that, you will find 28000 kN/m3.
If you use Settle 3D with flexible foundation, you will find something similar:
![](Settle3D.png)
So, the question is: **Where does this 100000 kN/m3 comes from?**
# Bowles
Bowles was a great engineer and teacher. His books have guided most of us. Since he was a practicing engineer, his recommendations were pin-point to most of the problems we had. It was the first geotechnical book I have read from the cover to the end.
But he didn't know his recommendations will be misunderstood. I will try to explain the two of the most misunderstood recommendations of Bowles and you will be the judge.
## Relationship Between Modulus of Subgrade and Bearing Capacity
He was such a clever engineer, he have found a simple equation to relate bearing capacity and modulus of subgrade reaction. And he was 100% correct. But I can say that 95% of use of this equation is not correct.
$$
K=40\cdot FS\cdot q_{all}
$$
What a magical formula. Let's start with the wrong use: If you ever calculated the bearing capacity from general bearing capacity formula (cNc+qNq+...) and use this equation with that bearing capacity, you were wrong.
This is the derivation of this equation: Since general bearing capacity equation for granular soils results in very high bearing capacity, it was the general tendency to calculate a bearing capacity for settlement limits and that limit was 1 inch - 25 mm. You can calculate an **allowable bearing** capacity using SPT which is in fact the load that corresponds to 25 mm settlement. An example from Bowles is Parry (1977) equation:
$$
q_a=30N_{55}
$$
If you have the required load to generate a 25 mm settlement, you can also calculate the modulus of subgrade reaction using the logic below.
$$
K=\frac{q}{s}=\frac{q_{all}\cdot FS}{25\ mm}=\frac{q_{all}\cdot FS}{0.025\ m}=q_{all}\cdot FS \cdot 40=40\cdot q_{ult}
$$
As you can see, it is not even slightly logical to use any bearing capacity that is calculated using some other method.
## Modulus of Subgrade Reaction Table for Different Ground Types
Bowles also has a table. And if you recommend something like 10-20 MN/m3 for a weathered rock to a client, they will come back with this table saying Bowles is clear.
![](Bowles.png)
Bowles recommended 128000 kN/m3 for dense sand, do we dare to say our weathered rock is softer than dense sand?
Of course not. The only thing we can say is: Bowles knows very well that modulus of subgrade reaction depends on the foundation width. So, how can he recommend a constant value for a soil type? He does not. He is -most probably- recommending the ranges for plate loading test or in worst case, for a foundation with a size of 2-3 m which he repeatedly uses in his book to demonstrate the methods. He usually does not use a foundation with 20 or 40 m width.
Assuming k1 = 150000 kN/m3 from plate loading test on a plate with B1 = 0.3 m,  we can estimate our ks using several methods given in Bowles:
Equation for clays from Terzaghi:
$$
k_s = k_1 \cdot \frac{B1}{B}=150000 \cdot \frac{0.3}{20}=2250 kN/m3
$$
Equation for sands from Terzaghi:
$$
k_s = k_1 \cdot (\frac{B+B1}{2B})^2=150000 \cdot (\frac{20+0.3}{2 \cdot 20})^2=38633kN/m3
$$
If we go back to our Settle 3D model where we found K=27000 kN/m3  and reduce the foundation size to 0.3x0.3m to represent a plate, the resulting deformation is **0.147 mm** for 200 kPa loading. **The modulus of subgrade reaction for plate loading on our weathered rock is 1,360,544 kN/m3**. If you do the same with immediate settlement hand calculations (such as Mayne & Poulos or Gazetas), the results are even higher (3 to 7 million kN/m3)
What do we say? 100000 kN/m3 is very low for our rock, it should be much higher! But not for our foundation, for plate loading. In fact, if we draw a curve using Mayne and Poulos' settlement equation, it will look like this: You can see that for elasticity modulus of 50 MPa (stiff clay / dense sand), it is around what Bowles proposed.
![](image-20210814211607941.png)
What we understand from these text was given at the beginning: IT DEPENDS ON THE FOUNDATION SIZE. For our weathered rock, you can see how much it depends on the foundation size.
![](FoundationSize.png)
# What Should We Do?
You have to calculate the settlement of the foundation using any method you like or your project specification requires: Hand calculations, plate loading tests, FE analyses using the parameters you have estimated. Any method. Use the settlement you have found in our holy formula and find the modulus.
Up to this point, we haven't discussed varying the modulus of subgrade reaction along the foundation. As you know, if our foundation is not rigid, we expect a dished shape settlement. However, if you enter uniform subgrade modulus under a plate loaded with uniform pressure, it will not bend, it will uniformly settle. So, you can see how wrong can our results be with uniform subgrade modulus: The plate will not bend in beam-on-spring model. Very dangerous.
But, if you are not on that level, just get an average deformation using any method mentioned above and divide the average pressure to the average deformation. If you want to use different modulus of subgrade reaction throughout the foundation, you can estimate some ranges using the deformation along the foundation. I think Settle calculates the subgrade reaction along a query line directly in the new versions.
An example calculation:
![](SettletoModulus.png)
You have many methods to calculate modulus of subgrade reaction. A modulus that can be calculated with a fair accuracy even with an hand calculation shouldn't be a problem anymore.
