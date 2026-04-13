# Week 5 Navigation and mapping

So far, we have:

- defined state  
- built models of how the robot moves  
- designed controllers to choose actions  

But we have been quietly assuming something pretty unrealistic:

> we know where the robot is

This week we remove that assumption.

---

## SLAM

Let’s start with the core problem:

> how do I know where I am if I don’t know where anything is?

This is the **SLAM problem**:

> **Simultaneous Localisation and Mapping**

We want to estimate:

- the robot state  
- the map of the environment  

at the same time.

---

### Why is this hard?

Because:

- to build a map, I need to know where I am  
- to know where I am, I need a map  

So we get a circular dependency.

---

### The key idea

We solve both problems **jointly**.

As the robot moves:

- it updates its belief about its position  
- it updates its belief about the map  
- each helps improve the other  

---

## Joint state estimation

Instead of estimating just the robot state:

$$
x = (x, y, \theta)
$$

we estimate a **joint state**:

$$
x = (\text{robot}, \text{map})
$$

For example:

$$
x = (x, y, \theta, m_1, m_2, \dots, m_n)
$$

where \(m_i\) are landmark positions.

---

### Same idea as before

Nothing fundamentally new.

Still:

$$
x_{k+1} = f(x_k, u_k)
$$

But now:

- the state is much bigger  
- uncertainty matters a lot more  

---

## EKF SLAM

One way to solve this is using the **Extended Kalman Filter (EKF)**.

---

### What EKF does

It maintains:

- a mean (best estimate of state)  
- a covariance (uncertainty)  

At each step:

1. **predict** using motion model  
2. **update** using measurements  

---

### Extending to SLAM

For SLAM:

- we include landmarks in the state  
- we update both robot and landmarks together  

When we observe a new landmark:

- we add it to the state  
- we update the covariance  

This gives us a full SLAM system

> Smith, R., Self, M., and Cheeseman, P. (1986). "On the representation and estimation of spatial uncertainty." *International Journal of Robotics Research*, 5(4), 56–68.

> Durrant-Whyte, H. and Bailey, T. (2006). "Simultaneous Localisation and Mapping (SLAM): Part I." *IEEE Robotics and Automation Magazine*, 13(2), 99–110.

---

### Key intuition

> uncertainty is shared between robot and map

If we improve one, we improve the other.

---

## Occupancy grid SLAM

Landmarks are nice.

But what if the world does not have nice discrete features?

---

### Alternative representation

We represent the world as a grid:

- each cell has a probability of being occupied  

So the map becomes:

$$
m = \{p(\text{occupied})\}_{\text{cells}}
$$

---

### What changes?

- state is now very high dimensional  
- we update probabilities instead of landmark positions  
- we often assume independence between cells  

---

### Tradeoff

- flexible representation  
- works with raw sensor data  

but:

- computationally expensive  
- loses structure  

---

## Rao-Blackwellisation

At this point things get big.

Very big.

---

### The idea

We exploit structure in the problem.

Instead of estimating everything jointly, we factor it:

- sample robot trajectories  
- conditionally estimate the map  

This is called **Rao-Blackwellisation**.

---

### Intuition

> break a hard problem into easier conditional problems

This leads to algorithms like **FastSLAM**.

> Montemerlo, M., Thrun, S., Koller, D., and Wegbreit, B. (2002). "FastSLAM: A Factored Solution to the Simultaneous Localization and Mapping Problem." In *Proceedings of the AAAI National Conference on Artificial Intelligence*, pp. 593–598.

---

## Planning

Now suppose we have a map.

We can finally ask:

> how do we get from A to B?

---

### Two broad approaches

1. **Reactive (no map)**  
2. **Map-based planning**

---

## Bug

Let’s start simple.

Assume:

- we know where the goal is  
- we can sense obstacles locally  
- we do not have a map  

---

### Idea

Move toward the goal.

If we hit something:

- follow the obstacle boundary  
- try again  

---

### Variants

- Bug 0: follow boundary until free  
- Bug 1: explore entire obstacle  
- Bug 2: return to goal line  

These are simple and surprisingly effective

