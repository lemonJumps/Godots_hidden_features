A helper class for `RefCounted`

I won't type out it's interface as it's pretty simple.
However, the important part for ref is how to use it and by extent, how to use `RefCounted`.

the two main functions are `RefCounted.reference()` and `RefCounted.unreference()`.
Which count how many references to `RefCounted` object there are.

if this number goes to zero, the object is marked for deletion.
this is indicated by the return value of `RefCounted.unreference()`. *Beware that this doesn't automatically delete the object*, rather the Ref class is expected to do this.

This means that `RefCounted` object that is kept either with *external reference* or has it's reference count increased on its own, *will never be deleted* even if the reference is decreased to 0.
And **you'll need to provide your own deletion for the object** otherwise this will cause memory leaks.
 

#### How to take reference
Calling `Ref( RefCounted )` will automatically take reference and return a `Ref<RefCounted>`.
This will also happen if you simply assign an empty `Ref` as well:
```
Ref<abc> ref = ref2;
```

#### How to create a reference with constructor that takes parameters
Normally you'd use `ref.instantiate()` to create a new object, but this doesn't work if you have parameters.
So instead you can wrap an object in memnew, (*this is how it's actually done inside of* `instantiate()`)
```
Ref<abc> ref(memnew(
	abc(param1, param2) // non default constructor
	));
```
note: *formatting has no effect here, it's formatted like this to emphasize the constructor*
#### Implementations as they appear in engine:
```
bool RefCounted::reference() {
    uint32_t rc_val = refcount.refval();
    bool success = rc_val != 0;

    if (success && rc_val <= 2 /* higher is not relevant */) {
        if (get_script_instance()) {
            get_script_instance()->refcount_incremented();
        }
        if (_get_extension() && _get_extension()->reference) {
            _get_extension()->reference(_get_extension_instance());
        }
        _instance_binding_reference(true);
    }

    return success;
}

  ```

```
bool RefCounted::unreference() {
    uint32_t rc_val = refcount.unrefval();
    bool die = rc_val == 0;

    if (rc_val <= 1 /* higher is not relevant */) {
        if (get_script_instance()) {
            bool script_ret = get_script_instance()->refcount_decremented();
            die = die && script_ret;
        }
        if (_get_extension() && _get_extension()->unreference) {
            _get_extension()->unreference(_get_extension_instance());
        }

        bool binding_ret = _instance_binding_reference(false);
        die = die && binding_ret;
    }

    return die;
}
```