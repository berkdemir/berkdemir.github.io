---
layout: default
title: Plaxis and Plate Properties
nav_order: 02
parent: Plaxis
permalink: /Plaxis/PlateProperties
---

# Plaxis and Plate Properties - A Look into the FE Adaptation of Long Term Stiffness Changes

There was a [question](https://communities.bentley.com/products/geotech-analysis/f/plaxis-soilvision-forum/209838/diaphragm-wall-movement-due-to-reduction-of-stiffness/636050#636050) regarding Plaxis in Bentley forum: A user asked the following question -paraphrased-:

> When we changed the plate parameters to simulate long term stiffness, there is no change in deformations. Is there something wrong?

I have tried my best to explain this in the forum, but it is not an isolated case. In fact, there are many examples of design reports that do not take this into account. For example, changing the plate parameters from shotcrete to final lining is not a correct approach to simulate long-term degradation of the temporary lining.

**Why are deformations not changed when we changed the stiffness of structural elements?**

There is a clear explanation for that: Because finite element method does not work that way. As can be seen below or in Appendix of Scientific Manual of Plaxis, the incremental deformations are caused by unbalanced load. If there is no unbalanced load, the deformations will not increase since the equation results in 0.

![1614286024541-1619901469706.png](../images/Plaxis/1614286024541-1619901469706.png)

But let's consider a tunnel. As you can see below, the real case is in first row. If we want the ground loads to act on the permanent lining, we can't just change the material properties and hope for the best. We have to **simulate the degradation of the temporary lining.** There are several methods for this such as gray rock or assuming a certain thickness of shotcrete thickness is degraded. However, some project requirements do not allow for the consideration of temporary lining for permanent lining analyses at all. This is the case for subway projects in Turkey.

![1614287907556.png](../images/Plaxis/1614287907556.png)

In the second row, you see the **wrong way** of using plates for tunnel design. If we use this method, structural forces will be less than actual. Why?

Let's remember few things and consider the above case of wrong use of plates:

- In soil-structure interaction, the stresses around the structures are affected by the stiffness of the structure and structural loads on these elements will be affected by the stresses and lining stiffness.
- Stresses to lining forces! Stresses are calculated based on the unbalanced forces. So if there is no unbalanced force, stresses will be the same and will be based on the black plate above.
- Lining forces? They depend on two things: Stresses and lining stiffness. Stresses are same but stiffness changed from black to red lining. So structural forces are changed. But is it correct? No. Because stresses used to calculate these forces are not true, they are based on black lining! They are based on previous soil-structure interaction analysis and not recalculated since there is no unbalanced force.

See the example below for assuming temporary lining is 25 cm and the permanent lining is 35 cm. Let me be clear: I do not recommend applying permanent lining directly, I have never used this method in design. What should we do is **TRANSFERRING THE GROUND LOADS FROM TEMPORARY LINING TO PERMANENT LINING.** To do that, the methodology should be revised to model both at the same time and degrading the temporary lining in long term. One can use a combination of volume elements and plate elements so that volume elements are deactivated in long term and only plates are left in place.

![1614288406973.png](../images/Plaxis/1614288406973.png)

**When can be changing plate properties a good idea?**

When we know that plate properties are changing in time and ground loads will be acting on different plate properties: Shotcrete vs. time analysis! You can use different properties for wet and dry shotcrete if you want to model the staged loading of temporary lining. For example, 50% of the soil loads will be relaxed until we reach the excavation, additional 20% will act on the wet shotcrete and remaining will act on the dry shotcrete.

**Do deformations change if we change the strength of soils?**

There is a really nice [explanation](https://communities.bentley.com/products/geotech-analysis/w/plaxis-soilvision-wiki/45962/reduction-of-stiffness-does-not-lead-to-a-change-in-displacements) by Plaxis for this. Assume that you have soil under a constant load. You change the initial c = 10 kPa and fi = 25 deg to c = 9 kPa and fi = 25 deg. Will there be any deformation?

**If** there is a plastic failure caused by an additional decrease of strength parameters, Plaxis will try to redistribute the forces in the soil volume until equilibrium is reached. Therefore, we will see additional displacements. However, if none of the soils reached the failure due to a decrease in strength, there will be no additional displacements even if we change the material properties significantly. We can see that in the stage of forming the reaction vector.

There is an option in Rocscience's RS2 for changing materials that allow users to select the soil volume's stress state. If we select the initial element loading as none, it will recalculate the stress when a certain material model is introduced to the model.

![1614286906765.png](../images/Plaxis/1614286906765.png)

**If we create a small unbalanced load to tweak the settings?**

No, it will not work. Notice that in the equations above, displacement increment is calculated, not the total displacements all over again. So your less stiff model (or stiffer) will not resist against all existing loads but only to unbalanced loads.