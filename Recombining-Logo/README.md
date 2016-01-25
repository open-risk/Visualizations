# Recombining Logo

This is a simple python script to create the effect of a "magically" recombining logo. You can apply the technique to any logo you can decompose into distinct svg elements that can be animated. The script produces a recombining animation of the OpenRisk [logo](https://www.youtube.com/watch?v=7s1QGWZ3dR8).

### Concept

The idea is as follows: 
* start with a logo composed of any set of svg elements
* animate those elements separately with random trajectories
* create svg frames for each new time point
* put those frames back together in reverse chronological order

### Libraries used

```python
# This is the main library for generatic programmatic svg frames
import svgwrite
# The following libraries help us with the computations
import random
import math
import numpy as np
# We use subprocess to do some file conversions on the fly (you can also do this separately)
import subprocess
```

### Store data in numpy arrays

Using numpy helps with fast computations (for more complicated trajectories)

```python
# This is the total number of svg logo elements (for storage)
n = 28

# The following numpy arrays store positions and velocities of the svg elements
# x coordinates
xo = np.ones(n)
# y coordinates
yo = np.ones(n)
# x velocity
vx = np.zeros(n)
# y velocity
vy = np.zeros(n)
# radious
r = np.ones(n)
# element color
c = []
```

### Programmatically generate the svg elements (shapes, positions, colors)

This can be more or less easy depending on the logo. Our OpenRisk logo is simply a set of circular elements :-)

```python
# Set initial locations, radii and colors of all circles

# Set the radii of inner circles
r[3] = 35
r[2] = 66
r[1] = 95
r[0] = 120

# Set the color scheme
c.append('#221616')
c.append('#F1D039')
c.append('#491311')
c.append('#B46B36')

for i in range(0, 4):
    xo[i] = 100
    yo[i] = 100

radius = 32.5
for i in range(4, 14):
    xo[i] = 100 + radius * math.cos((i - 4) * 2. * math.pi / 10)
    yo[i] = 100 + radius * math.sin((i - 4) * 2. * math.pi / 10)
    r[i] = 15
    c.append('#708033')

radius = 57.5
for i in range(14, 21):
    xo[i] = 100 + radius * math.cos((i - 14) * 2. * math.pi / 7)
    yo[i] = 100 + radius * math.sin((i - 14) * 2. * math.pi / 7)
    r[i] = 25
    c.append('#E85129')

radius = 60
phase = 2. * math.pi / 7 / 2
for i in range(21, 28):
    xo[i] = 100 + radius * math.cos(phase + (i - 21) * 2. * math.pi / 7)
    yo[i] = 100 + radius * math.sin(phase + (i - 21) * 2. * math.pi / 7)
    r[i] = 12.5
    c.append('#BA3B1D')
```

### Set the dynamic properties of the elements (velocities)

This is where the logo lifts off, so to speak. You select the initial velocities for all elements

```python
# Create random velocities
# Use inverse correlation between size and velocity to create illusion of inertia
dt = 0.004
rmax = max(r)
for i in range(0, 28):
    vx[i] = (1.2 * rmax - r[i]) * random.uniform(-10, 10)
    vy[i] = (1.2 * rmax - r[i]) * random.uniform(-10, 10)
    r[i] = r[i] / 2
```

### Create a bounding box

Depending on the physics you want to apply to the motion of the elements, you may have to contain them via a bounding box (where they bounce)

```python
# Set the bounding box dimensions
xbox = 600
ybox = 200
```
