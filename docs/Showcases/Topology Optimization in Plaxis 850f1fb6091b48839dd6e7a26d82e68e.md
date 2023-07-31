# Topology Optimization in Plaxis

Topology optimization is very similar to sculpting. You give a bulk volume to the algorithm and some constraints, it gives you the best shape that it can find.

It is possible to use this in any software, as long as you can calculate the stresses - or any other criteria that you can use to eliminate the unnecessary volumes.

I have been planning to try this in Plaxis for a long time. Using a couple of hours of coding and trials, it turns out Plaxis - Python connection works pretty well for topology optimization. This code lacks too much such as unstable volume check (stiffness matrix errors) or stop-criteria. But it was fun.

[TopologyOptimization.mp4](Topology%20Optimization%20in%20Plaxis%20850f1fb6091b48839dd6e7a26d82e68e/TopologyOptimization.mp4)