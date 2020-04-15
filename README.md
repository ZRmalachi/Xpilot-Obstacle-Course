# Welcome to my Project Site!

## Basic Project Description
For my Xpilot-AI project I am planning on creating an obstacle course or maze that a Genetic Algorithm will learn how to navigate while carrying the ball from Capture the Flag without crashing or dropping the ball. I will first create a production system to progress through the course and then use a GA to learn the parameters. 

I plan on having different checkpoints set up for the agent to try to reach which will lead the agent through the course. Or, if I do a maze, it will just have an endpoint the agent needs to reach. I’m not sure how well it will learn a maze, so I will likely just stick to the obstacle course idea, I will do a bit of research into xpilot mazes to see if anything similar has been done before. 

I’m thinking of evaluating fitness by how many checkpoints it hits, how close it gets to the next checkpoint, and will possibly add some evaluation of the time it takes to complete to try to make it go through as quickly as possible.

My first steps are going to be to find a map that I want to start working in and to create the production system for my agent that navigates and avoids the walls. If I can't find a map I'm content with I plan to create my own that satisfies what I want to accomplish. 

## Future Steps

### Phase 1
My first steps are going to be to find a map that I want to start working in and to create the production system for my agent that navigates and avoids the walls. If I can't find a map I'm content with I plan to create my own that satisfies what I want to accomplish. The main focus for the production system will be to avoid walls while making its way to the goal through the course.

### Phase 2
The next step will be to finalize my fitness function, decide how I intend to do crossover in the GA, and to build my chromosome. As I said above, the fitness will likely be evaluated on how long the agent spends in the course, as well as how far into the course it gets before dying. 

### Phase 3
Finally I will run the GA to train the agent and see how well what I decided on works and then tweak it as needed. I expect the first few training runs will not work the way I want so I plan on having to revisit and revise the fitness and chromosome many times during this phase. 

## Things I Still Need to Decide On
At this point I'm still not sure how I'm going to calculate fitness, nor how long the fitness will run for. I'm going back and forth about whether I should have the fitness end if the agent drops the ball or if I should give the agent the ability to pick the ball back up and continue.




*-Riah*
