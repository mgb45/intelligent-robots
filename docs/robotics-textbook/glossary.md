# Glossary

This glossary collects technical terms used throughout the textbook, grouped by week.

## Overview Terms

- **optimal control problem**: Finding control inputs that minimize a cost subject to system dynamics.
- **dynamics**: The rules/equations describing how state evolves over time under actions and disturbances.
- **state**: The minimal information needed to describe the system at a given time.
- **control**: Commands sent to the robot to influence future state.
- **actuators**: Physical components that convert control commands into motion/force.
- **action**: A decision variable selected by a policy (often at a higher abstraction than low-level control).
- **reward**: Scalar feedback used to measure desirability of outcomes in reinforcement learning.
- **return**: Cumulative (often discounted) sum of rewards over time.
- **reinforcement learning**: Learning a policy through interaction to maximize expected return.
- **decision making under uncertainty**: Choosing actions when state, outcomes, or models are uncertain.
- **planning**: Computing a sequence of actions/subgoals to achieve an objective.
- **subgoal**: Intermediate target used to decompose a larger task.
- **setpoint**: Desired target value/state for a controller to regulate to.
- **post condition**: Condition that should hold after executing a plan or action.
- **perception**: Inferring useful world/robot information from sensor data.
- **sensor**: Device that measures aspects of the robot or environment.
- **uncertain**: Not exactly known; represented with error bounds or probability.
- **partial observability**: Situation where the full true state cannot be directly observed.
- **probabilistic belief**: Distribution representing uncertainty about hidden state.
- **inference (inferring)**: Estimating hidden variables from observations and models.
- **sense-plan-act loop**: Control architecture cycling through sensing, planning, and acting.
- **perception-planning-control loop**: Iterative architecture coupling state inference, decision making, and control execution.
- **policy**: Mapping from state/observation (or belief) to action/control.
- **robust policy**: Policy that maintains performance under uncertainty or model mismatch.
- **adaptive policy**: Policy that updates behavior as conditions/data change.
- **autonomous system**: System that perceives, decides, and acts with limited external intervention.
- **agent**: Entity that takes actions to optimize an objective.
- **agency**: Capacity of an entity to make decisions and produce actions.
- **multi-agent system**: Environment with multiple interacting decision-making agents.

## Week 1: State Representations

- **state**: The minimal set of variables needed to describe a system at a given time.
- **reference frame**: A coordinate system used to express positions and orientations.
- **world frame**: A fixed global frame used as a common reference.
- **robot frame**: A frame attached to the robot body.
- **orientation**: The rotational part of pose.
- **pose**: Position plus orientation.
- **rotation matrix**: An orthonormal matrix that represents rotation between frames.
- **elementary rotations**: Axis-specific rotations (for example around x, y, z) used to build 3D orientation.
- **non-commutative rotations**: The property that rotation order changes the result.
- **translation**: Linear displacement by a vector.
- **rigid body motion**: Combined rotation and translation of a body with fixed shape.
- **homogeneous transform**: A matrix form that combines rotation and translation in one operation.
- **transform chaining**: Multiplying transforms to move through multiple frames.
- **TF tree**: A graph of coordinate-frame transforms used to represent robot geometry.
- **metric map**: A map with geometric distances and coordinates.
- **cost map**: A map layer assigning traversal or risk costs to space.
- **point cloud**: A set of 3D points (optionally with normals/features) representing geometry.
- **signed distance field**: A field storing distance to the nearest surface with inside/outside sign.
- **occupancy grid**: A discretized map where each cell stores occupancy probability.
- **uncertainty**: Lack of exact knowledge about state, measurements, or world structure.
- **octree**: A hierarchical tree structure for efficient 3D spatial partitioning.
- **kd-tree**: A data structure for partitioning and querying points in k-dimensional space.
- **topological map**: A graph-based map emphasizing connectivity over metric detail.
- **scene graph**: A structured graph representing objects/places and relations.
- **local planning**: Planning over a short horizon with local context.
- **global planning**: Planning over larger regions or full-map context.
- **world model**: A state representation that includes dynamics of the environment.

## Week 2: Modelling

