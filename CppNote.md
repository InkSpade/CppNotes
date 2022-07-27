# 一、Loops in C++

## 1. Control flow statements

apply to all loops: for loops, while loops, do-while loops

### 1 continue

when continue gets hit, it will give to the next iteration of the for loop

### 2 break

breaks for loop

### 3 return

`std::cin.get();` //this statement keeps our window open
return 可以写在程序的任意地方，不必一定写在loop里
用于exit the function

# 二、POINTERS in C++

## 1. Something to say

Raw pointers(not smart pointers)
a pointer is an integer, a number which stores a memory address.
that's all it is.

put your memory inside your computer is just like one big blob, it's like one big line.
that's all it is.

metaphor:
there's just a street and there's like a row of houses.
that is what memory is in a computer
it just a linear one-dimensional line and every house on that street is going to a number an address
picture that every house on that street that has an address is a byte, it is one byte of data.
pointer is that address the address that tells us where that house is, where that specific byte of memory is.

pretty much everything we do in our code is going to be reading or writing from and to memory. a pointer is just an address, it is an integer which holds a memory address.

forget about types(the differernce of types is only how big are they), types are just some kind of fiction that we've created to make our lives easier.
types are completely meaningless.
a pointer for all types is just that integer that holds a memory address.

that's all it is.

## 2. Jump in pointer, gist of how they work

### 1 a useless pointer

typeless and has the memory address of 0
`void* ptr = 0;` or `void* ptr = NULL;` or `void* ptr = nullptr;` 
void means we don't care about what type the data stored in this address is.
`= 0` means this pointer is not valid

### 2 pointers and memory

`int var = 8;`
every variable we create has a memory address because we need a place to store that variable, so what if i want to find out what the memory address of that variable is, where are you in memory

we can do that by using the ampersand operator
`&var`   we essentilly asking this variable hey what is your memory address and then of course we're taking the memory of that variable, and assigning it to a new variable called pointer.
`void* ptr = &var;`
so what the pointer variable is holding now is the memory address of that var variable, 0x00a0fb64 is where in memory we are storing our integer variable.

Hit DEBUG->Windows->Memory->Memory1

```
0x00A0FB64 08 00 00 00 cc cc cc cc ca...
0x00A0FB8C
```

这里的 08 00 00 00 就是int变量var

`void* ptr = &a;` `int* ptr = &a;` `double* ptr = (double*)&a;` (cast)
指针有多大取决于很多东西，随便是32-bit integer, 64-bit integer, 16-bit integer，不重要
类型对manipulation of that memory有用，当我想对那块内存读写时，类型可以帮助我，编译器会知道一个整数应该是4字节，所以当我想writing to the data at that memory address时，它会设置4字节的内存
types are completely meaningless

### 3 why not just using the void pointer

when we want to use the variable(be pointed by a pointer)
我们会用到dereferencing解引用
`*ptr` means i'm now accessing the data, i can read from or write to that data
`int a = 8;`    (这种创建变量的方式是在内存的stack部分创建的，我们也可以在heap上创建变量，让电脑allocate some memory，并且我想让它是一个特定的尺寸，见后面)
`void* ptr = &a;`   创建了一个无类型的指针
`*ptr = 10;`  now error occurs, is that 10 short, which is 2 byte integer, is is an int which is 4 byte integer, is it a long long which is an 8 byte integer. we don't know how many bytes of data should this right. 10 can mean anything.
that's what tpyes come in, we need to tell the compiler actually know this is an integer so i expect you to write 4 bytes in please.

we've told the compiler! 如果我们给指针错了应该给的类型，那就麻烦了。

### 4 create variable on the heap

