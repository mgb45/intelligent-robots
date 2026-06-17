# Week 2 Modelling mobile robots

Last week we talked about state representations, but this week we ask a slightly more fundamental question:

> how does our robot move?

This is the general **modelling problem**.

## Configuration and task space

Let’s start with some definitions.

The **configuration** of a robot is the minimal set of variables that completely describe its state. For a mobile robot, this is usually:

$$
\mathbf{x} = (x, y, \theta)
$$

- $(x, y)$: position in the world  
- $(\theta)$: orientation  

So this robot has **3 degrees of freedom**.

### A slightly strange example: a train

A train moves in 3D space.

But its configuration is just:

$$
q = \text{distance along the track}
$$

So it has **1 degree of freedom**, even though it exists in a 3D world. Why? Because it is constrained to move along the tracks.

### Configuration vs task space

This leads to an important distinction:

- **Configuration space**: what the robot *can do*  
- **Task space**: where the robot *exists*  

For the train:
- task space = 2D or 3D world  
- configuration space = 1D curve  

So the robot is moving on a **manifold** inside the task space.

### Fully actuated vs underactuated

Another important idea:

- **Fully actuated**: we can directly control all degrees of freedom  
- **Underactuated**: we cannot  

Example:

A hovercraft has:
- configuration: $(x, y, \theta)$ → 3 DOF  
- actuators: 2  

So it is **underactuated**. This is very common in robotics - we often need to move a robot in ways we can't directly control.

## Motion models

A **motion model** describes how the state or robot representation evolves over time:

$$
x_{k+1} = f(x_k, u_k)
$$

or in continuous time:

$$
\dot{x}(t) = f(x(t), u(t))
$$

This is just a more concrete version of what we wrote in Week 1.

### What goes into a motion model?

- state $x$: often position, orientation, velocity  
- control $u$: motor commands, steering angle, etc  
- disturbances $\epsilon$: things we don’t control  

So really:

$$
x_{k+1} = f(x_k, u_k, \epsilon_k)
$$ 

## Bicycle models

A very common model for cars is the **bicycle model**. We simplify the car to:

- one front wheel (steering)
- one rear wheel (driving)

> Rajamani, R. (2011). *Vehicle Dynamics and Control* (2nd ed.). Springer.

The state of this simplified vehicle is:
$$
(x, y, \theta)
$$

and the controls or inputs are:
- $v$: forward velocity  
- $\delta$: steering angle  

A key constraint is that the robot cannot move sideways because T=there is no way to directly generate motion in the lateral direction. This means the system is constrained and underactuated

### Motion model

The motion looks like:

$$
x_{k+1} = x_k + v_k \cos(\theta_k) dt
$$

$$
y_{k+1} = y_k + v_k \sin(\theta_k) dt
$$

$$
\theta_{k+1} = \theta_k + \frac{v_k}{L} \tan(\delta_k) dt
$$ 

Intuitively, velocity moves you forward in the direction you are facing, steering changes your heading and curved motion comes from combining both.  

## Differential drive robots

Now let’s look at a robot closer to what you will actually use. A **differential drive robot** has two wheels that are independently controlled. 

> Siegwart, R., Nourbakhsh, I. R., and Scaramuzza, D. (2011). *Introduction to Autonomous Mobile Robots* (2nd ed.). MIT Press.

The configuration is the same as before:
$$
(x, y, \theta)
$$
And for controls we often (sometimes we directly think about wheel velocities) use:
- $v$: linear velocity  
- $\omega$: angular velocity  
so
$$
u = (v, \omega)
$$

### Motion model

If the robot moves forward and rotates we can describe it's motion as

$$
\theta_{k+1} = \theta_k + \omega_k dt
$$

$$
x_{k+1} = x_k + v_k \cos(\theta_k) dt
$$

$$
y_{k+1} = y_k + v_k \sin(\theta_k) dt
$$

When turning the robot follows a circular arc, rotating around an **instantaneous centre of rotation (ICR)**. TThe turning radius is

$$
R = \frac{v}{\omega}.
$$

When $\omega = 0$, the motion is a straight line, so the radius is effectively infinite.

A more exact kinematic update over one time step is

$$
\begin{bmatrix}
x_{k+1} \\
y_{k+1}
\end{bmatrix}=
\begin{bmatrix}
x_k \\
y_k
\end{bmatrix}
+
R
\begin{bmatrix}
-\sin(\theta_k) + \sin(\theta_{k+1}) \\
\cos(\theta_k) - \cos(\theta_{k+1})
\end{bmatrix}.
$$

