So! Physical bone 3d can have constraints, BUT they're currently undocumented.

Constraint Types:
- JOINT_TYPE_NONE - obviously none lol
- JOINT_TYPE_PIN
- JOINT_TYPE_CONE
- JOINT_TYPE_HINGE
- JOINT_TYPE_SLIDER
- JOINT_TYPE_6DOF

Each of these has their own properties, which you can find bellow.
Now the thing about these is that they remain in the engine from godot 2/early godot 3 era, so they aren't accessible normally because of the naming from that era.

```gdscript
# if in code, bone is created as:
var bone := PhysicalBone3D.new()
root.add_child(bone)    # add to scene


# all joint properties can be accessed using set and get
bone.set("joint_constraints/bias") = bias
bias = bone.get("joint_constraints/bias")

bone.joint_type # check against this to see which joint is used
bode.get_property_list() # gets you all current properties
# output of this will change depending on joint_type!!!!
```
#### PinJoint (JOINT_TYPE_PIN)

##### "joint_constraints/bias"
Type: *float*
Default value: *0.3*
Min, Max, Step: *0.01, 0.99, 0.01*
##### "joint_constraints/damping"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.01, 8.0, 0.01*
##### "joint_constraints/impulse_clamp"
Type: *float*
Default value: *0.0*
Min, Max, Step: *0.0, 64.0, 0.01*

#### ConeJoint (JOINT_TYPE_CONE)

##### "joint_constraints/swing_span"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *PI \* 0.25*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/twist_span"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *PI*
Min, Max, Step: *-40000, 40000, 0.01* (or greater or less?)
##### "joint_constraints/bias"
Type: *float*
Default value: *0.3*
Min, Max, Step: *0.1, 16.0, 0.01*
##### "joint_constraints/softness"
Type: *float*
Default value: *0.8*
Min, Max, Step: *0.1, 16.0, 0.01*
##### "joint_constraints/relaxation"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.1, 16.0, 0.01*

#### HingeJoint (JOINT_TYPE_HINGE)

##### "joint_constraints/angular_limit_enabled"
Type: *bool*
Default value: *false*
##### "joint_constraints/angular_limit_upper"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *PI \* 0.5*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_lower"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *PI \* 0.5*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_bias"
Type: *float*
Default value: *0.3*
Min, Max, Step: *0.01, 0.99, 0.01*
##### "joint_constraints/angular_limit_softness"
Type: *float*
Default value: *0.9*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/angular_limit_relaxation"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.01, 16.0, 0.01*

#### SliderJointData (JOINT_TYPE_SLIDER)

##### "joint_constraints/linear_limit_upper"
Type: *float* (probably world units)
Default value: *1.0*
##### "joint_constraints/linear_limit_lower"
Type: *float* (probably world units)
Default value: *-1.0*
##### "joint_constraints/linear_limit_softness"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/linear_limit_restitution"
Type: *float*
Default value: *0.7*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/linear_limit_damping"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.0, 16.0, 0.01*

##### "joint_constraints/angular_limit_upper"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *0.0*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_lower"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *0.0*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_softness"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/angular_limit_restitution"
Type: *float*
Default value: *0.7*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/angular_limit_damping"
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.0, 16.0, 0.01*

#### SixDOFJoint (JOINT_TYPE_6DOF)

##### "joint_constraints/linear_limit_enabled"
Type: *bool*
Default value: *false*
##### "joint_constraints/linear_limit_upper"
Type: *float* (probably world units)
Default value: *0.0*
##### "joint_constraints/linear_limit_lower"
Type: *float* (probably world units)
Default value: *-0.0*
##### "joint_constraints/linear_limit_softness"
Type: *float*
Default value: *0.7*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/linear_restitution"
***warning! - this name differs from slider joint***
Type: *float*
Default value: *0.5*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/linear_damping"
***warning! - this name differs from slider joint***
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.0, 16.0, 0.01*


##### "joint_constraints/linear_spring_enabled"
Type: *bool*
Default value: *false*
##### "joint_constraints/linear_spring_stiffness"
Type: *float* (probably world units)
Default value: *0.0*
##### "joint_constraints/linear_spring_damping"
Type: *float* (probably world units)
Default value: *0.0*
##### "joint_constraints/linear_equilibrium_point"
Type: *float* (probably world units)
Default value: *0.0*

##### "joint_constraints/angular_limit_enabled"
Type: *bool*
Default value: *true*
##### "joint_constraints/angular_limit_upper"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *0.0*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_lower"
- Data here is stored in radians, but can be only set as **degrees** (calls *rad_to_deg* and *deg_to_rad*)
Type: *float* (degrees)
Default value: *0.0*
Min, Max, Step: *-180, 180, 0.01*
##### "joint_constraints/angular_limit_softness"
Type: *float*
Default value: *0.5*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/angular_restitution"
***warning! - this name differs from slider joint***
Type: *float*
Default value: *0.0*
Min, Max, Step: *0.01, 16.0, 0.01*
##### "joint_constraints/angular_damping"
***warning! - this name differs from slider joint***
Type: *float*
Default value: *1.0*
Min, Max, Step: *0.0, 16.0, 0.01*
##### "joint_constraints/erp"
*stares*
Type: *float*
Default value: *0.5*

##### "joint_constraints/angular_spring_enabled"
Type: *bool*
Default value: *false*
##### "joint_constraints/angular_spring_stiffness"
Type: *float*
Default value: *0.0*
##### "joint_constraints/angular_spring_damping"
Type: *float*
Default value: *0.0*
##### "joint_constraints/angular_equilibrium_point"
Type: *float*
Default value: *0.0*

