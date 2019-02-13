---
layout: post
title: "Using Genetic Algorithms to Design Stronger Trusses"
description: "Implementing a Genetic Algorithm to optimize the design of simple trusses."
categories: ["Machine Learning", "AI", "Truss", "Statics", "Genetic Algorithm"]
location: "Ruston, LA"
thumbnail: images/truss/truss-model.jpg
---
<br />{% include image.html url="{{../../../../../../images/truss/pot-trusses.jpg" caption="Three potential truss configurations for this competition. Can you guess which one would perform best?" width="800px" align="center" %} <br />
You can find the source code for this project on GitHub [here.](https://github.com/ajbradberry96/genetic-truss-generation)

## What are Genetic Algorithms?
*****
Disclaimer: I am not a biologist or an expert on genetics. I am fairly confident that the broad strokes of anything I say about how evolution works in nature is at least a little accurate, but take anything I say with a grain of salt.

*****
In nature, organisms have a collection of genes that help determine how an organism develops and behaves in its environment. An organism that survives long enough to procreate and ensure that its offspring survive to be able to procreate in turn are more likely to pass on their genetics to their children than an organism that doesn't survive as long or produce viable offspring. This results in a younger generation of organisms whose genes are more optimal for survival and procreation in the environment. In addition, the process with which parents pass their genes to their offspring isn't absolutely perfect. That is, slight mutations may occur. These mutations might make its offspring more or less likely to pass on its own genetic code, including those mutations. It is in this fashion that organisms in nature approximate the most optimal organisms for that environment or environmental niche.<br />

A [Genetic Algorithm](https://en.wikipedia.org/wiki/Genetic_algorithm) is a strategy for optimizing a function that does so by mirroring this process. 

1. Starting with an initial collection of random solutions, each is given a score that determines how well it optimized that function. This score is called its "fitness," and this group of solutions is called Generation 1.

2. Each solution is ranked based upon its fitness.

3. A certain proportion of the population is disposed of, with higher scoring solutions being more likely to survive than those with a lower fitness.

4. The population is replenished with copies of the surviving solutions. These copies might be exact replicas or have slight variations (mutations) from the parents. This new group of solutions is called Generation 2.

5. This process is repeated, creating Generation 3, 4, and onward, until some arbitrary stopping point.

At this point, the best performing solutions in the collection are likely to have approximated the true optimal solution to the function. <br />
### Let's walk through this process with an example.

We are going to try to find the global maximum for the (intentionally convoluted and arbitrary) function

$$ f(x) = e^{\sin(x)} - \sin(x) + \frac{1}{10}x^{2} - \frac{1}{100}x^3 $$

on the interval $$ 0 < x < 10 $$.

To start, we have a collection of 20 random values for $$ x $$ between 0 and 10. The fitness of each $$ x_i $$ is given by  $$ f(x_i) $$, as we simply want to maximize the function.
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-1-table.png" caption="Table of Solutions and Fitnesses for Generation 1" width="125px" align="center" %}
{% include image.html url="{{../../../../../../images/truss/Gen-1-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 1" width="400px" align="center" %}
<br />
Next, we get rid of a certain proportion of the population. $$ x $$ values with a higher fitness should be more likely to survive than those with a lower fitness. For simplicity, let's do this by just dropping the worst 10 $$ x $$ values.
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-1-survivors-table.png" caption="Table of Solutions and Fitnesses for Generation 1 after culling" width="125px" align="center" %}
{% include image.html url="{{../../../../../../images/truss/Gen-1-survivors-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 1 after culling" width="400px" align="center" %}
<br />
We then replenish this generation by having each $$ x $$ value clone itself with a small mutation, i.e. adding a random number between -1 and 1.
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-2-table.png" caption="Table of Solutions and Fitnesses for Generation 2" width="125px" align="center" %}
{% include image.html url="{{../../../../../../images/truss/Gen-2-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 2" width="400px" align="center" %}
<br />
Let's repeat this process until Generation 10.
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-3-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 3" width="400px" align="center" %}
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-5-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 5" width="400px" align="center" %}
<br />
{% include image.html url="{{../../../../../../images/truss/Gen-10-table.png" caption="Table of Solutions and Fitnesses for Generation 10" width="125px" align="center" %}
{% include image.html url="{{../../../../../../images/truss/Gen-10-graph.png" caption="Graph of Fitnesses vs Solutions for Generation 10" width="400px" align="center" %}
<br />
Let's see how well we did compared to the actual graph of $$ f(x) $$:
{% include image.html url="{{../../../../../../images/truss/f-of-x.png" caption="Graph of f(x)" width="400px" align="center" %}
<br />
It looks like we found something close to the maximum! In fact, our best solution (7.7218, 3.0618) is extremely close to the actual maximum at [(7.7106, 3.0619)](https://www.wolframalpha.com/input/?i=maximize+e%5Esin(x)+-+sin(x)+%2B+0.1x%5E2-+0.01x%5E3,+x+between+0+and+10), the fitnesses being only ~.0001 away. 

Obviously, this example could have been solved with simple analysis with calculus, but that isn't always practical. For example, much more complicated functions with multidimensional inputs quickly get out of hand to do manually. Or even worse, we might not even know how to formulate the function in terms of arithmetic in this nice way. For example, it's is very hard to analytically determine the best possible fin shape for swimming in maple syrup. It is in these areas that Genetic Algorithms thrive, so long as the initial population can fill up the input space sufficiently well.

*****
#### Note on explore vs exploit
Genetic Algorithms, like many search algorithms, have to deal with the fundamental tradeoff known as ["explore vs exploit"](https://medium.com/@dennybritz/exploration-vs-exploitation-f46af4cf62fe). We want to make sure not to get stuck in any local optima of our fitness function, as our goal is to find the global optimum. To reduce this risk (with the penalty of having to give the algorithm more time and generations to converge), one would increase the size of the population (thus increasing the chances that an initial solution in Generation 1 is near the global optimum) and/or reduce the number of solutions killed off each generation (thus reducing the chance of killing off a solution near the global optimum when that global optimum is surrounded by solutions that are far from optimum).

*****
#### Note on crossover
In creating a new generation of solutions, Genetic Algorithms and nature alike might use not only mutation, but a process known as [crossover](https://en.wikipedia.org/wiki/Crossover_(genetic_algorithm)). With crossover, the genetic information of two parents are combined to produce the offspring's genetic information. This is a popular, natural extension of the vanilla Genetic Algorithm described here.

*****
## What is a truss?
A [truss](https://en.wikipedia.org/wiki/Truss) is a structure that is made up of a collection of two-force members and a collection of joints connecting them. Two-force members are basically objects that only have force applied to them at two points. These are the beams connecting joints, and the joints on either end of the beams are the two points at which force is applied. 


{% include image.html url="{{../../../../../../images/truss/plain-truss.png" caption="A diagram of a basic truss." width="800px" align="center" %} <br />

Trusses are used all over the place, from bridges to buildings, and the characteristics of these trusses are incredibly important to determining how stable and strong the structures using them are.


{% include image.html url="{{../../../../../../images/truss/tolliver-truss.jpg" caption="Trusses in Tolliver Hall at Louisiana Tech, just one of many places you'll start to see them after you have a name for these triangle-y structures." width="800px" align="center" %} <br />

The difficult question is this: Given certain constraints, how do you determine what the optimal truss structure is? That is, what truss design has the optimal weight-to-strength ratio? This is precisely the question that I and 100 sutdents at Louisiana Tech were asked in the fall of 2016 for our final project and Sophomore class-wide competition in our Statics and Mechanics of Materials course.

*****

## The Project
**The task:** Design a truss using tempered hardboard with at least 5 joints, bases 28" apart, and a load placed at 13" with the best performance ratio (weight supported / weight of truss) possible.

This is an extremely hard problem to solve by hand, as there are inifinitely different possible truss configurations. Also, up to this point in the course, we had never even attempted to actually design a truss, so we had no theoretical strategies to use to optimize our designs. 


{% include image.html url="{{../../../../../../images/truss/pot-trusses.jpg" caption="Three potential truss configurations for this competition. Can you guess which one would perform best?" width="800px" align="center" %} <br />

Not only did we have to design the shape of the truss, but also the shape of each of the members. Box shaped members are stronger in compression than flat members, but weigh more and are more difficult to assemble. We also had to take into consideration how thick each member would have to be.

*****
#### Note on statically determinate trusses
Another requirement was that our trusses be [statically determinate](http://www.ae.msstate.edu/vlsm/truss/statically_det_indet_trusses/statically_det_indet_trusses.htm). This basically meant that they had to be able to be analyzed with the statics that we had learned up to that point in the course. Effecively, a truss is statically determinate if the number of members is two times the number of joints minus 3, i.e. $$ M = 2J - 3 $$.

*****
As you might have figured out by now, at this point, my thoughts immediately jumped to Genetic Algorithms. After all, here we have a complicated function, i.e. the performance ratio, that is determined by a solution, i.e. the truss design, that can be easily modified after each generation to iterate toward an optimal solution.

Before we can use a genetic algorithm, we have to answer several questions: What is our fitness function going to be? How do we generate an initial population? How do we evolve the offspring of each generation? Let's answer these one by one:

- What is our fitness function?

Our fitness function is simply our performance ratio, which it turns out, is easily calculated if we are given a valid truss.

- How do we generate a random truss?

This part is deceptively tricky. We want a collection of joints with members between them that don't intersect. Also, we want to make sure that every joint is connected to at least two members and that every member is connected to at lest two joints. *Also*, we want to make sure that the truss doesn't have two or more "sections" that are connected solely by a single joint that would just flop around uselessly. (For those of you with a graph theory background, we want a plane graph with no vertices of degree one and with no cut vertices.)


{% include image.html url="{{../../../../../../images/truss/bad-truss.png" caption="An example of a truss with two sections where one is just free to flop around." width="800px" align="center" %} <br />

This turns out to be surprisingly complicated, but the method I used was as follows:

1. Place a joint at each endpoint and one at a random height at the point that the weight was to be applied.

2. Add two other random joints.

3. Connect these randomly until there are 7 members.

4. Optionally, add joints, connecting them to two other joints with members.

5. Assign each member a type and width randomly.

- How do we mutate trusses?

This question is a little bit easier. Simply slightly move around joints randomly(as long as doing so doesn't cause members to intersect), adjust widths randomly, adjust types of member randomly, or add a new joint with two more random members.

I ended up implementing the following algorithm in Java:

1. Generate 1000 random trusses.

2. Repeat the following until the performance ratio of the median truss stabilizes (that is, until consecutive performance ratios are less than a certain distance apart):

	a. Analyze trusses

	b. Sort trusses by performance ratio

	c. Remove trusses until 500 remain, favoring better performing trusses

	d. Add back in 500 slightly modified versions of the surviving 500

My first generation of trusses ended up looking like this, with the left truss being the best, the middle being the median, and the right being the worst:


{% include image.html url="{{../../../../../../images/truss/Gen-1.png" caption="The first generation of trusses." width="800px" align="center" %} <br />

The best truss here has a theoretical performance ratio of 1038. This would have likely won the competittion as it was, as the typical winners usually scored around 600. However, this theoretical performance ratio was **very** optimistic, as my model didn't take into consideration factors like [stress concentrations](https://en.wikipedia.org/wiki/Stress_concentration) (which I would have loved to incorporate into my model, but unforunately we didn't learn this concept until after the competition was well under way), and certain constraints at each joint (a topic I will revisit in a moment).

Let's skip forward to Generation 13:


{% include image.html url="{{../../../../../../images/truss/Gen-13.png" caption="The 13th generation of trusses." width="800px" align="center" %} <br />

Already we were seeing massive improvements to the best score, with the best score here scoring 1242! Not only that, but even the median truss was performing at a very high level, scoring 618. It also appears as if multiple different truss shapes were still being explored, so we were probably still far from converging on our optimal solution. Let's jump forward 1000 generations:


{% include image.html url="{{../../../../../../images/truss/Gen-1013.png" caption="The 1013th generation of trusses." width="800px" align="center" %} <br />

At this point, it seemed like we were starting to converge on a solution. The median and the best truss appeared very similar, and had comparable performance ratios of 1752 and 1852 respectively. Once you reach the point at which much of the population is nearly identical, dramatic increases of fitness are unlikely to occur.


{% include image.html url="{{../../../../../../images/truss/performance-ratio-over-generation.png" caption="The fitness of the best performing truss over the generations. As you can see, initially the trusses were improving dramatically with each generation. As the generations went on, though, those improvements became more incremental." width="800px" align="center" %} <br />

The next step was to take this solution, along with its very attractive performance ratio, and move into the next phase of the process: Actually making and testing the truss in real life.

The first thing I had to do was fix those issues with the joints I mentioned earlier. What I mean by "joint constraints" is that at each joint, box-shaped members had to be able to fit neatly inside one another like so:


{% include image.html url="{{../../../../../../images/truss/joint-issues-2.png" caption="Members had to fit together on a joint like so." width="400px" align="center" %} <br />

This meant that I had to go through each joint and slightly modify each member attached to that joint so that all of them would fit nicely together. As this process slightly reduced the theoretical strength of the truss, our new optimal performance ratio was about 1,400, still nothing to sneeze at.

Those minor issues dealt with, it was time to move on to laser cutting our truss out of tempered hardboard along with a team of two other Engineering students. We modeled the truss I designed in Solidworks and sent it off to the manufacturing lab to be cut out.


{% include image.html url="{{../../../../../../images/truss/truss-model.jpg" caption="Our Solidworks model of our truss" width="400px" align="center" %} <br />
{% include image.html url="{{../../../../../../images/truss/disassembled-truss.jpg" caption="The pile of pieces we got back from the lab." width="600px" align="center" %} <br />

A day later, we picked up a black trash bag full of the nerdiest jigsaw puzzle you've ever seen. We Gorilla glued the box beams together, glued the reinforcement tabs onto the end of the members, and we were ready to see what this design looked like in real life.

We assembled it and finished it off with a fresh coat of paint. We were left with this beautiful piece of work, confident that we were going to dominate the competition:


{% include image.html url="{{../../../../../../images/truss/constructed-truss.jpg" caption="Our competition-ready truss." width="600px" align="center" %} <br />

This truss was supposed to be able to hold up 6,254 lbs. while weighing only 4.4 lbs. giving it a performance ratio around 1,400, double what was required to win the competition most years. 

The final step was undergoing the actual competition. We were to put our truss into a machine whose job is basically just to squish it as hard as possible. We found out the day of the competition that the machine had a maximum force of only 5,000 lbs. This meant our truss could potentially beat the machine! We set our truss up, crossed our fingers, and hoped for the best.

We were biting our nails as the machine hit 500 lbs...

1,000 lbs... we only needed 2,600 or so to have a score of 600 and likely win the competition...

2,000 lbs... so far so good...

3,000 lbs... Now we were almost guaranteed to win, so all we have to do now is beat the mach-

~**CRACK**~

Our truss had failed at only 3,200 lbs. giving us a performance ratio of just 726.

We won the competition, but sadly we were unable to win against the machine.

It turned out that the part of the truss that failed was actually a poorly glued member that hadn't even broken, just fallen apart. The world will never know how good the design truly could have been if we were just a little more liberal with the glue, sadly.

Overall though, this was by far the most fun project I had ever done up to this point!

## Future work

If I ever were to revisit this model, I would

- Investigate hyperparameters more, doing things like messing with population size, mutation rate, etc.

- Incorporate joint constraints and stress concentraions into the model.

And above all...

- USE MORE GLUE