This expression makes the circular arc motion explicit. The vehicle is not translating independently in $x$ and $y$; instead, its position evolves according to a rigid-body rotation about an instantaneous center of curvature.

To connect this with rigid-body motion we saw last week and to prepare for control (next week), we can write the dynamics in *state-space* form. Let

$$
\mathbf{x}_k =
\begin{bmatrix}
x_k \\
y_k \\
\theta_k
\end{bmatrix},
\qquad
\mathbf{u}_k =
\begin{bmatrix}
v_k \\
\omega_k
\end{bmatrix}.
$$

For small time step $\Delta t$, the nonlinear unicycle model is

$$
\mathbf{x}_{k+1}
=
\mathbf{x}_k
+
\Delta t
\begin{bmatrix}
v_k \cos \theta_k \\
v_k \sin \theta_k \\
\omega_k
\end{bmatrix}.
$$

This is a discrete-time rigid-body motion model. Around a nominal trajectory $(\bar{\mathbf{x}}_k, \bar{\mathbf{u}}_k)$, we can linearize it as

$$
\delta \mathbf{x}_{k+1} =
A_k \delta \mathbf{x}_k
+
B_k \delta \mathbf{u}_k,
$$

where

$$
A_k =
\begin{bmatrix}
1 & 0 & -\Delta t \bar{v}_k \sin \bar{\theta}_k \\
0 & 1 & \Delta t \bar{v}_k \cos \bar{\theta}_k \\
0 & 0 & 1
\end{bmatrix},
$$

and

$$
B_k =
\begin{bmatrix}
\Delta t \cos \bar{\theta}_k & 0 \\
\Delta t \sin \bar{\theta}_k & 0 \\
0 & \Delta t
\end{bmatrix}.
$$

This gives us a locally linear model of the rigid-body dynamics. We will see next week that this is a useful form for a particular class of common control problems, the *linear quadratic regulator*.

## A differential drive robot in a ball?

Now let’s ask a slightly weird question.

> what if we put our robot inside a ball?

Does the model change? The **mechanism changes**, but the **state representation might not**. We might still describe the robot by:
$$
(x, y, \theta)
$$
But:
- the control inputs change  
- the constraints change  
- the motion model \(f(x, u)\) changes  

Fundamentally, robot modelling is about abstraction. We choose what state to represent, what controls to use and what physics to include. Different choices lead to different models and different behaviour. Some models are easier to control, or help us because they align closely with information we can sense.

## Simulators as models

So far, we have been writing down motion models like:

$$
x_{k+1} = f(x_k, u_k)
$$

and calling this a **robot model**, because our state has been something like
$$
x = (x, y, \theta)
$$
which only describes the robot. That means our model is implicitly assuming the world does not exist (or does not matter).

### Expanding the state

If we instead define a bigger state:

$$
x = (\text{robot}, \text{world})
$$

then the exact same idea becomes:

$$
x_{k+1} = f(x_k, u_k)
$$

But now, the robot moves, the world can change and interactions can happen. There is nothing new mathematically, we just changed what we decided to model.

### A simulator is just a bigger model

A simulator is simply a model with a richer state and more detailed dynamics. It might include contact, friction, collisions, sensors and other agents, but fundamentally, it is still just:

$$
x_{k+1} = f(x_k, u_k)
$$

because [everything is functions and functions describe the world](https://youtu.be/PAZTIAfaNr8).

## Final thought

So far, we have used very simple models.

That was deliberate.

Because once you understand:

> everything is just $x_{k+1} = f(x_k, u_k)$

you can scale that idea to anything:

- a wheeled robot  
- a robot in a cluttered room  
- a multi-agent system  
- a full simulator  

Same idea.

Just a bigger $x$, and a more complicated $f$. All of robotics sits on top of modelling.

Unfortunately, f your model is wrong, your controller will be wrong. And your robot will let you know.

## Key Papers

> Rajamani, R. (2011). *Vehicle Dynamics and Control* (2nd ed.). Springer.

> Siegwart, R., Nourbakhsh, I. R., and Scaramuzza, D. (2011). *Introduction to Autonomous Mobile Robots* (2nd ed.). MIT Press.

> Thrun, S., Burgard, W., and Fox, D. (2005). *Probabilistic Robotics*. MIT Press.

# Coming up next

So far we have assumed we know where the robot is.

Next:

> how do we estimate $x$ in the real world?

→ localisation and SLAM