### Starter Code Explainer
---
April 2019\
Woodrow Wiest - wwiest@gmail.com - https://github.com/woodrowwiest/FCND-Motion-Planning
#### What's going on in the provided `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that includes all necessary flight states, callbacks and transitions for an autonomous vertical takeoff and landing aerial vehicle to traverse a given preplanned path.


**Functionality**:
1. Create a log.
2. Start the connection.
3. Arm.
4. Search for a path using A* search
    - Set Global and Local Home position
    - Read map data from `colliders.csv` 
    - Build map with margins for safe flight
    - Set Start and Goal positions
5. Send waypoints to simulator
6. Takeoff
7. Navigate Waypoints 
8. Land
9. Disarm
10. Switch to manual
11. close connection

While the initially supplied `motion_planning.py` features all of the functionality of `backyard_flyer_solution.py`, there are some powerful differences implemented herein.

- A `PLANNING` state.
- A `plan_path()` function.
    - which loads `colliders.csv`
    - uses `create_grid()` function from `planning_utils.py` to build the map.
    - uses `a_star()` function from `planning_utils.py` to find a valid path from start to goal.
    - converts that path to waypoints
