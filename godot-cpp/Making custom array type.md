Continuation of [[Typed array (or how to do arrays in gdextension)]]

So, whyy would you want to do this?
Well, `std::Vector<>` is the biggest reason...

Godot has `Arrays`, and the rest of C++ libraries use `std::Vector<>` or  `std::Array` or any other iterated type, this isn't a problem for anyone wanting to write a simple api, since you can just write a set and get function for the array.

But this is kind of painful, especially for the end user.

So let's see if we can convince `ClassDB` that our class is an array.