something to say, a pointer just points to a location in memory.
some people say it points to a block of memory(not accurate, we don't know how big that block of memory is, in the above example, our pointer does point to essentially a block of 4 bytes, 但是实际的指针并没说内存有多大，)

如果我们想要8字节大的内存
`char* buffer = new char[8];` (we know char is 1 byte, we ask for 8 bytes of memory, so we use char[8] to allocate 8 bytes)
char[8] is an array we use the array operator to allocate
这行代码has allocated 8 bytes of memory for us and is returning a pointer to the beginning of that block of memory
we're storing a pointer to the beginning of that data.

`function memset()`
这个函数basically fills a block of memory with data that we specify, it takes in a pointer which is going to be the pointer to the beginning of the block of memory. and it's going to take a value such as zero and then it's going to take a size.

```
char* buffer = new char[8];
memset(buffer, 0, 8);
```

进入memory view视图，在Address中输入variable name buffer, and we'll see that we've got 8 bytes of memory in a row that are all set to zero

in this case we use the `new` keyword and this data is actually heap allocated, we should also delete the data when we're done with it. we should use the delete keyword with the array operator and delete that buffer
`delete[] buffer;`

### 5 last but not least, pointers to pointers

pointers themselves are just variables and those pointers, those variables are also stored in memory.
that's where we can get things like double pointers or triple pointers. basically pointers to pointers how does that work? you just go down a level and you're basically just saying i now have a pointer which points to my pointer,
so i now have a variable which is storing the memory address, that's pointing to another variable which is storing a memory address.
simple.

```
char* buffer = new char[8];
memset(buffer,0,8);
char** ptr = &buffer;
```

`char**` 是指，ptr所指向的内存里用于存放char类型的数据，即另一个指针变量的地址，可能是16进制表示所以有abcd所以是char？，但是具体多大数据我们是不知道的
将指针ptr的值输入到memory view的Address上，
`0x00BCF7D8`
我们看到这个内存地址里存储着`b8 f1 02 00`一共4个字节的数据
tips:
when running a 32-bit application, 在32位程序中，a memory address is 32 bits，4字节

because the endianess of this computer is actually in reverse order so we have to rearrange it.
所以正确的顺序应该是`00 02 f1 b8`

将这个内存地址输入Address
we're taken to the memory which stores this buffer of zeros.

the pointers are just integers which store memory address, that's it.

# 三、REFERENCE in C++

references are just pointers
reference is a way for us to reference an existing variable
it has to reference an already existing variable
references themselves are not new variable, they don't really occupy memory, they don't really have storage
unlike typical variables, what they are instead is a reference to a variable

## 1. example of as an alias

`&a` the ampersand operator here is to take a memory address of an existing variable

`int&` the ampersand here is actually part of the types

```
int a =5;
int& ref = a;
```

now we've essentially created something called an alias for `a`
this `ref` here is `a variable in quotes` `'variable' ref`

## 2. example why we need to use reference, pass by what

1 in the following example, we just passing this by value
2 we're not passing this as a pointer or as a reference or anything
3 it actually going to copy this value 5 into this function, just copy, it's gonna create a brand new variable called `value`
4 it just like we were to write `int value =5;`in the first line of the body of the functuon `Increment`, that's exactly what will happen
5 if we log a, and in this example, `a` will not increment, only the variable `value` will increment

```
void Increment(int value)    //2 //3
{
    //4
    value++;
}

int main()
{
    int a = 5;
    Increment(a);    //1
    LOG(a);    //5

    std::cin.get();
}
```

so we need to pass this variable by reference in order for it to increment
hold on, let's see a way first

## 3. example of passing an variable by reference into a function

what we really want to do, is to affect the variable of `a`
how can we actually modify this variable by passing into a function

pointers are memory addresses
so we could instead of passing the actual value `5` into that function, we could pass the memory address of this `a` variable, we could write to that memory address

so, we can modify the function to actually take in a pointer
`void Increment(int* value);`
and in the 调用语句, i'll pass the memory address of `a`
`Increment(&a);`
and then, we have to modify the body
`*value++;`    we reference this value（改成解引用dereference的形式，the dereference operator, 即加asterisk*） so that we can actually `write to that memory` instead of modifying the pointer itself, or it's going to just increment that memory address, not the actual value
but, because of the order of operations
`(*value)++;` dereference the pointer first and then increment the value at pointer

but we can simplify this way

## 4. just use a reference to simplify the above, pass by reference

less code
without decorating our syntax

```
void Increment(int& value)
{
    value++;
}

int main()
{
    int a = 5;
    Increment(a);
    LOG(a);

    std::cin.get();
}
```

引用能做的事，指针都能做
cleaner and simpler to read

## 5. tips

1. reference requires an initialize, immediately assign it something, it has to reference something

2. you cannot change 

```
#include <iostream>
#define LOG(x) std::cout << x << std::endl;

int a = 5;
int b = 8;
int& ref = a;
ref = b;    //this will cause a = b = 8, will not cause ref references b
```

if i want to change what ref was reference of, i have to use pointers to realize the switching to b

```
int a = 5;
int b = 8;
int* ref = a;    //pointer to a
*ref = 2;    //to actually start assessing the value that this pointer is pointing to, i need to dereference the pointer first, and then set this equal to something. this case i'm setting a equal to 2
ref = &b;    //switch to b, being pointer to b
*ref = 1;    //in this case i'm setting b equal to 1

LOG(a);
LOG(b);
```

# 四、CLASSES in C++

## 1. object-oriented programming

what kind of things do we need to actually represent a player
1 some kind of data, e.g., the position of the player in the game
2 the speed at which the player moves
3 3d model represent the player
all these data needs to be stored somewhere

### 1 create variables for all these

```
#include <iostream>
#define LOG(x) std::cout<< x << std::endl

void Move(int x, int y, int speed)    //要传入很多参数

int main()
{
    int PlayerX0,PlayerY0;
    int PlayerSpeed0 = 2;
    int PlayerX1,PlayerY1;
    int PlayerSpeed1 = 2;
        //unorganized

    std::cin.get();
}
```

### 2 created by class

```
#include <iostream>
#define LOG(x) std::cout << x << std::endl
class Player    //the definition of Player Type         
{
    int x, y;    //for the position
    int speed;
};
```

//类名要unique, because classes are types, a new variable type
//因为类是变量，所以花括号后面有;semicolon at the end of the closing brace
//we create a brand new class called player which is essentially its own type
//故当要使用这个Player类型的时候，我们可以像创建其他任何变量一样

```
int main()
{
    Player player;        //TheType TheGivenName 1
    player.x = 5;        //set those variables 2
}
```

//variables that are made from class types are called objects
//and a new object variable is called an instance
//1 so what we've done here is we've instantiated a player object，实例化了一个Player对象, because we've credit a new instance of that Player type

//2 但是上面的definition是有问题的，在设置变量值时，会报错，player对象不能access private member declared in class Player

## 2. visibility

when you create a new class you have the option to specify how visible the `stuff` in that class actually is
by default a class makes everything private, which means that only function inside that class can access those variables
however we actually want to be able to access these variables from the main function
so

```
#include <iostream>
#define LOG(x) std::cout << x << std::endl
class Player    //the definition of Player Type         
{
public:            // 1
    int x, y;    //for the position
    int speed;
};
```

//1 public means that we're allowed to access these variables outside of this class anywhere in our code

so we now have managed to drastically clean up the code and have all of our variables in one place

## 3. function

### 1 function standalone

we've got all this data
if we make standalone function

```
void Move(Player& player, int xa, int ya)    // 1
{
    player.x += xa * player.speed;
    player.y += ya * player.speed;
}

int main()
```

//xa and ya is the amount that we move the player by in both x and y, according to the speed, we can make out the distance
//1 这里传入函数的实参和函数自己的形参之间其实相当于在函数体内做了以下语句Player& player = INPUT; int xa = INPUT; int ya = INPUT;
//1 由于是引用，所以不需要函数写return语句，直接void函数就行

when we want to call that
we would just write

```
int main()
{
    Player player;
    Move(player, 1, -1);

    std::cin.get();
}
```

这样我们实现了对player的移动，但是我们还可以做得更好

### 2 function inside class

classes can actually contain functionality
so we can move function into the class
function inside classes are called methods

```
#include <iostream>
#define LOG(x) std::cout << x << std::endl
class Player    //the definition of Player Type         
{
public:
    int x, y;
    int speed;    //three variables exist in the Player type
    void Move(int xa, int ya)    //a function which manipulates those variables
    {
        x += xa * player.speed;
        y += ya * player.speed;
    }
};

int main()
{
    Player player;
    player.Move(1, -1);

    std::cin.get();
}
```

classes allow us to group variables together into a type and also add functionality to those variables
we got data and functions to manipulate that data
calsses are essentially just syntactic sugar that we can use to organize our code and make it easier to maintain, that's all they are

# 五、CLASSES vs STRUCTS in C++

## 1. two terms in C++, what the differences are, when should be using

a struct which is short for structure
a class
there is basically none differences
there is one small difference to do with visibility

in a calss
if not add `public:`
only other methods inside the class can actually access the `Move` method

```
class Player         
{
    int x, y;
    int speed;
    void Move(int xa, int ya)
    {
        x += xa * player.speed;
        y += ya * player.speed;
    }
};
```

a class is private by default, so if you don't specify any kind of visibility modifier like `public:`, the default is private
to the struct, the default is public, if i actually did want something to be private, i would explicitly have to write`private:`

```
struct Player         
{
private:
    int x, y;
    int speed;
    void Move(int xa, int ya)
    {
        x += xa * player.speed;
        y += ya * player.speed;
    }
};
```

that's really all there is to it

## 2. the usage in code is actually going to differ

the only reason struct really even exists in C++ is because of the backwards compatibility兼容性 that it wants to maintain with C.
because C code doesn't have classes, it does have structures though, and if we were suddenly to wipe out this whole `struct` Keyword entirely, then we would lose all compatibility

but if we delete `struct` in C++, we can fix it pretty easily by just using a hash define `#define`. so we could write something like `#define struct class`, then we replace all of our structs with classes. that means the `struct` in the code will be replaced with the word `class`. be noted that then you should add `public:` in the definition of `struct` in C code before copying the code to C++.

## 3. My style of programming 1

i like to use `struct` whenever possible, when i'm basically talking about plain old data or pod basically where i'm talking about some kind of structure which just represents variables, a bunch of variables, that's really all it's there for.
A great example of this might be something like a mathematical Vector class
example, i want to make some kind of structure which just held two floats together

```
struct Vec2
{
    float x, y;        //just have my x and y float
};
```

because fundamentally this Vec2 class or struct is just a representation of 2 floats, 这个Vec2结构只是两个浮点数的表现形式
that's what it is at its core
it's not supposed to contain a massive amount of functionality like a `Player` class(Player class has a 3D model, it might handle all the rendering code for that 3D model, it might handling how the player moves around the map, taking in keyboard input and all of that)
it's just 2 variables and we've grouped them literally just for the reason of making our code easier to use

but i will add method to this, just for manipulating these variables, i'm still just talking about those two variables到头来

```
struct Vec2
{
    float x, y;
    void Add(const Vec2& other)
    {
        x += other.x;
        y += other.y;
    }
};
```

there is a bit of difference in terms of design, it's something massively more complicated

## 4. My style of programming 2, the other scenario, inheritance

i will never use inheritance with structs
if i'm going to have an entire class hierarchy or some kind of inheritance Hierarchy, i'm going to use a class
inheritance is something that's adding another level of complexity, and i just want my structures to be structures of data
that's it

if you have a class called A, and then a struct called B which inherits from A, some compliers will give you warning telling you that you're inheriting from a class but you're a struct, your code will still work, it's just warning and a bit of a semantically语义上的 difference

## 5. Conclusion, reasons for using a struct versus a class, differentiate between those two types

if i just want to represent some data in a structure, i will use a `struct`.
but if i've got an entire class filled with functionality like a game world or a player or something that additionally might have inheritance and all these systems, i'm going to use a `class`.

# 六、How to Write a C++ Class

from this episode, you will get to see the process and the difference of going from a basic version of a Class to a more advanced version that does the same thing but could be considered as a better code
write a Class from scratch从头开始

## why to write a basic Log calss

the Log class is going to be a way for us to manage our log messages. it's messages or information that we want our program to print to the console
This is usually used for debugging purposes and it can really help us out in our game or application, if we wanna kinda see what's going on and just print the status of things into our console.
Because the console in our application is really like an information dump信息转储, which we can use to just print out what's going on, it's something that is practically guaranteed to always work
if we have a graphical display of a console in our game or something that might be a little bit different, and it might not work all the time, because if there is something with our graphics rendering system or something like that, we might not get those messages printing properly
However, a console is something that's basically built into the operating system, so we can pretty much guarantee that it will always work

关于为什么选择Log class，因为Log类对debugging和development非常重要，it is definitely worth spending time on that
另外，Logging systems can do things like:
1 print just the console
2 print console in different colors
3 actually output log messages to a file or a network
you can literally make a Log class be ten lines of code or ten thousand lines of code.

## Log的功能划分

it's going to basically probide the ability to write text to the console
and it's going to maintain some log level which is what level of log messages we actually want to send out to the console
有三种等级
error
warning
message or trace messages跟踪消息

## 开始编写1

a really good way of creating a new class or designing an API is by its usage

in the main function, i'm going to instantiate it
注意我们一般we're just going to use `const char pointer` as `strings` for now，后面会讲到更多的
下面看一个简单的例子，这个例子中，0是Error，1是warning，2是information

```
#include <iostream>

class Log
{
private:
    int m_LogLevel    //m的意思是提醒我，这个变量 is a class member variable that is private, this is a convention公约, to organize your code and keep it clean

public:
    void SetLevel(int level)
    {
        m_LogLevel = level;        //assign our member variable with the parameter(local variables)
    }
    void Warn(const char* message)
    {

    }
};

int main()
{
    Log log;
    log.SetLevel(2);    // 1
    log.Warn("Hello!");
    std::cin.get();
}
```

1 if i call SetLevel with 2, it will assign the value 2 to our member variable `m_LogLevel`
但是这样的问题是，没有人知道2是什么，一个人raise了这个代码，他不知道2是什么
所以我们要预设一些public variables

## 改进编写2
```
#include <iostream>

class Log
{
public:		//public variables, and we can create a new section for public static variables
	const int LogLevelError = 0;
	const int LogLevelWarning = 1;
	const int LogLevelInfo = 2;	//Trace/information messages
private:
	int m_LogLevel = LogLevelInfo;//默认将成员变量m_LogLevel设置为正常的信息输出等级everything should get printed
public:		//public methods
	void SetLevel(int level)
	{
		m_LogLevel = level;
	}

	void Error(const char* message)
	{
		if(m_LogLevel >= LogLevelError)
			std::cout << "[ERROR]: " << message << std::endl;
	}
	void Warn(const char* message)
	{
		if(m_LogLevel >= LogLevelWarning)	//既然你call Wran 函数了，那么你的level一定要至少是1
			std::cout << "[WARNING]: " << message << std::endl;
	}
	void Info(const char* message)
	{
		if(m_LogLevel >= LogLevelInfo)
			std::cout << "[INFO]: " << message << std::endl;
	}
};

int main() {
	Log log;
	log.SetLevel(log.LogLevelError); //because our level was set to warning, so only error will get printed
	//log.SetLevel(log.LogLevelWarning); //because our level was set to warning, so the info will not get printed
	//if we never set a log level, it should be using info as default
	log.Error("Hello");
	log.Warn("Hello!");
	log.Info("Hello!");

	std::cin.get();

}
```
