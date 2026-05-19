---
title: "Hoek-Brown Parameters Database"
date: 2021-03-12T00:00:00+00:00
draft: false
tags: ["Geotechnics"]
---

If you will use Hoek-Brown in your Python code, you may want to recommend some constants based on rock type. There is a widely used table in literature by Hoek and others that we use to select Modulus Ratio and material constant (mi) in the absence of high quality laboratory tests.
I have done the manual labour, don't write it all again. A dictionary called `RockDict` is given in the following Gist. Rock types are given as keys of dict and a sub-dictionary with:
- MR: Modulus Ratio
- MRSTD: Modulus Ratio Deviation
- Type: Rock Type (Igneous, Metamorphic, Sedimentary)
- mi: Material Constant
- miSDT: Material Constant Deviation

```python
"""
Hoek-Brown parameters in literature by Berk Demir
This is a simple dictionary called "RockDict" for different rock types and their mi and MR values. Deviations (± from the average) are also stored as miSTD and MRSTD.
Keys: Rock Types.
Sub-dictionary keys:
   * "MR": Modulus Ratio - int
   * "MRSTD": Deviation of Modulus Ratio - int
   * "Type": Rock type - str
   * "mi": Hoek-Brown constant
   * "mi": Deviation of mi.
   
Prepared using Plaxis Reference Manual.
"""

RockDict = {
            "Agglomerate": {
                "MR": 500,
                "MRSTD": 100,
                "Type": "Igneous",
                "mi": 19,
                "miSTD": 3,
            },
            "Amphibolites": {
                "MR": 450,
                "MRSTD": 50,
                "Type": "Metamorphic",
                "mi": 26,
                "miSTD": 6,
            },
            "Andesite": {
                "MR": 400,
                "MRSTD": 100,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 5,
            },
            "Anhydrite": {
                "MR": 350,
                "MRSTD": 0,
                "Type": "Sedimentary",
                "mi": 12,
                "miSTD": 2,
            },
            "Basalt": {
                "MR": 350,
                "MRSTD": 100,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 5,
            },
            "Breccia-I": {
                "MR": 500,
                "MRSTD": 0,
                "Type": "Igneous",
                "mi": 19,
                "miSTD": 5,
            },
            "Breccia-S": {
                "MR": 290,
                "MRSTD": 60,
                "Type": "Sedimentary",
                "mi": 19,
                "miSTD": 5,
            },
            "Chalk": {
                "MR": 1000,
                "MRSTD": 0,
                "Type": "Sedimentary",
                "mi": 7,
                "miSTD": 2,
            },
            "Claystones": {
                "MR": 250,
                "MRSTD": 50,
                "Type": "Sedimentary",
                "mi": 4,
                "miSTD": 2,
            },
            "Conglomerates": {
                "MR": 350,
                "MRSTD": 50,
                "Type": "Sedimentary",
                "mi": 21,
                "miSTD": 3,
            },
            "Dacite": {
                "MR": 400,
                "MRSTD": 50,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 3,
            },
            "Diabase": {
                "MR": 325,
                "MRSTD": 25,
                "Type": "Igneous",
                "mi": 15,
                "miSTD": 5,
            },
            "Diorite": {
                "MR": 325,
                "MRSTD": 25,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 5,
            },
            "Dolerite": {
                "MR": 350,
                "MRSTD": 50,
                "Type": "Igneous",
                "mi": 12,
                "miSTD": 3,
            },
            "Dolomites": {
                "MR": 425,
                "MRSTD": 75,
                "Type": "Sedimentary",
                "mi": 9,
                "miSTD": 3,
            },
            "Gabbro": {
                "MR": 450,
                "MRSTD": 50,
                "Type": "Sedimentary",
                "mi": 27,
                "miSTD": 3,
            },
            "Gneiss": {
                "MR": 525,
                "MRSTD": 225,
                "Type": "Metamorphic",
                "mi": 28,
                "miSTD": 5,
            },
            "Granite": {
                "MR": 425,
                "MRSTD": 125,
                "Type": "Igneous",
                "mi": 32,
                "miSTD": 3,
            },
            "Granodiorite": {
                "MR": 425,
                "MRSTD": 125,
                "Type": "Igneous",
                "mi": 29,
                "miSTD": 3,
            },
            "Greywackes": {
                "MR": 350,
                "MRSTD": 0,
                "Type": "Sedimentary",
                "mi": 18,
                "miSTD": 3,
            },
            "Gypsum": {
                "MR": 350,
                "MRSTD": 0,
                "Type": "Sedimentary",
                "mi": 8,
                "miSTD": 2,
            },
            "Hornfels": {
                "MR": 550,
                "MRSTD": 150,
                "Type": "Metamorphic",
                "mi": 19,
                "miSTD": 4,
            },
            "Limestone (Crystalline)": {
                "MR": 500,
                "MRSTD": 100,
                "Type": "Sedimentary",
                "mi": 12,
                "miSTD": 3,
            },
            "Limestone (Micritic)": {
                "MR": 900,
                "MRSTD": 100,
                "Type": "Sedimentary",
                "mi": 9,
                "miSTD": 2,
            },
            "Limestone (Sparitic)": {
                "MR": 700,
                "MRSTD": 100,
                "Type": "Sedimentary",
                "mi": 10,
                "miSTD": 2,
            },
            "Marble": {
                "MR": 850,
                "MRSTD": 150,
                "Type": "Metamorphic",
                "mi": 9,
                "miSTD": 3,
            },
            "Marls": {
                "MR": 175,
                "MRSTD": 25,
                "Type": "Sedimentary",
                "mi": 7,
                "miSTD": 2,
            },
            "Metasandstones": {
                "MR": 250,
                "MRSTD": 50,
                "Type": "Metamorphic",
                "mi": 19,
                "miSTD": 3,
            },
            "Migmatite": {
                "MR": 375,
                "MRSTD": 25,
                "Type": "Metamorphic",
                "mi": 29,
                "miSTD": 3,
            },
            "Norite": {
                "MR": 375,
                "MRSTD": 25,
                "Type": "Igneous",
                "mi": 20,
                "miSTD": 5,
            },
            "Peridotite": {
                "MR": 275,
                "MRSTD": 25,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 5,
            },
            "Phyllites": {
                "MR": 550,
                "MRSTD": 250,
                "Type": "Metamorphic",
                "mi": 7,
                "miSTD": 3,
            },
            "Porphyries": {
                "MR": 400,
                "MRSTD": 0,
                "Type": "Igneous",
                "mi": 20,
                "miSTD": 5,
            },
            "Quartzites": {
                "MR": 375,
                "MRSTD": 75,
                "Type": "Metamorphic",
                "mi": 20,
                "miSTD": 3,
            },
            "Rhyolite": {
                "MR": 400,
                "MRSTD": 100,
                "Type": "Igneous",
                "mi": 25,
                "miSTD": 5,
            },
            "Sandstones": {
                "MR": 275,
                "MRSTD": 75,
                "Type": "Sedimentary",
                "mi": 17,
                "miSTD": 4,
            },
            "Schists": {
                "MR": 675,
                "MRSTD": 425,
                "Type": "Metamorphic",
                "mi": 12,
                "miSTD": 3,
            },
            "Shales": {
                "MR": 200,
                "MRSTD": 50,
                "Type": "Sedimentary",
                "mi": 6,
                "miSTD": 2,
            },
            "Siltstones": {
                "MR": 375,
                "MRSTD": 25,
                "Type": "Sedimentary",
                "mi": 7,
                "miSTD": 2,
            },
            "Slates": {
                "MR": 500,
                "MRSTD": 100,
                "Type": "Metamorphic",
                "mi": 7,
                "miSTD": 4,
            },
            "Tuff": {"MR": 300, "MRSTD": 100, "Type": "Igneous", "mi": 13, "miSTD": 5,},
        }
```