- **configuration**: The minimal variables that fully specify robot state in its configuration space.
- **degrees of freedom (DoF)**: Number of independent coordinates needed to specify configuration.
- **configuration space (C-space)**: The space of possible robot configurations.
- **task space**: The physical space where task-relevant behavior is expressed.
- **manifold**: A lower-dimensional structure embedded in a higher-dimensional space.
- **fully actuated**: System where all DoFs are directly controllable.
- **underactuated**: System where fewer independent controls exist than DoFs.
- **motion model**: A function describing state evolution under actions/inputs.
- **discrete-time dynamics**: State evolution represented at sampled time steps.
- **continuous-time dynamics**: State evolution represented by differential equations.
- **disturbance**: Unmodeled effect acting on system dynamics.
- **bicycle model**: Kinematic abstraction for car-like steering dynamics.
- **differential drive**: Two-wheel independent-drive mobile robot model.
- **instantaneous center of rotation (ICR)**: The instantaneous pivot point of planar turning motion.
- **unicycle model**: Nonlinear planar mobile robot model with linear/angular velocity inputs.
- **state-space form**: Vector-matrix representation of dynamics.
- **linearization**: Local first-order approximation of a nonlinear model around an operating point.
- **Jacobian**: Matrix of first-order partial derivatives of a vector function.
- **observation model**: Mapping from latent state to sensor measurement.
- **latent state**: Underlying state variable not directly observed.
- **measurement noise**: Random sensor error in observations.
- **latency**: Delay between physical event and measurement availability.
- **bias**: Systematic measurement offset from true value.
- **drift**: Slow time-varying bias or accumulated error.
- **time synchronization**: Aligning sensor clocks/timestamps for consistent fusion.
- **intrinsics**: Internal sensor parameters (for example camera focal length, distortion).
- **extrinsics**: Rigid transform relating one sensor/body frame to another.
- **calibration**: Estimation of sensor/model parameters from data.
- **outlier**: Measurement inconsistent with assumed noise model.
- **gating**: Rejecting measurements based on statistical consistency thresholds.
- **RANSAC**: Robust estimator that fits models using random consensus subsets.
- **Huber loss**: Robust loss that is quadratic near zero and linear for large residuals.
- **observability**: Ability to infer hidden state from available measurements.

## Week 3: Control

- **open-loop control**: Applying precomputed actions without feedback correction.
- **feedback control**: Using measured state/output to correct actions online.
- **regulation**: Driving state to a fixed target/setpoint.
- **trajectory tracking**: Following a time-varying reference trajectory.
- **PID control**: Proportional-Integral-Derivative feedback controller.
- **proportional term (P)**: Control contribution proportional to current error.
- **integral term (I)**: Control contribution from accumulated past error.
- **derivative term (D)**: Control contribution from error rate of change.
- **integral windup**: Integrator accumulation causing excessive control after saturation/large transients.
- **setpoint**: Desired target state/value.
- **saturation**: Clamping control output to actuator limits.
- **Linear Quadratic Regulator (LQR)**: Optimal linear-state feedback under quadratic cost.
- **state error**: Difference between current and desired/equilibrium state.
- **Riccati equation**: Matrix equation solved to compute optimal LQR gains.
- **finite-horizon control**: Optimization over a fixed time horizon.
- **infinite-horizon control**: Optimization over an unbounded horizon.
- **iterative LQR (iLQR)**: Nonlinear trajectory optimization via repeated local LQR approximations.
- **forward pass**: Simulation rollout step in iterative trajectory optimization.
- **backward pass**: Dynamic-programming gain/correction computation in iLQR.
- **feedforward term**: Open-loop correction component computed by optimization.
- **feedback gain**: Closed-loop gain multiplying state deviation.
- **Model Predictive Control (MPC)**: Receding-horizon optimization executed repeatedly online.
- **receding horizon**: Apply first optimized action, then re-solve at next step.
- **state constraints**: Allowed region limits on states.
- **control constraints**: Allowed region limits on inputs.
- **quadratic program (QP)**: Optimization with quadratic objective and linear constraints.

## Week 4: State Estimation