> Lumelsky, V. J. and Stepanov, A. A. (1987). "Path-planning strategies for a point mobile automaton moving amidst unknown obstacles of arbitrary shape." *Algorithmica*, 2(1–4), 403–430.

---

### Limitations

- inefficient  
- no global optimality  
- can take very long paths  

---

## A* / D*

Now assume we **do have a map**.

We can turn planning into a graph problem.

---

### The idea

- discretise space into nodes  
- connect nodes with edges  
- search for shortest path  

---

### A*

A* uses:

- cost so far  
- heuristic to goal  

to efficiently find paths.

> Hart, P. E., Nilsson, N. J., and Raphael, B. (1968). "A Formal Basis for the Heuristic Determination of Minimum Cost Paths." *IEEE Transactions on Systems Science and Cybernetics*, 4(2), 100–107.

---

### D*

D* is a dynamic version:

> re-plans efficiently when the map changes

> Stentz, A. (1994). "Optimal and Efficient Path Planning for Partially-Known Environments." In *Proceedings of the IEEE International Conference on Robotics and Automation (ICRA)*, pp. 3310–3317.

---

## Probabilistic Roadmaps

Instead of a grid, we can represent space as a graph.

---

### Idea

- sample random configurations  
- keep only collision-free ones  
- connect nearby nodes  

This builds a **roadmap** of free space

> Kavraki, L. E., Švestka, P., Latombe, J. C., and Overmars, M. H. (1996). "Probabilistic roadmaps for path planning in high-dimensional configuration spaces." *IEEE Transactions on Robotics and Automation*, 12(4), 566–580.

---

### Then planning becomes

> find a path through the graph

---

### Strengths

- good for high-dimensional spaces  
- reusable roadmap  

---

### Weaknesses

- struggles with narrow passages  
- assumes static environment  

---

## RRT

Instead of building a full roadmap, we grow a tree.

---

### Idea

- start at initial state  
- sample random points  
- extend tree toward samples  

This rapidly explores space.

---

### Key property

> naturally explores large spaces efficiently

> LaValle, S. M. (1998). "Rapidly-Exploring Random Trees: A New Tool for Path Planning." Technical Report TR 98-11, Iowa State University.

---

### When do we use it?

- complex systems  
- continuous spaces  
- when planning from a single start  

---

## Big picture

Let’s zoom out.

This week connects everything:

- modelling → how the robot moves  
- estimation → where the robot is  
- mapping → what the world looks like  
- planning → what the robot should do  

---

## Final thought

Robotics is not just:

> choosing actions

It is:

> choosing actions **under uncertainty, in a world we do not fully know**

That is what makes it hard.

And also what makes it interesting.

---

## Key Papers

> Smith, R., Self, M., and Cheeseman, P. (1986). "On the representation and estimation of spatial uncertainty." *International Journal of Robotics Research*, 5(4), 56–68.

> Durrant-Whyte, H. and Bailey, T. (2006). "Simultaneous Localisation and Mapping (SLAM): Part I." *IEEE Robotics and Automation Magazine*, 13(2), 99–110.

> Montemerlo, M., Thrun, S., Koller, D., and Wegbreit, B. (2002). "FastSLAM: A Factored Solution to the Simultaneous Localization and Mapping Problem." In *Proceedings of AAAI*, pp. 593–598.

> Hart, P. E., Nilsson, N. J., and Raphael, B. (1968). "A Formal Basis for the Heuristic Determination of Minimum Cost Paths." *IEEE Transactions on Systems Science and Cybernetics*, 4(2), 100–107.

> Kavraki, L. E., Švestka, P., Latombe, J. C., and Overmars, M. H. (1996). "Probabilistic roadmaps for path planning in high-dimensional configuration spaces." *IEEE Transactions on Robotics and Automation*, 12(4), 566–580.

> LaValle, S. M. (1998). "Rapidly-Exploring Random Trees: A New Tool for Path Planning." Technical Report TR 98-11, Iowa State University.

# Coming up next

We now have:

- models  
- estimators  
- planners  

Next:

> how do we *learn* these instead of hand-designing them?

→ reinforcement learning