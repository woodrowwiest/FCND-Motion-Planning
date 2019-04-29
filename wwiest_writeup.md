## Project: 3D Motion Planning
April 2019\
Woodrow Wiest\
Göteborg, Sweden\
wwiest .at. gmail.com - [GitHub](https://github.com/woodrowwiest)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
    - **done**
2. Discretize the environment into a grid or graph representation.
    - **done** - *would like to do more work here*
3. Define the start and goal locations.
    - **done** - *would like to do more work here*
4. Perform a search using A* or other search algorithm.
    - **done**
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
    - **done**
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.

### Explain the Starter Code
See : [wwiest_starter_code_explainer.md](https://github.com/woodrowwiest/FCND-Motion-Planning/blob/master/wwiest_starter_code_explainer.md)

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Below is described how I addressed each rubric point and where in my code each point is handled. 

---
### Writeup

#### 1. TODO: Provide a Writeup / README that includes all the rubric points and how you addressed each one.
You're on it, keep on reading.


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.


#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.


#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!


#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.


#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Modified the code in wwiest_planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2). Explain the code you used to accomplish this step.


#### 6. Cull waypoints 
Use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.



### Execute the flight
#### 1. Does it work?
Yes.

This initial commit uses the bare minimum systems required to get the drone going to pass project evaluation with respect to project timing deadlines.  I have all intent to continue this project until there is an efficient drone flight algorithm

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.




[]: https://github.com/woodrowwiest