- **ground truth**: True but typically inaccessible state.
- **belief**: Probability distribution representing state uncertainty.
- **likelihood**: Probability of an observation given a hypothesized state.
- **prior**: Belief before incorporating current measurement.
- **posterior**: Updated belief after applying Bayes rule.
- **Bayes rule**: Formula combining prior and likelihood to produce posterior.
- **marginal likelihood (evidence)**: Normalizing probability of the observation.
- **recursive Bayes filter**: Predict-update recursion for sequential state estimation.
- **prediction step**: Propagate belief through transition/motion model.
- **update step**: Correct predicted belief using measurement likelihood.
- **transition model**: Probabilistic dynamics model for state evolution.
- **histogram filter**: Discrete Bayes filter over finite state bins.
- **normalization constant**: Factor that ensures probabilities sum/integrate to one.
- **Kalman filter (KF)**: Optimal linear-Gaussian recursive estimator.
- **mean**: Expected value of a probability distribution.
- **covariance**: Second-moment uncertainty and correlation structure.
- **innovation/residual**: Difference between observed and predicted measurement.
- **innovation covariance**: Uncertainty of residual used for weighting measurements.
- **Kalman gain**: Optimal weighting between prediction and measurement.
- **Extended Kalman Filter (EKF)**: Nonlinear estimator using local linearization.
- **particle filter**: Sample-based Bayesian estimator with weighted particles.
- **resampling**: Re-drawing particles to avoid weight degeneracy.
- **sequential Monte Carlo**: Family of particle-based sequential inference methods.
- **sensor fusion**: Combining multiple sensor streams in one estimation framework.

## Week 5: Navigation and Mapping

- **SLAM**: Simultaneous localization and mapping.
- **joint state**: Combined state vector containing robot and map variables.
- **loop closure**: Reduction of accumulated uncertainty by re-observing known places/features.
- **landmark**: Distinct map feature used for localization/mapping.
- **data association**: Matching current observations to existing map entities.
- **Mahalanobis distance**: Covariance-aware distance used for statistical matching.
- **log-odds**: Logit representation of occupancy probability used for additive updates.
- **ray casting**: Tracing sensor rays through map cells for occupancy updates.
- **Rao-Blackwellization**: Factorization using conditional independence to reduce inference complexity.
- **FastSLAM**: Particle-based SLAM with per-particle landmark estimators.
- **global planner**: Planner over full map for long-horizon path generation.
- **local planner**: Short-horizon reactive planner for immediate feasible motion.
- **Bug algorithms**: Reactive obstacle-boundary-following navigation family.
- **M-line**: Straight line from start to goal used by Bug2.
- **A***: Heuristic graph-search algorithm minimizing path cost.
- **heuristic admissibility**: Heuristic property that guarantees no overestimation of true cost.
- **D***: Incremental replanning algorithm for changing/partially known maps.
- **probabilistic roadmap (PRM)**: Sampling-based graph planner for high-dimensional spaces.
- **Rapidly-exploring Random Tree (RRT)**: Incremental sampling-based tree planner.
- **RRT***: Asymptotically optimal RRT variant with rewiring.
- **Dynamic Window Approach (DWA)**: Local velocity-space planner under dynamic constraints.
- **admissible velocity set**: Velocities that allow safe stopping before collision.
- **trajectory rollout**: Simulating candidate controls over short horizon and scoring outcomes.
- **tentacle planner**: Local planning using precomputed candidate arcs.
- **navigation stack**: Integrated pipeline of estimation, mapping, planning, and control.

## Week 6: Articulated Robots and Kinematics

- **articulated chain**: Rigid links connected by joints.
- **link**: Rigid body element in a robot mechanism.
- **joint**: Mechanical connection allowing constrained relative motion.
- **revolute joint**: Joint with rotational DoF.
- **prismatic joint**: Joint with translational DoF.
- **end-effector**: Terminal link/tool that interacts with task environment.
- **Denavit-Hartenberg (DH) convention**: Standard parameterization for serial-chain kinematics.
- **DH parameters**: Geometric parameters (a, alpha, d, theta) defining adjacent-link transforms.
- **URDF**: Unified Robot Description Format for robot structure, geometry, and properties.
- **visual geometry**: Mesh/shape used for rendering robot appearance.
- **collision geometry**: Simplified shape used for collision checks.
- **inertial parameters**: Mass properties including COM and inertia tensor.
- **forward kinematics (FK)**: Compute end-effector pose from joint configuration.
- **workspace**: Set of poses reachable by some configuration.
- **inverse kinematics (IK)**: Compute joint configuration that achieves desired pose.
- **kinematic redundancy**: More DoFs than needed for primary task constraints.
- **analytical IK**: Closed-form symbolic IK solution for specific robot geometries.
- **numerical IK**: Iterative optimization/linearization-based IK.
- **resolved-rate control**: IK strategy using differential kinematics and Jacobian.
- **Jacobian pseudoinverse**: Least-squares inverse mapping from task velocity to joint velocity.
- **singularity**: Configuration where Jacobian loses rank and motion authority degrades.
- **joint limits**: Physical bounds on joint positions/velocities.

