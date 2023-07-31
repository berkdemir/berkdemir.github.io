---
layout: default
title: Seismic Deformation
nav_order: 04
parent: Plaxis
permalink: /Plaxis/SeismicDeformation
---

# Plaxis-Python | Seismic Deformation

For underground structures, a rough but reasonable simplification is pseudo-static deformation method. In this method, we apply seismic strain which can be calculated as the ratio of effective PGV (Peak Ground Velocity) to effective shear wave velocity.

$$
\gamma = \frac{PGV_e}{VS_e} 
$$

Effective PGV can be multiplied with depth dependent reduction factors (see FHWA-NHI-10-034) and maximum shear wave velocity obtained from geophysical tests with almost zero strain can be converted to effective shear wave velocity based on recommendations of FHWA or Eurocode 8.

A simple Python code can be written to implement lateral deformation profile in Plaxis to simulate seismic loading. If you locate this Python file inside the Bentley folder (< PLAXIS installation folder >\pytools\input) this can be directly called from Plaxis Input.

```python
"""
Seismic Deformations for Plaxis by Berk Demir / https://github.com/berkdemir
Locate this file in <PLAXIS installation folder>\pytools\input and call it from Plaxis - Expert - Run Python tool
"""

import easygui
from plxscripting.easy import *
# If script is used OUTSIDE of Plaxis.
"""
localhostport_i = 10000 # Local host port id can be different.
password = "password" # Your password should be here.
s_i, g_i = new_server("localhost", localhostport_i, password=password)
"""
# If we use the script as a Plaxis tool.

s_i, g_i = new_server()

def get_borehole_layers(borehole):
    """ reads the borehole information to collect soillayer thickness
        information and returns a dictionary per layer top-down """
    borehole_layers = []
    for soillayer in g_i.Soillayers:
        for zone in soillayer.Zones:
            if (zone.Borehole.value) == borehole:
                borehole_layers.append({"name": soillayer.Name.value,
                                        "top": zone.Top.value,
                                        "bottom": zone.Bottom.value,
                                        "thickness": zone.Thickness.value
                                        }
                                       )
    return borehole_layers

def get_xmin_xmax():
    """ gets the xmax and xmin of the model."""
    point_list = []
    
    for i in g_i.SoilContour:
        point_list.append(i)
    
    xmin = point_list[0].x.value
    xmax = point_list[1].x.value
    return xmin, xmax

bh = g_i.Boreholes[0]

borehole_layers = get_borehole_layers(bh)

top = borehole_layers[0]["top"]
bottom = borehole_layers[-1]["bottom"]
depth = top-bottom

title = "Seismic Deformation Application by Berk Demir"
msg = "Please enter seismic parameters."
fieldNames = [
    "Peak Ground Velocity (cm/sec)",
    "Reduction in PGV due to depth",
    "Maximum Shear Wave Velocity (m/s)",
    "Ratio of Effective to Maximum Shear Wave Velocity"
    ]

fieldValues = easygui.multenterbox(msg, title, fieldNames)

def_choice = ["Triangular","Z-Shape"]

select_def_type = easygui.buttonbox("Select deformation type", "Seismic Deformation Type", def_choice)

PGV, PGV_Red, VS, VS_Red = [float(item) for item in fieldValues]

xmin, xmax = get_xmin_xmax()

strain = PGV * PGV_Red * 0.01 / (VS*VS_Red)

if select_def_type == "Triangular":
    deformation = strain * depth
    LD_Left = g_i.linedispl((xmin,top),(xmin,bottom))[-1]
    LD_Right = g_i.linedispl((xmax,top),(xmax,bottom))[-1]
    LD_Top = g_i.linedispl((xmin,top),(xmax,top))[-1]
    LD_Left.setproperties("Displacement_x","Prescribed","Displacement_y","Free","Distribution","Linear","ux_start",deformation,"ux_end",0)
    LD_Right.setproperties("Displacement_x","Prescribed","Displacement_y","Free","Distribution","Linear","ux_start",deformation,"ux_end",0)
    LD_Top.setproperties("Displacement_x","Prescribed","Displacement_y","Fixed","Distribution","Uniform","ux_start",deformation)
    
elif select_def_type == "Z-Shape":
    deformation = strain * depth * 0.5
    LD_Left = g_i.linedispl((xmin,top),(xmin,bottom))[-1]
    LD_Right = g_i.linedispl((xmax,top),(xmax,bottom))[-1]
    LD_Top = g_i.linedispl((xmin,top),(xmax,top))[-1]
    LD_Bottom = g_i.linedispl((xmin,bottom),(xmax,bottom))[-1]
    LD_Left.setproperties("Displacement_x","Prescribed","Displacement_y","Free","Distribution","Linear","ux_start",deformation,"ux_end",-deformation)
    LD_Right.setproperties("Displacement_x","Prescribed","Displacement_y","Free","Distribution","Linear","ux_start",deformation,"ux_end",-deformation)
    LD_Top.setproperties("Displacement_x","Prescribed","Displacement_y","Fixed","Distribution","Uniform","ux_start",deformation)
    LD_Bottom.setproperties("Displacement_x","Prescribed","Displacement_y","Fixed","Distribution","Uniform","ux_start",-deformation)
```

