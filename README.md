# a matrix lava lamp

A web interface for generating fluid wave animations for RGB Matrix displays. 

## Overview 

This project provides an intuitive web interface to generate and customize wave patterns for RGB Matrix displays (32x64). Users can select from various themes or create cycling animations, then either export the code or run it on Hack Club's Neon editor.

## Technical Stack 

### Frontend
- Pure HTML/CSS/JavaScript (ALL ON ONE FILE!!!!)
- No external dependencies or frameworks
- CSS Features used:
  - CSS Grid for layout
  - CSS Animations for fluid effects
  - Glassmorphic UI elements
  - Backdrop filters
  - Custom keyframe animations

### Fonts
- Inter Mono (via Google Fonts)

### Animation Engine
The Python code generates fluid animations using:
- Multiple overlapping sine waves
- Dynamic color interpolation
- Time-based pattern generation
- Automatic day/night intensity adjustment

### Matrix Display
Compatible with:
- 64x32 RGB LED Matrix
- CircuitPython
- Hack Club Neon's hardware configuration

## Features 

- 6 preset themes:
  - Waves (Ocean blues)
  - Elixir (Purple mystical)
  - Campfire (Warm oranges)
  - Storm (Moody grays)
  - Snow (Icy whites/cyans)
  - Lava (Hot reds/oranges)
- Theme cycling option
- Live preview animations
- One-click code generation
- Direct Neon editor integration
- Responsive design
- Glassmorphic UI
- Matrix Preview:
  - Accurate 64x32 LED simulation
  - Real-time wave animation
  - Pixel-perfect representation
  - Individual LED visualization
  - True-to-life color rendering

## Usage 

1. Visit the web interface
2. Select a theme or choose "Cycle All Themes"
3. Preview how it will look on your actual matrix
4. Click "Run it yourself"
5. Paste the generated code in the Neon editor
6. Watch your matrix come alive!

## Hardware Requirements 

- Hack Club Neon RGB Matrix (64x32)
- Compatible CircuitPython board
- Proper power supply
- Matrix properly connected to:
  - RGB pins: D6, D5, D9, D11, D10, D12
  - Address pins: A5, A4, A3, A2
  - Clock pin: D13
  - Latch pin: D0
  - Output Enable pin: D1

## The Math Behind the Waves 

The fluid animations are created by combining multiple wave functions. Here's a breakdown:

### Base Wave Function
```python
def wave(x, y, frame):
    t = frame * 0.05  # Time factor for animation speed
    
    # Primary wave: Creates main diagonal flow
    wave1 = math.sin(x * 0.1 + t) * math.cos(y * 0.1 - t * 0.7)
    
    # Secondary wave: Adds circular motion
    wave2 = math.sin((x + y) * 0.1 + t * 0.8) * math.cos((x - y) * 0.05 + t * 0.3)
    
    # Tertiary wave: Creates organic variation
    wave3 = math.cos(x * 0.15 - y * 0.1 + t * 0.3) * math.sin(y * 0.2 + t * 0.2)
    
    # Radial wave: Adds expanding/contracting motion
    wave4 = math.sin(math.sqrt((x*x + y*y)) * 0.1 + t * 0.5)
    
    # Modulation wave: Adds subtle complexity
    wave5 = math.cos(x * 0.05 + y * 0.05 + math.sin(t * 0.2) * 2)
    
    # Combine all waves and normalize to 0-1 range
    combined = (wave1 + wave2 + wave3 + wave4 + wave5) / 5
    return combined * 0.5 + 0.5
```

### Key Components:

1. **Time Evolution**: `t = frame * 0.05`
   - Slows down the animation to a pleasing speed
   - Creates continuous motion through the pattern

2. **Wave Interactions**:
   - `wave1`: Primary diagonal flow pattern
   - `wave2`: Circular motion using x+y and x-y terms
   - `wave3`: Organic variation with phase shifts
   - `wave4`: Radial patterns using distance from center
   - `wave5`: Subtle modulation using sine of time

3. **Color Mapping**:
```python
if wave_val < 0.25:
    pixels.append(0)    # Darkest color
elif wave_val < 0.5:
    pixels.append(1)    # Medium-dark color
elif wave_val < 0.75:
    pixels.append(2)    # Medium-bright color
else:
    pixels.append(3)    # Brightest color
```

4. **Theme Transitions**:
```python
def interpolate_color(color1, color2, factor):
    r1, g1, b1 = (color1 >> 16) & 0xFF, (color1 >> 8) & 0xFF, color1 & 0xFF
    r2, g2, b2 = (color2 >> 16) & 0xFF, (color2 >> 8) & 0xFF, color2 & 0xFF
    r = int(r1 + (r2 - r1) * factor)
    g = int(g1 + (g2 - g1) * factor)
    b = int(b1 + (b2 - b1) * factor)
    return (r << 16) | (g << 8) | b
```

The combination of these mathematical components creates fluid, organic-looking animations that never exactly repeat, similar to the patterns seen in actual lava lamps.

## Preview Feature

The website includes a real-time matrix preview that shows exactly how the animation will appear on your LED matrix:

- Simulates the actual 64x32 LED grid
- Shows individual LED pixels with proper spacing
- Renders accurate colors and brightness levels
- Updates in real-time as waves animate
- Supports all themes including cycle mode
- Matches the physical matrix display layout

This preview helps you visualize how your chosen theme will look before uploading to your matrix display.

## Credits

Created by [ashfelloff](https://github.com/ashfelloff)  
Hardware platform by [Hack Club Neon](https://neon.hackclub.dev) 
