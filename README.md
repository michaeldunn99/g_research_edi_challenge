# Assigning tasks
In this challenge, your goal is to implement a strategy that schedules jobs to processors.
These jobs (referred to as tasks in the problem statement) arrive at different points in time.
The produced schedule should respect certain constraints.

The problem is described through several examples, which gradually introduce the constraints and properties of each of the tasks and processors.

## Setting
You are given a set of T tasks that need to be allocated to one of P processors.
Each processor can only work on one task at a time and scheduled tasks cannot be interrupted: they are run until the task is completed.
Both tasks and processors are 0-indexed: tasks 0, 1, …, T-1; processors 0, 1, …, P-1.

Tasks have durations, specified by numbers in a list duration.
These durations tell us how long a task takes to complete once scheduled on a processor.
For example, `duration = [5,3,4]` means that task 0 takes 5 ticks to finish once scheduled, task 1 takes 3 ticks, and task 2 takes 4 ticks.

Tasks have arrival times given in the list arrival; these are the earliest times a job can be cheduled on any processor.
For example, if `arrival[5] == 7`, that means that it is impossible to schedule task 5 before the time corresponding to 7 ticks since the start of the scenario.

Time is 0-indexed: there are no tasks initially scheduled on any processors, and no task is available before time 0.

# Score, affinities, bonus
Once tasks arrive, there is a preference for scheduling them as soon as they arrive: the earlier they are scheduled, the higher the score.
The exact scoring formula relies on the following lists: reward, factor, bonus and time_bonus.
These are described in turn.

Completing each task yields a reward; the list reward provides these rewards for each task if it is completed under circumstances described later.
For example, `reward = [10,15]` means that completing task 0 yields a reward of 10 points, and completing task 1 yields a reward of 15 points.
Note that these numbers are base rewards, since other factors can make the score gain larger or smaller than reward[i].

Tasks have affinities towards being run on different processors, which are given in the list of lists factor.
The i-th element of factor is a list factor[i], whose length is P and whose values tell us if scheduling task i on a particular processor gives more points.
For example, `factor[4] = [6, 3]` tells us that scheduling task 4 on processor 0 would give us twice as many points than if we had scheduled it on processor 1, assuming all else equal (meaning, scheduled at the same time).

Scheduling a task as early as possible gives you additional bonus points.
Two lists describe these bonuses: time_bonus and bonus.
If task i is scheduled within time_bonus ticks of its arrival, you gain additional bonus points.

These are all of the parts of the scoring equation.
The reward received when task i is assigned to processor p at time t is given by the following formula:

- if `t < arrival[i] + time_bonus[i]`, the reward received is `factor[i][p] * (bonus[i] + reward[i] * duration[i] / (duration[i] + t - arrival[i]))`
- otherwise, the reward is `factor[i][p] * reward[i] * duration[i] / (duration[i] + t - arrival[i])`

# Implementation
You need to implement only one function, assign_tasks. This function expects the following arguments:

- factor: a matrix of affinities (represented as a list with T elements, each a list of length P); all of the values are non-negative real numbers
- arrival: a list of arrival times (of length T); all of the values are non-negative integers
- bonus: a list of bonus reward amounts (of length T); all of the values are positive integers
- reward: a list of rewards for completing tasks (of length T); all of the values are positive integers
- duration: a list of durations of tasks (of length T); all of the values are positive integers
- time_bonus: a list of times within which tasks should be scheduled in order to receive a bonus
(of length T); all of the values are positive integers
The output of the function should be a schedule representing task assignments, represented as a list of length T whose items are pairs (p, t) representing the assigned processor and time when the task is scheduled. The numbers t should be positive integers.

Scoring
Your solution will be evaluated based on the total reward your schedule achieves and the reward achieved by a reference solution. The reference solution receives slightly more than 80 points. You will receive 0 points on an example if your schedule violates the required constraints (for example, a processor handles two jobs at the same time).

Your solution will be evaluated on four examples, satisfying the following constraints:

- test 1: `P = 1, T = 200, max{arrival[i], duration[i], time_bonus[i]} <= 500`
- test 2: `P = 3, T = 200, max{arrival[i], duration[i], time_bonus[i]} <= 100`
- test 3: `P = 10, T = 200, max{arrival[i], duration[i], time_bonus[i]} <= 500`, the matrix factor has all 1s (in other words, there are no special affinities)
test 4: P = 10, T = 200, max{arrival[i], duration[i], time_bonus[i]} <= 500
The first test is given to you in its entirety, whereas the others are not.
The available test, given in the attached test1.test file, has each of the arguments to assign_task in a separate line (in the order in which they are expected by assign_task).
The final line contains the reference score against which your solution is evaluated.

Your solution does not need to check that the constraints stated in the problem description are satisfied (for example, that none of the numbers are negative).

You can submit multiple solutions.
Only the most recently submitted solution counts in the end.
There is no penalty for submitting multiple solutions.

## Example
assign_tasks([[1]], [2], [3], [4], [5], [6]) is scheduling a single task to a single processor.

The task has affinity of 1 towards that processor, arrives at time 2, has a bonus factor of 3, a reward of 4, a duration of 5, and a bonus time of 6.

If the function returns [(0, 3)], it signifies that you are assigning task 0 (the first and only task) to processor 0, at time 3.

Since it was scheduled within the bonus time (3 < 2 + 6), it has the bonus factor applied to it,
1 * (3 + (4 * 5) / (5 + 3 - 2))
giving a score of 6.33.

On the other hand, if the function returns [(0, 2)], the task is assigned at time 2.
The score is then computed as:
1 * (3 + (4 * 5) / (5 + 2 - 2))
giving a score of 7.
This is the optimum solution and the highest achievable score.