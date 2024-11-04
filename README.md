# jgilwood.github.io
![IMG_8609](https://github.com/user-attachments/assets/45dabf72-2b68-4587-a844-9086a14f2b9b)

```python
# prompt: generate code that would create perlin noise with PIL

from PIL import Image, ImageDraw
import random
import math
import numpy as np

def generate_perlin_noise_2d(width, height, scale):
  """Generates 2D Perlin noise."""
  noise = [[0.0] * height for _ in range(width)]
  for x in range(width):
    for y in range(height):
      nx = x / width * scale
      ny = y / height * scale
      noise[x][y] = perlin_noise_2d(nx, ny)
  return noise

def perlin_noise_2d(x, y):
  """Calculates Perlin noise at a given coordinate."""
  X = int(x)
  Y = int(y)

  xf = x - X
  yf = y - Y

  u = fade(xf)
  v = fade(yf)

  n00 = gradient_noise_2d(X, Y, x, y)
  n01 = gradient_noise_2d(X, Y + 1, x, y)
  n10 = gradient_noise_2d(X + 1, Y, x, y)
  n11 = gradient_noise_2d(X + 1, Y + 1, x, y)

  x1 = lerp(n00, n10, u)
  x2 = lerp(n01, n11, u)

  return lerp(x1, x2, v)

def fade(t):
  """Interpolation function."""
  return t * t * t * (t * (t * 6 - 15) + 10)

def lerp(a, b, t):
  """Linear interpolation."""
  return a + t * (b - a)

def gradient_noise_2d(x, y, px, py):
  """Calculates gradient noise at a given coordinate."""
  vectors = [
      (1, 0), (-1, 0), (0, 1), (0, -1), (1, 1), (-1, 1), (1, -1), (-1, -1)
  ]
  random.seed(x * 57 + y * 113)
  gradient = random.choice(vectors)

  dx = px - x
  dy = py - y

  return dx * gradient[0] + dy * gradient[1]
```
