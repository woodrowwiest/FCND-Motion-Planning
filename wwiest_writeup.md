## Project: 3D Motion Planning
April 2019\
Woodrow Wiest\
Göteborg, Sweden\
wwiest .at. gmail.com - [GitHub](https://github.com/woodrowwiest)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
    - *finished*
2. Discretize the environment into a grid or graph representation.
    - *finished* - *would like to do more work here*
3. Define the start and goal locations.
    - *finished* - *would like to do more work here*
4. Perform a search using A* or other search algorithm.
    - *finished*
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
    - *finished*
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
    - *finished*
7. Write it up.

### Starter Code Explained
See : [wwiest_starter_code_explainer.md](https://github.com/woodrowwiest/FCND-Motion-Planning/blob/master/wwiest_starter_code_explainer.md)

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points

### Writeup
*Below is described how I addressed each rubric point and where in my code each point is handled.*

### Implementing the Path Planning Algorithm

#### 1. Set global home position
Read the first line of the csv file, extract lat0 and lon0 as floating point values and use the `self.set_home_position()` method to set global home.  I used [this](https://docs.python.org/3.7/library/csv.html) csv package to parse `colliders.csv`

```
import csv

with open('colliders.csv', 'r') as f:
    reader = csv.reader(f, delimiter=',')
    coords = next(reader)

lat0, lon0 = float(coords[0][5:]), float(coords[1][6:])
```

#### 2. Set current local position
Using the above we can determine the drone's local position relative to global home with the following code.
```
self.set_home_position(lon0, lat0, 0)
```


#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location.
```
grid_start = (int(-north_offset + current_local_position[0]),
              int(-east_offset + current_local_position[1]))
```

#### 4. Set grid goal position from geodetic coordinates
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.
```
global_goal = [-122.393932, 37.79731, 0]

local_goal = global_to_local(global_goal, self.global_home)

grid_goal = (int(-north_offset + local_goal[0]),
             int(-east_offset + local_goal[1]))
```

#### 5. Modify A* to include diagonal motion
Modified the code in wwiest_planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2) by adding the next four lines to `Action()`


```
    NORTH_WEST = (-1, -1, np.sqrt(2))
    NORTH_EAST = (-1, 1, np.sqrt(2))
    SOUTH_WEST = (1, -1, np.sqrt(2))
    SOUTH_EAST = (1, 1, np.sqrt(2))
```
And add the following to `valid_actions()`
```
    if (x - 1 < 0 or y - 1 < 0) or grid[x - 1, y - 1] == 1:
        valid_actions.remove(Action.NORTH_WEST)
    if (x - 1 < 0 or y + 1 > m) or grid[x - 1, y + 1] == 1:
        valid_actions.remove(Action.NORTH_EAST)
    if (x + 1 > n or y - 1 < 0) or grid[x + 1, y - 1] == 1:
        valid_actions.remove(Action.SOUTH_WEST)
    if (x + 1 > n or y + 1 > m) or grid[x + 1, y + 1] == 1:
        valid_actions.remove(Action.SOUTH_EAST)
```


#### 6. Cull waypoints 
Use a collinearity test to prune the path of unnecessary waypoints. 
```
def collinearity_check(p1, p2, p3, epsilon=1e-6):
    """
    Takes three points as arguments to calculate determinant of a matrix
    Returns True only if determinate absolute value is less than epsilon threshold
    Called by pruned_path()
    """
    mat = np.concatenate((p1, p2, p3), 0)
    det = np.linalg.det(mat)
    return abs(det) < epsilon


def prune_path(path):
    pruned_path = [p for p in path]

    i = 0
    while i < len(pruned_path) - 2:
        p1 = point(pruned_path[i])
        p2 = point(pruned_path[i+1])
        p3 = point(pruned_path[i+2])

        if collinearity_check(p1, p2, p3):
            pruned_path.remove(pruned_path[i+1])
        else:
            i += 1
    return pruned_path
```


### Execute the flight
#### 1. Does it work?
Yes.

This initial commit uses the bare minimum systems required to get the drone going to pass project evaluation with respect to project timing deadlines.  I have all intent to continue this project until there is an efficient drone flight algorithm.

