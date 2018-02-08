---
layout: post
title:  "Proving the Deriviative of the |E|"
date: 2018-2-11 00:00:00 -0600
categories: [Math, GoogleBait]
---

>For man had succeeded in orbiting the world -
>but eccentricity, the eccentricity vector never changes.
>         -Ron Perlman in Fall-Calc III

Once for extra credit in my Calc III class I was tasked with showing for an elliptical orbit, like the one our space station orbits in, the $$\vec{e}$$ is constant.
We were to do this by showing that it's derivative was 0.
After searching around the internet, I could not find this proof presented anywhere. 
So in my quest to water down the graduating students of tomorrow (and gain mad page views), I will present my proof on the matter here.

I'll start with some definitions, and needed identities will be introduced as necessary.

* The position vector: $$\vec{r}$$ and its magnitude $$r$$
* The velocity vector: $$\frac{d}{dt}\vec{r} = \vec{v} = \vec{r}^{\prime}$$ and its magnitude $$v = r^\prime$$ 
* The acceleration vector: $$\frac{d^2}{dt^2}\vec{r} = \vec{a} = \vec{v}^{\prime} = \vec{r}^{\prime\prime}$$ 
* The gravitaional parameter: $$\mu$$ (This depends on the masses of the objects but is simply an arbitrary constant here)
* The angular momentum vector: $$\vec{h} = \vec{r} \times \vec{v}$$ (which is constant due to Newton's Laws of Motion)


Start with the definition of the eccentricity vector:
$$\vec{e} = \frac{1}{\mu} (\vec{v} \times \vec{h}) - \frac{\vec{r}}{r} $$
and take its derivative, if we can show that the derivative of a function is equal to zero, then the function is constant

$$ \frac{d}{dt} \Big( \vec{e} = \frac{1}{\mu} \big( \vec{v} \times \vec{h} \big) - \frac{\vec{r}}{r} \Big) $$

$$ \frac{d}{dt}\vec{e} = \frac{1}{\mu} (\vec{v}^{\prime} \times \vec{h} + \vec{v} \times \vec{h}^{\prime} ) - \left(\frac{ r \vec{r}^{\prime} - \vec{r} r^\prime}{r^2} \right) $$

Through various applications of product, and quotient rules. Observe the $$\vec{h}^{\prime}$$ in the second cross product, as mentioned above, angular momentum is conserved in a system.
Therefore, $$\vec{h}$$ is constant and  $$\vec{h}^{\prime}$$ will be zero (since the derivative of a constant is zero).

From there we have 
$$\left(\frac{ r \vec{r}^{\prime} - \vec{r} r^\prime}{r^2} \right) $$ which I will multiply by $$\frac{-\mu r}{ -\mu r}$$. After moving the quotent out front and distribution we have:

$$\frac{-\mu}{-\mu r^3}\Big( r^2 \vec{v} - \vec{r} r r^\prime \Big) $$

Keeping in mind that $$r$$ and $$r^{\prime}$$ are scalar coefficients we will transform them into dot product analogs.

 $$\frac{-\mu}{-\mu r^3}\Big( (\vec{r}\cdot\vec{r}) \vec{v} - (\vec{r} \cdot \vec{v} )\vec{r} \Big) $$

From here we will simply need two properties of cross products. The first being one of the triple cross product rules $$ v \times ( u \times w) = (u\cdot w)u-(u\cdot v)w $$
and the second being $$ \alpha \vec{a} \times \vec{b} = \alpha (\vec{a} \times \vec{b} )$$. Using these identities we have:

 $$\frac{-\mu}{-\mu r^3}\Big( \vec{r} \times (\vec{r} \times \vec{v}) \Big) $$

The final identity we need here is an inverse square boi $$ \vec{r}^{\prime\prime} = \frac{-\mu}{r^3}\vec{r}$$. Applying this and noticing the $$\vec{h}$$ hiding we are left with:

$$\frac{d}{dt}\vec{e}  =\frac{1}{\mu} (\vec{v}^{\prime} \times \vec{h} ) - \frac{1}{\mu}\Big( \vec{r^{\prime\prime}} \times \vec{h} \Big) = 0$$

And since the derivative of $$\vec{e}$$ is zero we have proven that the eccentricity of an orbit is constant.
This is good since arbitrary change in the nature of an orbit along its course would not make very much sense and be devastating to all sorts of things (so long as they rely on stable orbits that is).
