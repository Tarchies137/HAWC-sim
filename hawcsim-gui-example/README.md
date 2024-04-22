# About

This small example shows how to run the graphical interface of HAWCSim. 
It consists on a vertical muon shoot at the center PMT. 
Only one tank is simulated.
The config files are based on the config directory of the hawcsim project in 
AERIE.

# Before starting

## Install HAWCSim with GUI
You need to install HAWCSim. Although it is part of AERIE you need to have 
GEANT4 installed, otherwise it won't be build. 

When you install GEANT4 make sure you compile it with GUI capabilities. With
APE this is achived by adding the following to your `aperc` (uncommenting
lines https://gitlab.com/sgso-alliance/aerie-install/blob/master/install.sh#L68):

```
[package geant4]
cmakeOptions = -DGEANT4_USE_OPENGL_X11=ON -DGEANT4_USE_RAYTRACER_X11=ON
```

or for older APE versions:

```
[package geant4]
buildGui = True
```

## Checking OPENGL
Make sure you can execute `glxgears` and see the gears spinning. Otherwise you
have a problem with your OPENGL installations.

# Instructions

1. Clone and `cd` to this repository

2. Set the config directory

```
export HAWCSIM_CONFIG=./hawcsim_config
```

3. Run hawcsim:

```
hawcsim-exe --itype ascii --input muon.in --output muon.xcd --otype xcdf -R 100 -I
```

We are using a simple ascii text file to tell hawcsim how to shoot the 
particle. The format for `muon.in` is `particle_name x y z px py pz`, where the
coordinates are in cm and the momentum in GeV/c. You can have one particle per
line, although in this case we only have one and are reusing it 100 times (`-R`).
You can use `corsika` files as well. Finally, we need `-I` to open the interactive
terminal and use the GUI.

4. Run the visualization macro from the Geant4 terminal, which is displayed as `Idle>`, by typing

```
/control/execute runopengl
```

This runs a series of commands to change the viewpoint and drawing style, and 
finally shoots a particle.

5. Run more commands interactively

Check `runopengl` to have an idea of what you can do. There a more GEANT4 UI 
commands of course, you can google some tutorial. At the minimum:

  * Run `/run/beamOn 1` to shoot another particle
  * Use a combination of the `/vis/viewer` commands `set/viewpointThetaPhi`,
    `zoomTo` and `pan` to move around 

# Extras

## Choosing what to show

You can select/deselect what to show in `${HAWCSIM_CONFIG}/settings.dat`. For
example, if you don't want to see the baffles set `InBaffleVisible` to 0.

# Troubleshooting

## Geant doesn't compile in High Sierra
Geant4 doesn't compile with the clang version in High Sierra, which is more 
strict. You can download the path 4.10.00.p04 
(http://cern.ch/geant4-data/releases/geant4.10.00.p04.tar.gz), put it into the 
distfile folder in ape and add this to your aperc before installing

```
[package geant4]
version = 4.10.00.p04
buildGui = True
```

## COMMAND NOT FOUND </vis/open OGLSX 1200x1000>
Make sure `UseVisualization` is set to 1 in `${HAWCSIM_CONFIG}/settings.dat`.
While this is the case for this repo you might encounter some file in the wild
that are set to 0 for reason I ignored.
