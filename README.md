# Xpilot Obstacle Course Project

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

## Progress Update 4/29/20
I have finalized the first version of the map I intend to use for my obstacle course. At this point the next major step is to get my production system working. To first test my production system I will use a simple map that doesn't have obstacles yet to make sure the system functions properlly in avoiding walls and attaching to the ball. At this point I believe I know what my fitness function will be, so once I have the production system complete, I will determine what the chromosome will be and begin the training of the Agent.

Here is an image of the map as it is seen in the mapedit program. The agent spawns at the base on the bottom right corner of the map, right next to the ball as the intent is not for the agent to have to track down the ball. In the upper left corner is the destination the agent is trying to get to. Currently the destination is represented by the red square but in the code it will just be a square of coordinates the agent needs to reach. 
![map image](https://user-images.githubusercontent.com/63681412/80630848-d973fd80-8a22-11ea-9855-871570bc5760.png)

## Progress Update 5/6/20
With the production system complete and the chromosome determined, it is now time to begin running the genetic algorithm to train the production system parameters. The biggest issue I've been working to solve is how to get the ball to respawn once the agent dies and respawns. I have solved this issue by turning teams on and assigning the ball to the opponent team so the game recognizes it as a "capture the flag" game so the ball respawns when the team (the agent) dies. With this sorted out training can begin. 

### Production system
```
//sets power relative to speed
		setPower(minPower+(selfSpeed()*1.25));

		//turning rules
		if (selfSpeed() > brakeSpeed){
			turnToDeg(tracking+180);
		}else if (rightWall < wallsCloseR){
			turnLeft(1);
		}else if (frontWall < wallsCloseF && rightWall < leftWall){
			turnLeft(1);
		}else if (frontWall < wallsCloseF){
			turnRight(1);
		}else if (leftWall < wallsCloseL){
			turnRight(1);
		}else if (trackWall < wallsCloseT && rightWall < leftWall){
			turnToDeg(tracking-180);
		}else if (trackWall < wallsCloseT){
			turnToDeg(tracking+180);
		}else{
			//begin turning towards goal
			double x1 = (double) selfRadarX();
			double y1 = (double) selfRadarY();
			double x2 = goalX;
			double y2 = goalY;
			double dx = Math.abs(x2-x1);
			double dy = Math.abs(y2-y1);

			if(x2>x1 && y2>y1){  //goal is upper right of current position
				turnToDeg(radToDeg(Math.atan((dy)/(dx))));
			}else if(x2>x1 && y2<y1){  //goal is below right of current position
				turnToDeg(360-radToDeg(Math.atan((dy)/(dx))));
			}else if(x2<x1 && y2<y1){  //goal is below left of current position
				turnToDeg(180+radToDeg(Math.atan((dy)/(dx))));
			}else if(x2<x1 && y2>y1){  //goal is upper left of current position
				turnToDeg(180-radToDeg(Math.atan((dy)/(dx))));
			}
		}

		//thrusting rules
		if (selfSpeed() > brakeSpeed && heading > tracking+150 && heading < tracking+210){
			thrust(1);
		}else if (minBackArc < wallsCloseB){
			thrust(1);
		}else if (heading > tracking+150 && heading < tracking+210){
			thrust(1);
		}else if (selfSpeed() > maxSpeed){
			thrust(0);
		}else if (rightWall < wallsCloseR){
			thrust(0);
		}else if (frontWall < wallsCloseF){
			thrust(0);
		}else if (leftWall < wallsCloseL){
			thrust(0);
		}else{

			thrust(1);
		}

```


### Chromosome
For the chromosome I decided to have 8 parameters for the GA to learn;

| Parameter     | Bits Used |  Bit Values   |    Function   | Parameter Values |                  Parameter Description               |
| ------------- | --------- | ------------- | ------------- | ---------------- | ---------------------------------------------------- |
| Max Speed     |    4      | 0-15          |     +1        |     1-16         | Max speed the agent will go                          |
| Brake Speed   |    4      | 0-15          |     +1        |     1-16         | Once going above this speed, the agent brakes        |
| WallsCloseR   |    5      | 0-31          |     x16       |     0-496        | Distance a wall to the right is considered close     |
| WallsCloseL   |    5      | 0-31          |     x16       |     0-496        | Distance a wall to the left is considered close      |
| WallsCloseB   |    5      | 0-31          |     x16       |     0-496        | Distance a wall behind is considered close           |
| WallsCloseF   |    5      | 0-31          |     x16       |     0-496        | Distance a wall in front is considered close         |
| WallsCloseT   |    5      | 0-31          |     x16       |     0-496        | Distance a wall being tracked to is considered close |
| Min Power     |    5      | 0-31          |               |     0-31         | Minimum thrusting power                              |

This creates a 38 bit chromosome, which is quite managable. 

### GA
For the GA I used a population of 50 running for 50 generations. I used a mutate chance of 1/100 . Crossover was done with single point crossover.

## Final Result
<video src="Xpilot Obstacle Course Agent.mp4" width="320" height="200" controls preload></video>

The final chromosome learned is 11011010001001111100011101110011000100.
Which is converted into the parameter values (in order as listed in table):  14, 11, 64, 496, 48, 368, 96, 4

*-Riah*
