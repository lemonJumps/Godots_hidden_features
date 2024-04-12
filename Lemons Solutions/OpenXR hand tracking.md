
While godot has a boilerplate class for handling hand tracking (OpenXRHand), it's very barebones and there's better ways to do this, and even use and combine animations :D

This is a feature that comes from XR_EXT_hand_tracking (part of OpenXR)
Info can be found here: [XR hand extension spec](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_EXT_hand_tracking)

So here's my approach, using OpenXRInterface class instead, which can get info like individual finger motion.

Now there's few assumptions that we have to make, and just hope that all headsets will support.
1. headset/controller supports hand tracking (duh)
2. the headset/controller will always behave in a same or similar way when pressing/releasing fingers
3. we can trust the user to calibrate fingers them selves if anything else fails

#### Implementation theory

`XrHandJointLocationEXT` implements most functionality, and that is split in godot into 4 methods.

`get_hand_joint_flags`
`get_hand_joint_rotation` < this one is actually the one we'll use
`get_hand_joint_position`
`get_hand_joint_radius`

So to do this we can take rotations and get a *open* and *closed* rotations, and then see where these angles are between all the angles.

Here we can decide wether we'll pass actual rotations onto joints, or use them to drive animations.

The problem is each headset can rotate each joint at different times so calculating a percentage for each joint and then adding it per finger is the only way to give us how much each finger moves for driving animations and or events.

**I can't stress this enough, don't rely on this as an only input method. it's not supported on every single headset**

#### Calculating angles

So this is very easy, you get the 100% angle and current to open and divide them :D
```
var closed : Quaternion()
var open : Quaternion()
var current : Quaternion()

var a := closed.angle_to(open)
var result := current.angle_to(open) / a
```

so just do this for each joint and add them together. and then you can drive animations with that.