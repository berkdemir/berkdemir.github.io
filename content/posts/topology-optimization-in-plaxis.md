---
title: "Topology Optimization in Plaxis"
date: 2021-12-23T00:00:00+00:00
draft: false
---

Topology optimization is very similar to sculpting. You give a bulk volume to the algorithm and some constraints, it gives you the best shape that it can find.
It is possible to use this in any software, as long as you can calculate the stresses - or any other criteria that you can use to eliminate the unnecessary volumes.
I have been planning to try this in Plaxis for a long time. Using a couple of hours of coding and trials, it turns out Plaxis - Python connection works pretty well for topology optimization. This code lacks too much such as unstable volume check (stiffness matrix errors) or stop-criteria. But it was fun.
<video controls style="max-width:100%"><source src="_assets/TopologyOptimization.mp4" type="video/mp4"></video>
