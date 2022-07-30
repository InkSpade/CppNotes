# Static outside of a class or a struct
## 情况1
static.cpp
int s_Variable = 5;
source.cpp
extern int s_Variable;
这条语句是在external translation unit to reference变量s_Variable

## 情况2
static.cpp
static int s_Variable = 5;
没有其他的translation unit能够看到这个s变量，the linker不会在global scope里看到这个变量
同理
static void function() {}在哪个cpp file 被declared in，就只在这个translation unit可见visible

## 注意
全局变量很不好，最好mark函数和变量为static，除非你需要他们be linked across translation units


# Static inside a class or a struct
见CppSeries的StaticInClass
## static variables
不管你make a whole bunch of class instances，都只有一个变量
因此，对于在类中的静态变量来说，There is no point of referring to the variable through a class instance，因为，对于这个class来说，这个static的变量是global instance

## static methods
同理于static methods
a static method can be called without a class instance
inside a static method，你不可以写code which refers to a class instance，因为你没有that class instance to refer to
注意，在class中定义static methods的时候，不可以调用non-static的成员变量

# static in local scope
见CppSeries的StaticInLocalScope

declare a variable时，考虑两点，life time of the variable and the scope of the variable
how long it will remain in our memory before it gets deleted
the scope refers to where we can actually access that variable(declare in a function, only this function can access, be local to the function we declared the static variable in)

总之，a static local variable allows us to declare a variable that has a lifetime of essentially our entire program however its scope is limited to be inside that function(scope, inside if statement).

## 函数里的static变量
第一次调用才会construct，后面都是用同一个，变量的生命周期是forever，extending the life time of this static variable in function to be essentially forever, which means that every time we call get the first time we actually construct that singleton instance and then all subsequent times it will just return that existing instance.



