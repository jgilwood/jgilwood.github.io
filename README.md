# 
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

``` python
# a little weirded out by gemini going beyond what i asked and already moving into making the image "Rothko-esque"
#consulting gemini to get a better understanding of how the code works, specifically for the colors
#will likely have to make three separate functions to get the three colors and best replicate the og piece

#moving this down here so i can mess with using a lambda instead of a def <-- he said, proceeding to not do that
```

``` python
def create_bg(width, height, num_layers):
  """Creates an image with a Rothko-esque style using Perlin noise."""
  img = Image.new("RGB", (width, height), "black")
  draw = ImageDraw.Draw(img)

  for _ in range(num_layers):
    noise = generate_perlin_noise_2d(width, height, random.uniform(1, 5))
    
    for x in range(width):
      for y in range(height):
        noise_value = noise[x][y]
        
        # Remap noise value to a color range (e.g., red-maroon)
        red = int(255 * (0.4 + noise_value * 0.5)) - 75
        green = int(255 * (0.2 + noise_value * 0.2)) - 75
        blue = int(255 * (0.5 + noise_value * 0.1)) - 75

        draw.point((x, y), fill=(red, green, blue))

  return img

def create_top(width, height, num_layers):
  img= Image.new("RGB", (width, height), "black")
  draw = ImageDraw.Draw(img)

  for _ in range(num_layers):
    noise = generate_perlin_noise_2d(width, height, random.uniform(1, 5))
    
    for x in range(width):
      for y in range(height):
        noise_value = noise[x][y]
        
        
        red = int(255 * (0.5 + noise_value * 0.5)) - 75
        green = int(255 * (0.2 + noise_value * 0.2)) - 75
        blue = int(255 * (0.1 + noise_value * 0.1)) - 75

        draw.point((x, y), fill=(red, green, blue))


background = create_bg(500, 650, 1)
background

top = create_top((25, 475), (50, 375), 1)
```

``` python
def color_map_1(noise_value): #background purple
          red = int(255 * (0.4 + noise_value * 0.4)) - 75
          green = int(255 * (0.2 + noise_value * 0.2)) - 75
          blue = int(255 * (0.4 + noise_value * 0.5)) - 75
          return (red, green, blue)

def color_map_2(noise_value): #top red
          red = int(255 * (0.5 + noise_value * 0.5)) 
          green = int(255 * (0.2 + noise_value * 0.2)) 
          blue = int(255 * (0.1 + noise_value * 0.1)) 
          return (red, green, blue)

def color_map_3(noise_value): #bottom blue

          red = int(255 * (0.1 + noise_value * 0.1)) 
          green = int(255 * (0.2 + noise_value * 0.2)) 
          blue = int(255 * (0.5 + noise_value * 0.5)) 
          return (red, green, blue)

def create_bg(width, height, num_layers):
  img = Image.new("RGB", (width, height), "purple")
  draw = ImageDraw.Draw(img)

  for _ in range(num_layers):
    noise = generate_perlin_noise_2d(width, height, random.uniform(1, 5))
    
    for x in range(width):
      for y in range(height):
        noise_value = noise[x][y]
        scaled_noise = (noise_value +1)/2
      
       # color = color_map_1(scaled_noise)

        if x > 25 and x < 475 and y > 50 and y < 375:
          color = color_map_2(scaled_noise)
        elif x > 25 and x < 475 and y > 400 and y < 625:
            color = color_map_3(scaled_noise)
        else:
           color = color_map_1(scaled_noise)

        draw.point((x, y), fill=color)

    return img



#def create_top(width, height, num_layers):
#  img= Image.new("RGB", (width, height), "black")
#  draw = ImageDraw.Draw(img)

#  for _ in range(num_layers):
#    noise = generate_perlin_noise_2d(width, height, random.uniform(1, 5))
    
#    for x in range(width):
#      for y in range(height):
#        noise_value = noise[x][y]
        
        
#        red = int(255 * (0.5 + noise_value * 0.5)) - 75
#       green = int(255 * (0.2 + noise_value * 0.2)) - 75
#        blue = int(255 * (0.1 + noise_value * 0.1)) - 75

#        draw.point((x, y), fill=(red, green, blue))


background = create_bg(500, 650, 1)
background

#top = create_top((25, 475), (50, 375), 1)
```
