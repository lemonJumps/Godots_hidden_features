SO!

This is very possible to do in godot, but it's not even remotely documented. I've gotten this implementation from [stack overflow](https://stackoverflow.com/questions/77651562/how-do-i-properly-configure-typedarrays-in-gdextension).

Where a user figured it out. And it makes sense, essentially, `ClassDB` is what keeps all the info about anything that happens.

Sooo, If we want to make an array type, all we need to do is to tell `ClassDB` that our type is an array, or implements `operator[]`.


#### from the stack overflow question
What we get here, is the actual usage of already created array of custom type.
So what this is: **usage of custom type/class in godot Array.**
What this is not: **creation of custom class that contains an operator[].**


```cpp
class TestObject : public Resource { 
//... 
};

class TypedArrayTest : public Resource {
    GDCLASS(TypedArrayTest, Resource)

private:
    TypedArray<TestObject> test_array;

protected:
    static void _bind_methods();

public:
    TypedArray<TestObject> get_test_array() const;
    void set_test_array(const TypedArray<TestObject> p_value);
};

//test.cpp
void TypedArrayTest::_bind_methods() {
    ClassDB::bind_method(D_METHOD("get_test_array"), &TypedArrayTest::get_test_array);
    ClassDB::bind_method(D_METHOD("set_test_array", "p_value"), &TypedArrayTest::set_test_array);
    ClassDB::add_property("TypedArrayTest", PropertyInfo(Variant::ARRAY, "test_array"), "set_test_array", "get_test_array");
}
```
The code above works, but! it doesn't show the type when exporting it :P

but this modification works, it essentially provides additional type hints to the engine
```cpp
void TypedArrayTest::_bind_methods() {
    //...

    ClassDB::add_property(
        "TypedArrayTest", 
        PropertyInfo(
            Variant::ARRAY, 
            "test_array", 
            PROPERTY_HINT_TYPE_STRING, 
            String::num(Variant::OBJECT) + "/" + String::num(PROPERTY_HINT_RESOURCE_TYPE) + ":TestObject"
        ), 
        "set_test_array", 
        "get_test_array"
    );
    
}
```
#### making your own array type

[[Making custom array type]]

PS: *I'm unsure weather to include people or not, so for foreseeable future I'll use neutral terms and simply link to where I found it.*