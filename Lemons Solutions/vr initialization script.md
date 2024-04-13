A boiler plate script, that initializes openXR and makes it available to the rest of code

```
extends XROrigin3D

class_name OXR

static var xr_interface: OpenXRInterface

func _ready():
    xr_interface = XRServer.find_interface("OpenXR")
    if xr_interface and xr_interface.is_initialized():
        print("OpenXR initialized successfully")

        # Turn off v-sync!
        DisplayServer.window_set_vsync_mode(DisplayServer.VSYNC_DISABLED)

        # Change our main viewport to output to the HMD
        get_viewport().use_xr = true
    else:
        print("OpenXR not initialized, please check if your headset is connected")

```

Mind you this essentially makes the class a singleton, so don't make more than one instance.

The interface can be accessed via:
```
OXR.xr_interface
```