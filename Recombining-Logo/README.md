# Recombining Logo

This is a simple python script to create the effect of a magically recombining logo. You can apply the technique to any logo you can decompose into distinct svg elements that can be animated. 

### Libraries Used


#### This is the main library for generatic programmatic svg frames
import svgwrite
#### The following libraries help us with the computations
import random
import math
import numpy as np
#### We use subprocess to do some file conversions on the fly (you can also do this separately)
import subprocess
