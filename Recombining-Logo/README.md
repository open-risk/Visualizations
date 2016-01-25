# Recombining Logo

This is a simple python script to create the effect of a magically recombining logo. You can apply the technique to any logo you can decompose into distinct svg elements that can be animated. 

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

<pre><code>
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
</code></pre>

