# cv2_gui

`cv2_gui` is a Python library for creating GUI elements using OpenCV.

# Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Button Types](#button-types)
    1. [Toggle Button](#toggle-button)
    2. [Cycle Button](#cycle-button)
    3. [Slider](#slider)
    4. [Eyedropper](#eyedropper)
    5. [D-Pad](#d-pad)



## Introduction

`cv2_gui` simplifies creating GUI elements, such as buttons, sliders, and more, with OpenCV.

## Installation

```bash
pip install cv2_gui
```
This will also install dependencies such as `cv2` and `numpy`.

## Button Types

| Button Type        | Description                          | Example Usage           |
|--------------------|--------------------------------------|-------------------------|
| [Toggle Button](#toggle-button)      | Switch between two states.          | `create_toggle_button`  |
| [Cycle Button](#cycle-button)       | Cycle through a list of options.    | `create_cycle_button`   |
| [Slider](#slider)             | Adjust a value within a range.      | `create_slider`         |
| [Eyedropper](#eyedropper)         | Select a color from an image.       | `create_eyedropper`     |
| [D-Pad](#d-pad)              | Control directional movements.      | `create_dpad`           |


### Toggle Button
Toggle button can switch between on and off state performing 1 of 2 actions based on current state.
#### Parameters for `create_toggle_button`

| Parameter Name   | Description                                                                 | Type                |
|------------------|-----------------------------------------------------------------------------|---------------------|
| `on_text`        | The text displayed on the button when it is in the "on" state.                | `str`               |
| `off_text`       | The text displayed on the button when it is in the "off" state.               | `str`               |
| `on_callback`    | The function that will run when the button is in the "on" state.              | `function`          |
| `off_callback`   | The function that will run when the button is in the "off" state.             | `function` or `None`|
| `toggle_once`    | If `True`, will run the callback function every update; otherwise, it runs the callback only when the state changes. | `bool`              |
| `on_color`       | The color of the button in the "on" state, represented as a list of RGB values. | `List[float, float, float]` |
| `off_color`      | The color of the button in the "off" state, represented as a list of RGB values. | `List[float, float, float]` |
| `tooltip`        | The text displayed when you hover over the button, providing additional information about the button's state. | `str`               |

#### example
a basic function that will be executed based on button state
```python
def display_image(img):
    return img[0]
```


import toggle button, button manager and dependecies from library.
```python
from cv2_gui import create_button_manager, create_toggle_button
import cv2
import numpy as np

toggle=create_toggle_button('ON','OFF',display_image,display_image,tooltip="A simple on-off button")

```
this will create a button which when pressed will display the passed image.
```python
sample = cv2.imread('sample.png')
while 1:
    output = toggle.update([sample],[None])
    output = create_button_manager.update(output)
```
first argument of `update` is `argument_on` which will be passed to `on_callabck` and second argument is `argument_off` passed to `off_callback` these are run when the corresponding state is active.

You can press `q` to close the window and stop execution,
Hovering over the button will display the tooltip.

<div align="center">
  <div style="display: inline-block; margin-right: 20px;">
    <img src="media/toggle_video.gif" width="800" />
    <p><em>Toggle Button Demo</em></p>
  </div>
</div>

### Cycle Button
Cycle button can switch between multiple states performing various actions based on current state.
#### Parameters for `create_cycle_button`
| Parameter   | Description                                                                 | Type                        |
|-------------|-----------------------------------------------------------------------------|-----------------------------|
| `modes`     | The text that will be displayed on the button when in a particular mode.     | List of Strings             |
| `callbacks` | The functions that will run when the button is in a selected mode.          | List of Functions           |
| `toggle_once` | If `True`, the callback function runs every update; if `False`, runs once per state change. | Boolean                    |
| `tooltip`   | Text displayed when you hover the mouse on the button, providing further information. | String                      |

#### example
a basic function that will be executed based on button state
```python
def convert_img(arguments):
    type,frame=arguments

    if type.lower()=="hsv":
        frame=cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)
    elif type.lower()=="rgb":
        frame=cv2.cvtColor(frame,cv2.COLOR_BGR2RGB)
    elif type.lower()=="bw":
        frame=cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
        frame=cv2.merge([frame, frame, frame])

    return frame
```


import cycle button, button manager and dependecies from library.
```python
from cv2_gui import create_button_manager, create_cycle_button
import cv2
import numpy as np

cycle=cycle=create_cycle_button(["BGR","RGB"],[convert_img,convert_img],tooltip="convert image color format")
```
this will create a button which when cycled will convert image formats of the passed image.
```python
sample = cv2.imread('sample.png')
while 1:
    output = cycle.update([["BGR",output],["RGB",output]])
    output = create_button_manager.update(output)
```
first argument of `update` is `arguments` the first element of argument will be passed to first callback function and so on.

You can press `q` to close the window and stop execution,
Hovering over the button will display the tooltip.

<div align="center">
  <div style="display: inline-block; margin-right: 20px;">
    <img src="media/cycle_video.gif" width="800" />
    <p><em>Cycle Button Demo</em></p>
  </div>
</div>

  