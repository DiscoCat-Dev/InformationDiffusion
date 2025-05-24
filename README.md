# Information Diffusion Model: Simulating the Competition of Multiple Information Sources

Table of Contents

- [Overview](#overview)

- [Getting Started](#getting-started)

- [Theoretical Underpinning](#theoretical-underpinning)

- [Structure of the Model](#structure-of-the-model)

- [Observation](#observation)

- [Note on Limitations](#note-on-limitations)

- [Citation](#citation)

- [Miscellaneous](#miscellaneous)

## Overview

The model presented here was inspired by the chapter *City Rumors* from the book *Lively Mathematics* ([1934] 2020), where its author, Yakov Perelman, discusses how fast a rumor spreads in a city. Imagine that one morning, at 8:00 a.m., a visitor brings some interesting news to a city of 50,000 people. Upon arriving at a hotel, he tells the news to three residents, which takes a quarter of an hour. So, at 8:15 a.m., four people know the story: the visitor and three residents. Each of these three residents tells three more, again taking 15 minutes. So by 8:30 a.m., 13 people know the news, including the visitor. The question is: when will everyone in the city know the news?

This simple story, which Perelman wrote to popularize the exact sciences, is also sociologically significant. It emphasizes the effect of structure of interaction on the spread of information. The model presented here expands on this by introducing more realistic features, such as the spatial distance between agents and competing information sources that vie for the attention of a limited population. At the same time, the model strives to remain clear and simple, without an overload of parameters.

This model is a part of the author's project on information diffusion and designed as a case study of how [interaction structure](https://github.com/DiscoCat-Dev/InteractionStructure) affects overall social dynamics, focusing on information diffusion. 

## Getting Started

To run the model:

1. Open the `.nlogo` file in NetLogo 6.3.0 (or above).
2. Set the switches and sliders as shown in the image below.
3. Press `setup`, and follow the examples of the Structure of the Model for  `go`  

## Theoretical Underpinning

Charles A. Lave and James G. March, in their book *An Introduction to Models in the Social Sciences* (1993), state that the information diffusion process belongs to a broad class of phenomena called **"birth process" models**, named after a model that approximates rabbit reproduction. The diffusion of rumors or information and the reproduction of rabbits are, in fact, similar processes: in one case, we have rabbits begetting rabbits; in the other, rumors begetting rumors.

However, in real-life situations involving human beings — or other entities that are best counted in discrete terms — and in populations where diffusion takes place within a finite group, the simple birth process model cannot realistically reflect reality. It requires a **limiting factor** to capture the true nature of the growth process.

Lave and March define such a process as a **"birth process with limits"**, modeled by the following differential equation:

$$
\frac{\Delta n}{\Delta t} = a \cdot n \cdot (N - n)
$$

Where:

- $N$ - the growth limit or total population size (as in Perelman's example)

- $n$ - the number of people who have the information at a given time, such as at 8:00 a.m. or 8:15 a.m.

- $t$ -  the time period (e.g., $t_1$ and $t_2$), such as for 8.00 a.m. and 8.15 a.m.

- $a$ - a constant specific to the situation, such as the number of people to whom each person transmits the information (e.g., 3 or 4) 

A change in the value of $a$ directly affects the **speed of growth**. However, as this model demonstrates, $a$ is not the only factor influencing growth speed. In the early stages, when $n$ is small, $(N - n)$ is large. But as time passes and $n$ increases, $(N - n)$ becomes progressively smaller. When everyone knows the information — that is, when $N = n$ — the value of $(N - n)$ becomes zero, and growth stops. In short, the term $a \cdot n$ grows, while $(N - n)$ shrinks.

The expression $\frac{\Delta n}{\Delta t}$ represents the **rate of change in growth**. This is important because, in Perelman's story, the actual change occurs between 8:00 a.m. and 8:15 a.m., during which the number of informed individuals increases from 1 to 4. The $\Delta$ symbol marks this difference, and the **rate** is computed by dividing the change in quantity by the change in time.

Differential equations are not the only way to model diffusion processes. Paul E. Smaldino, for instance, in his book *Modeling Social Behavior* (2023), discusses diffusion models as a subclass of **contagion models**, and uses **difference equations** instead. However, this does not invalidate the use of a differential approach in our model.

## Structure of the Model

When you load the model and press `setup`, you will see the following interface:

![interface](Information%20Diffusion%20interface.png)

The world initially consists of grey agents, and among them are one red and one yellow agent. Information diffusion begins with a single agent, one red and one yellow,  and spreads through the surrounding population of grey agents. The presence of grey agents is not accidental — they represent the **substratum** for diffusion.

The sliders `info-red-link-num` and `info-yellow-link-num` represent the constant $a$ for red and yellow agents, who spread two different types of information (indicated by color). The word "link" is used symbolically and does not imply actual network links. Asking an agent to "link" to 3 others and then change their color is logically equivalent to directly selecting 3 agents and changing their colors. While technically different, both approaches produce the same outcome. Avoiding the use of literal links reduces computational costs by eliminating redundant actions.

The `red-radius` and `yellow-radius` sliders represent the distance over which information is transmitted. In other words, an agent may transmit information to others located at distances of 1, 2, 3 units, and so on. This introduces a **spatial dimension** to information diffusion, which can play a significant role in shaping the dynamics.

There are four plots that illustrate different aspects of the diffusion process:

- **Number of Adopters**:  
  This plot shows the number $n$ — the number of agents who have adopted each piece of information at every tick. It typically forms an S-curve, which is characteristic of diffusion processes.

- **Growth Rate of Spread**:  
  This plot represents the change $\frac{\Delta n}{\Delta t}$, which usually follows a bell-shaped curve. It reflects the speed of diffusion — initially slow, then accelerating rapidly, and finally decelerating after reaching a peak.

- **Competition Ratio**:  
  This plot shows the relative proportions of red and yellow agents. Since the two types of information are in competition, the plot indicates which is becoming dominant. An upward trend signifies red is gaining dominance; a downward trend means yellow is overtaking.

- **Spatial Spread Direction**:  
  This rare plot shows the trajectory of the **centroids** of red and yellow agents. A centroid is the average of the $x$ and $y$ coordinates of all agents of a given type. As information spreads, its “center of gravity” shifts, illustrating the spatial directionality of diffusion.

There are also three monitors:

- **Tipping Point (10%)**:  
  This monitor displays the 10% adoption threshold of the total population ($N$). Hannu Jaakkola, in his paper *“Comparison and Analysis of Diffusion Models”* (1996), calls this the "lower threshold level of penetration" and argues that crossing it usually leads to full diffusion; below this point, the process remains unstable.

- **Red** and **Yellow** Threshold Monitors:  
  These indicate the precise moment when each type of information crosses the 10% threshold. The information that crosses this level first is more likely to become dominant in the population, according to the classic diffusion model.

## Observation

Make sure the interface looks like the image above, except that the positions of the initial red and yellow agents in the world are random. When `go` is pressed, the observer will see how the number of red and yellow agents increases. They will multiply exponentially, and one of them—the one that first crosses the threshold—will end up outnumbering the other.

Each piece of information will spread and collide somewhere in the world. The information fronts, where the two types face each other, will roughly form an even "line." The model will eventually stop when there is no more change in the world because the system has reached equilibrium.

Now increase the parameter $a$ of one type of information by adjusting either `info-red-link-num` or `info-yellow-link-num` (not both) by one unit and press `go`. Was the completion of diffusion faster or slower than the previous run? Increase the same slider by three units and observe again. Does the information with the higher $a$ increase faster? Does the information front still form a roughly "straight line"? Or is it now a little bit "bowed" somewhere?

If the information front looks a little bit "bowed," this results from the fact that information with higher $a$ spreads rapidly and aggressively "pushes" the information with lower $a$, as if trying to exclude it from the arena. Keep increasing the $a$ of the same slider and see what happens. Also observe how the plots change with each experiment.

Now reset the slider to a previous value, such as 3, and increase the radius of the same type of information to twice the radius of the other type. This time, the information with the larger radius not only increases rapidly but may also surround the information with the smaller radius. If the initial agents were generated closer to the center of the world, this will be easily observable. A greater radius gives each agent a significant advantage in spreading information by covering larger areas of neighbors.

Next, set $a$ of red agents lower, for example to 3, and increase the $a$ of yellow agents to twice that, such as 6. But this time, increase the radius of the red agents by twice plus 1 or 2 units. See who wins. According to the classic diffusion model, an increase of $a$ should significantly increase the speed of diffusion. However, this time the observer may notice that agents with lower $a$ but a higher radius win more often. Also, in this case, the Competition Ratio graph may look different from cases with equal radii and equal $a$—not jagged up-and-down fluctuations, but a rapid increase or decrease similar to the S-curves shown above. Try to explain why this happens. Then set the $a$ values equal for both types of agents, increase the radius of one type to the maximum, and observe what happens.

Apart from the emergent patterns resulting from the above scenarios, one significant emergent feature will keep appearing in the world. There are only two active agent types in the model—red and yellow. However, every time two different types of information collide, the observer will notice a third type appearing right at the information front. These new agents, colored orange, are by-products of the information competition and possess both types of information simultaneously. When two information types collide, they interpenetrate each other, producing new agents that were unexpected. Information behaves like waves, not tectonic plates crashing into each other. They strive to occupy empty space (the minds of the population) and search for places to penetrate. Play with the $a$ and radius parameters of each agent type and observe how interpenetration changes.

The observer may also notice that at the end of the simulation, some grey agents remain. This is because not everyone becomes informed in reality, and there will always be "ignorant" agents even among a well-informed population. However, this can change, as variations in parameters such as $a$ and radius can significantly affect the state of "ignorance." Keep playing with the sliders and observe the plots.

These are only a few examples and experiments the observer can conduct with the model. There are numerous other interesting patterns to explore, and the author believes this model has great potential for revealing many more emergent patterns resulting from interactions, spatial features, and competition in the diffusion of information.

## Note on Limitations

No model can capture all the complexity of the real world — they are simplifications. But even though models are inherently “wrong,” as George E. P. Box famously said, they can still provide valuable insights and be **useful** for understanding patterns, making predictions, and guiding decisions. Mistakes in the model are not only possible — they are expected in any rational exploration of complex systems.

## Citation

If you are going to use this for your research or projects, please consider citing it as: DiscoCat-Dev. (2025). Information Diffusion Model: Simulating the Competition of Multiple Information Sources. [GitHub Repository](https://github.com/DiscoCat-Dev/InformationDiffusion). 

## Miscellaneous

The **model** was created and tested on NetLogo 6.3.0.
