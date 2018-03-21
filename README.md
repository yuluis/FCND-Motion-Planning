# FCND - 3D Motion Planning
![Quad Image](./misc/enroute.png)



This project is a continuation of the Backyard Flyer project where [project rubric](https://review.udacity.com/#!/rubrics/1534/view) details a passing submission.

This project is a modified clone of Udacity supplied code and completed on a local machine and verified using FCND-Sim_Windows_64-bit.exe drone simulator over local Mavlink
connection and supplied Drone class.

`motion_planning.py` is basically a modified version of `backyard_flyer.py` that leverages some extra functions in `planning_utils.py`. It should work right out of the box.  

The planning algorithm includes following:

- Load the 2.5D map in the `colliders.csv` file in ModtionPlanning class procedure plan_path. The token split command is used to parse.
- Discretize the environment into a grid or graph representation. Inside planning_util.py, procedure create_grid discretizes and populates obstacles onto grid.
- Define the start and goal locations. You can determine your home location from `self._latitude` and `self._longitude`. The start location is set as the map center. The goal location accepts geodetic coordinates and converts to local coordinates onto the ECEF frame NEU (north east up).  The home location is set as the start location.  The collision data grid dimensions are translated such that the origin starts at (0,0) and only the 1st quadrant is used i.e. positive north and east integer coordinates.
- Perform a search using A* or other search algorithm. A* is implemented in the planning_utils.py file and the Action and valid actions are enhanced to support diagonal motion.  The north and south sense was reversed in the starter code and this has been corrected.
- A collinearity_check procedure was copied over from previous exercise and used to remove unnecessary waypoints.
- Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0]). Waypoints are adjusted to the drone's local reference frame in line 182 of motion_planning.py.  Note that while the drone has a local frame where home is (north=0,east=0), the grid and calculations are performed on a reference frame where the drone is centered on (316, 445) as to simplfy our A* computations using just natural numbers. The altitude was fixed at 5 and heading was altered for this project.

Some of these steps are already implemented for you and some you need to modify or implement yourself.  See the [rubric](https://review.udacity.com/#!/rubrics/1534/view) for specifics on what you need to modify or implement.

Where a start GPS coordinate provided was outside the collider space, a randomly generated grid location is chosen.  

A safety margin of 5 meters has been added during the generation of the grid in planning_utils.py procedure create_grid.