## Week 7: Dynamics

- **manipulator equations**: Standard joint-space dynamics equation for robot arms.
- **mass matrix**: Configuration-dependent inertia mapping from acceleration to torque.
- **Coriolis/centripetal term**: Velocity-dependent dynamic coupling term.
- **gravity vector**: Joint torques required to counter gravity.
- **joint torque**: Actuation torque applied at joints.
- **forward dynamics**: Compute accelerations from current state and applied torques.
- **inverse dynamics**: Compute torques required for desired motion.
- **recursive Newton-Euler algorithm**: O(n) method for inverse dynamics of serial chains.
- **computed torque control**: Model-based controller canceling dynamics and adding feedback.
- **dynamic decoupling**: Transforming coupled nonlinear dynamics into simpler independent forms.
- **system identification**: Estimating model parameters from motion/force data.
- **regressor form**: Dynamics expressed linearly in unknown parameters.
- **persistently exciting trajectory**: Motion rich enough to identify parameters reliably.
- **robot calibration**: Estimation of geometric model parameters for accuracy.
- **contact dynamics**: Dynamics when bodies interact through constraints and forces.
- **unilateral constraint**: Contact can push but not pull.
- **complementarity condition**: Mathematical condition coupling contact gap and normal force.
- **Coulomb friction model**: Friction cone model limiting tangential force by normal force.
- **Linear Complementarity Problem (LCP)**: Optimization/feasibility form common in rigid contact simulation.
- **sim-to-real gap**: Performance drop when transferring from simulation to physical robot.
- **domain randomization**: Randomizing simulation parameters during training for robustness.

## Week 8: Perception and Learning

- **robot learning**: Learning perception, dynamics, or policy components from data.
- **supervised learning**: Learning from labeled input-output examples.
- **dataset**: Collected examples used for model fitting and validation.
- **loss function**: Objective minimized during model training.
- **classification**: Predicting discrete labels from inputs.
- **regression**: Predicting continuous outputs from inputs.
- **segmentation**: Predicting per-pixel or per-point labels.
- **feature representation**: Learned or engineered input abstraction for downstream tasks.
- **latent variable**: Hidden variable inferred from observations.
- **dynamics learning**: Learning transition models from state-action-next-state data.
- **residual dynamics model**: Learned correction added to known physics model.
- **probabilistic dynamics model**: Transition model that outputs uncertainty/distribution.
- **imitation learning**: Learning policy behavior from demonstrations.
- **behavior cloning**: Supervised imitation mapping observations directly to expert actions.
- **covariate shift**: Distribution mismatch between training states and deployment states.
- **DAgger**: Dataset Aggregation method reducing imitation-learning distribution shift.
- **action representation**: Chosen control parameterization predicted by a learned policy.
- **stochastic policy**: Policy outputting a distribution over actions.
- **mixture density network (MDN)**: Model outputting mixture distributions for multimodal targets.
- **diffusion policy**: Policy that samples actions through iterative denoising.
- **motion primitive**: Structured reusable movement pattern.
- **Dynamic Movement Primitive (DMP)**: Stable attractor-based movement primitive with learned forcing term.
- **probabilistic movement primitive**: Distributional extension of movement primitives.
- **Decision Transformer**: Sequence model treating control as conditional sequence prediction.
- **vision-language-action (VLA) model**: Policy conditioned on visual observations and language commands.
- **inductive bias**: Structural assumptions that improve generalization/data efficiency.
- **closed-loop evaluation**: Testing learned models in feedback execution, not only offline metrics.

## Week 9: Reinforcement Learning