To determine the effective shear wave velocity, I have developed a simple methodology and will present this as a part of a paper in the near future (hopefully in WTC 2022). I will not go into detail too much, but you can understand from the code that it is a iterative process.

- Calculate seismic shear strain using reduced PGV based on the depth of tunnel and maximum shear wave velocity.
- Ignoring any static strain, calculate the G/Gmax based on either one of the presented approaches.
- Calculate shear modulus reduction ratio as square root of the G/Gmax.
- Using the calculated ratio, calculate effective shear wave velocity.
- Calculate new seismic shear strain using reduced PGV and new effective shear wave velocity.
- Continue until reasonable difference is obtained between Vsi+1 and Vsi.

Here is the simple code that calculates the effective shear wave velocity:

```python
def EFS(Ground_Type, PGV, PGV_Red, VS, PI=20, OCR=1, Eff_Pressure=300):
    """
    Effective Shear Wave Velocity Using Darendeli (2001) and Schnabel (1973) by Berk Demir / https://github.com/berkdemir
    
    Inputs:
        Ground_Type: Either "Soil" or "Rock"
        PGV: Peak Ground Velocity (cm/sec) 
        PGV_Red: PGV reduction ratio with depth
        VS: Maximum shear wave velocity (m/s)
        PI: Plasticity index in percent. (For Soil only.)
        OCR: Overconsolidation Ratio. (For Soil only.)
        Eff_Pressure: Effective pressure (kPa) (For Soil only)
        
    Returns:
        VS_Red: Reduction ratio for maximum shear wave velocity.
    
    Notes:
        Also prints a sentence with reduction ratio and effective shear wave velocity.
        
    Example:
        For Soils: Vs_Red = EFS(Ground_Type = "Soil", PGV = 70, PGV_Red = 0.8, VS = 300, PI=20, OCR=1, Eff_Pressure=300)
        For Rocks: Vs_Red = EFS(Ground_Type = "Rock", PGV = 70, PGV_Red = 0.8, VS = 800)
    """
    
    PGV_eff = PGV * PGV_Red * 0.01  # m/s
    Shear_Modulus_Reaction = 0.7  # initial value
    VS_Red = pow(Shear_Modulus_Reaction, 0.5)

    if Ground_Type == "Soil":
        Strain_Ref = (0.0352 + 0.001 * PI * pow(OCR, 0.3246)) * \
            pow(Eff_Pressure / 100, 0.3483) / 100
        VS_Red = pow(Shear_Modulus_Reaction, 0.5)
        for i in range(0, 20):
            seismic_shear_strain = PGV_eff / (VS_Red * VS)
            Shear_Modulus_Reaction = 1 / \
                (1 + pow(seismic_shear_strain / Strain_Ref, 0.919))
            VS_Red = pow(Shear_Modulus_Reaction, 0.5)
        print("Reduction ratio of shear wave velocity is", round(VS_Red, 2), "and effective shear wave velocity is", round(
            VS*VS_Red, 0), "m/s using Darandeli (2001) G/Gmax curves for soils.")

    elif Ground_Type == "Rock":
        for i in range(0, 20):
            seismic_shear_strain = PGV_eff / (VS_Red * VS)
            Shear_Modulus_Reaction = -3.642 * \
                pow(seismic_shear_strain, 0.02553) + 3.784
            VS_Red = pow(Shear_Modulus_Reaction, 0.5)
        print("Reduction ratio of shear wave velocity is", round(VS_Red, 2), "and effective shear wave velocity is", round(
            VS*VS_Red, 0), "m/s using Schnabel (1973) G/Gmax curve for rocks.")

    return VS_Red

if __name__ == "__main__":
    VS_Red = EFS(Ground_Type, PGV, PGV_Red, VS, PI, OCR, Eff_Pressure)
    VS_eff = VS * VS_Red
```