- **reinforcement learning (RL)**: Learning policies from interaction to maximize long-term reward.
- **agent**: Decision-maker that acts in an environment.
- **environment**: External process generating transitions and rewards from actions.
- **Markov Decision Process (MDP)**: Formal model of fully observable sequential decision making.
- **state space**: Set of possible states in an MDP.
- **action space**: Set of allowable actions.
- **transition probability**: Probability of next state conditioned on current state/action.
- **reward function**: Scalar signal evaluating transitions/actions.
- **discount factor**: Weighting factor for future rewards.
- **Markov property**: Future depends only on present state and action, not full history.
- **return**: Discounted cumulative future reward.
- **Partially Observable MDP (POMDP)**: Decision model with hidden true state and partial observations.
- **belief state**: Probability distribution over hidden states.
- **value function**: Expected return from a state under a policy.
- **action-value function (Q-function)**: Expected return from taking action in state then following policy.
- **Bellman equation**: Recursive relation defining value functions.
- **Bellman optimality equation**: Recursive condition for optimal value.
- **temporal-difference (TD) learning**: Value learning via bootstrapped one-step targets.
- **TD error**: Difference between predicted value and bootstrapped target.
- **dynamic programming**: Exact MDP solution methods with known model.
- **value iteration**: Bellman-optimality fixed-point iteration over values.
- **policy iteration**: Alternate policy evaluation and policy improvement.
- **Q-learning**: Off-policy TD method for learning optimal Q-function without model.
- **off-policy learning**: Learning one policy while following another behavior policy.
- **epsilon-greedy exploration**: Random-action exploration mixed with greedy exploitation.
- **Deep Q-Network (DQN)**: Neural-network approximation of Q-values.
- **experience replay**: Random minibatch training from stored transitions.
- **target network**: Delayed Q-network used to stabilize training targets.
- **actor-critic**: RL framework with separate policy (actor) and value estimator (critic).
- **advantage**: Relative action quality compared to baseline state value.
- **Proximal Policy Optimization (PPO)**: Policy-gradient method with clipped updates for stability.
- **sim-to-real transfer**: Deploying policies trained in simulation onto hardware.
- **reward engineering**: Designing reward functions that produce intended behavior.
- **credit assignment**: Determining which past actions caused delayed outcomes.

## Week 10: Human-Robot Interaction (HRI)

- **human-robot interaction (HRI)**: Design and analysis of robot behavior with/around people.
- **human factors**: Human capabilities/limitations relevant to system design.
- **proxemics**: Social use of interpersonal distance.
- **BDI model**: Belief-Desire-Intention cognitive architecture for goal-directed agents.
- **intent inference**: Estimating a human's likely goal/purpose from observations.
- **human motion prediction**: Forecasting future human trajectories under uncertainty.
- **Social Force Model**: Crowd/pedestrian model using attractive/repulsive interaction forces.
- **inverse reinforcement learning (IRL)**: Inferring reward/objective from demonstrated behavior.
- **safety-rated monitored stop**: Safety mode where robot halts when humans enter defined area.
- **speed and separation monitoring**: Adjusting robot speed based on human distance.
- **power and force limiting**: Collaborative safety mode limiting collision forces.
- **compliance control**: Control that yields under interaction forces instead of rigidly resisting.
- **impedance control**: Force-motion behavior shaped as virtual mass-spring-damper.
- **chance constraint**: Probabilistic safety constraint (for example collision probability bound).
- **shared control**: Human and robot jointly influence action execution.
- **predictability**: Ease with which humans can anticipate robot behavior.
- **legibility**: Robot behavior designed to communicate intent clearly.
- **transparency**: Clarity of robot internal state, intention, and decision rationale to users.
- **trust calibration**: Aligning human trust with true robot capability.
- **technology acceptance**: User willingness to adopt/use a system.
- **Uncanny Valley**: Drop in comfort/acceptance for near-human-but-not-quite agents.
- **multi-agent system**: System of interacting decision-making agents.
- **task allocation**: Assignment of subtasks across humans and robots.
- **coordination**: Temporal and behavioral alignment across team members.
- **situation awareness**: Understanding of environment, task state, and team status.
- **handover**: Coordinated transfer of object/control between human and robot.
- **affective computing**: Modeling or recognizing emotion/affective state in interaction.
- **personalization**: Adapting robot behavior to individual user preferences/capabilities.
- **Wizard of Oz (WoZ) study**: HRI study where hidden human operator simulates autonomy.
- **ecological validity**: Extent to which study findings generalize to real-world settings.
- **NASA-TLX**: Standard questionnaire for perceived workload measurement.

