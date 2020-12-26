# C++

----

__cs 205__





## Pointer





指针就是数据在内存里的地址

高级语言（JAVA、Python）：内存自动回收。C++可以直接处理内存

- Allocate Memory: C Style
- Allocate Memory: C++ Style
- Managing memory for data



### - What's a Pointer?

address in memory. 

```c++
cout << cups << endl; // value
cout << &cups << endl; // address

int jumble = 23;
int * pe = &jumbo;
// & 取地址 
// int * 是一个int类型的指针，它需要指向一个int类型的数据
cout << pe; // address
cout << *pe; // int value
```

- Declaring and Initializing Pointers

Example:

> int* birddog
>
> - \* birddog is a int type variable
> 
> - birddog is a ==pointer type variable==
> 
> - The t



- A disaster: 

```c++
int * ptr;
// it‘s a random value, the compiler doesn’t care whether there’s important value or not!

* ptr = 2;
cout << * ptr << endl;
// This may not leads to an error, but it will fuck up your memory!
```



- a good way: 

```c++
int * ptr = 0;
int * ptr = null;
// this will lead to an error if you try to access ptr.
```



- Similarities and differences between pointer and integer

  > they are both integers but pointers are not the integer type 
  >
  > Both are numbers and can apply add and minus, no meaning if \* or \\ .






### Allocate Memory

---



#### in C

```c
void* malloc( size_t size );
// size_t is the unsigned integer type of the result of the sizeof operater
// it can store the maximum size of a theoretically possible object of any type (including array)
```

> - __DON’T__ forget to free the memory
> - `void free( void* ptr );`
> - The address of `ptr` will NOT be NULL() after you free the memory

Example:

```c
int main(void)
{
    int *p1 = (int*)malloc(4*sizeof(int));
    // allocate enough for an array of 4 int
    double *p2 = (double*)malloc(4*sizeof(double));
    // 检查是否成功分配内存 for all == 0
    
    if(p1) {
        for (int n - 0; n < 4; n++) //populate the array
            p1[n] = n*n;
        for (int n = 0; n < 4; n++) //print it back out
            printf(“p1[%d] == %d\n”, n, p1[n]);
    }
    
    // ... p2
    
    free{p1};
    free{p2};
    *p1 = 0;
    *p2 = 0;
}
```



- How it works

```c++
int * ptr = 0;
ptr = (int *) 
```





#### in C++

In C++, we use `new`, not `malloc()`

1. `int * ps = new int;`

   `delete ps;`

   only pointer that are allocated by ==`new`== needs to `delete`

2. `int * psome. = new int [10]// get a block of 10 ints` 

   _that is 40 bytes_

   `delete [] psome;` remember the ==[]==

3. 对于`int *`类型指针，`psome + 1` 实际上是 `+ 4`

Example:

```c++
structure inflatable {
    char name[20];
    int year;
}

int main() {
    inflatable * ps = new inflatable;
    inflatable * p2 = & ps;
    
    ...//initialization
    
    std::cout << (*ps).year;
}
```



- cases that `new` and `delete` can’t appear together





### Managing memory for data

---

- Dynamic Storage





### Code Choices

---

```c++
#include <iostream>
#include <vector> // STL C++98
#include <array> // C++11

int main()
{
    using namespace std;
    
    // C, original C++
    double a1[4] = {1.2, 2.4, 3.6, 4.8};
    
    // C++98 STL
    vector<double> a2(4); // create vector with 4 elements
    // no simple way to initialize in C98
    a2[0] = 1.0/3.0;
    a2[1] = 1.0/5.0;
    a2[2] = 1.0/7.0;
    a2[3] = 1.0/9.0;
    
    // C++11 -- create and initialize array object    
    array<double, 4> a3 = {3.14, 2.72, 1.62, 1.41};
    array<double, 4> a4;
    a4 = a3; // valid for array objects of same size
    
    // use array notation
    cout << "a1[2]: " << a1[2] << " at " << &a1[2] << endl;
    cout << "a2[2]: " << a2[2] << " at " << &a2[2] << endl;
    cout << "a3[2]: " << a3[2] << " at " << &a3[2] << endl;
    cout << "a4[2]: " << a4[2] << " at " << &a4[2] << endl;

    // misdeed    
    a1[-2] = 20.2;
    cout << "a1[-2]: " << a1[-2] <<" at " << &a1[-2] << endl;
    // but
    for(int i = -4; i < 4; i++)
        cout << i << ":" << a3[i] << endl;
    return 0;
}
```



## Function

**Pointer used in functions**





## Adventures in Functions

- 深度学习

### Mechanism of Function Call

- when the compiler see a function :
    - save the parameter to the stack
    - goto Function
    - put out the parameters
    - do the func
    - return to the previous location

### Default Arguments

- How do you establish a default value?

> You must use the **function prototype**

- When you use a function with an argument list, you must add defaults **from right to left**.

    `int harpo(int n, int m = 4, int j = 5);`

### Function Templates

- Define a function in terms of a **generic tyep**
    - A specific type, such as `int` or `double`, can be substituted
    - The process in termed `generic programming`
    - The keywords `template` and `typename` (class)

```c++
template <typename AnyType>
void Swap (AnyType &a, AnyType &b) {
    AnyType temp;
    temp = a;
    a = b;
    b = temp;
}
```

### Specializations

- Explicit specializations
    - Template: merely a plan for generating a function definition
    - Instantiation: use the template to generate a function
        - Implicit: the compiler deduces the necessity for making the definition
        - Explicit: using the `<>` notation to indicate the type and prefixing the declaration with the keyword `template`

```c++
template <> void 
```

### Which Function Version Does the Compiler Pick?

- Multiple fuctions of the same name
    - Include: function overloading, function templates, and fuction template overloading
- Overload resolution
    - Phase 1--Assemble a list of candidate functions
    - Phase 2--From the candidate functions, assemble a list of feasible funcitons
        - Correct number of arguments
        - An exact match for each type of actual argument to the type of the corresponding formal argument
    - Phase 3--Delermine whether there is a best viable fuction


### Speed up your program

- An example:
    - Our teacher uses a `.cpp` document to store the data, so that it reads fast & prevents loading errors
    - 1700+ lines -- complete deep learning
    - `$g++ main.cpp -c -o main.o -std=c++11`: `-c` only compile
    - `$g++ main.o matoperation.o -o dotp`: links two `.o` files
    - the duration change? why? :probably some asshole apps take your cpu, nothing big issues
    - `g++ main.cpp matperation.cpp -o dotp -std=c++11 -O3`: `-O3`(not `-03`) compile fastest way, use when the program calculates a lot --> **IMPORTANT**
    - sometimes unfold `for()` runs faster, use when the length is for sure
    - The first time computer execute a function, it needs to push it into cache first, so orders matters.
    - The speed often depends on the speed of the memory I/O, not the cpu's.
    - How to prevent this? Run the funciton 2 times ahead ;)
    - If you read the data in an ascending order, it is usually faster.

- Using **SIMD**: Single instruction, multiple data
    - do `+` three times, rather than do the addition multiple times, it do it in a single circulation.
    - `#pragma omp parallel for`: even faster: using all the cores of the cpu, requires openMP
        - bugs: parallel program: do operations on the same data at the same time, cconflict-->take 0 or 1 randomly
    - 


## Makefile

**How to use it**

if you want to compile two `.cpp` file together
- `g++ -c main.cpp -std=c++11` and `g++ main.o matoperation.o -o dotp`

However for multiple workfiles, it is hard to do it manually

Use Makefile: the input file to `make` mingling
- `make -f Makefile`
- or `make` if makefilename=Makefile
- `make clean`: clean the `.o` (you config it); remenber to clean the `.o`(s), or it will not update it
- `make main.o`

- RULES
    A makefile consists of a set of rules. A rule including three elements:
    1. target,
    2. prerequisites
    3. commands

```makefile
targets: prerequisites
    command
```

- The **target** is an object file, which means the program that needs to compile.
- The **prerequisites** are file names, separated by spaces.
- The **commands** are a series of steps typically used to make the target(s). These need to shtart with a <kbd>tab</kbd> character, not <kbd>spaces</kbd>.

```makefile
# Since the hello target is the first, it is the default target
# and will be run when we run "make"
hello: main.cpp factorial.cpp printhello.cpp
g++ main.cpp factorial.cpp printhello.cpp -o hello
```


- Other Examples

```makefile



CXXFLAGS=
LDFLAGS=


dotp: main.o matoperation.o # don't leave extra lines below!
    $(LD) $+ -o $@ $(LDFLAGS)
    // or do everything you want
    #g++ main.o matoperation.o -o dotp same as below

main.o:main.cpp

matoperation.o:matoperation.cpp

clean:
    rm *.o dotp
```

- `#include`

```c++
#pragma once // avoid including a head file twice
#include <csddf>
#include 'balabala.hpp'
```
- other IDEs: do the makefile for you

- CMAKE: help you write a makefile
    - different machine different properties
    - it will generate a very long makefile for you.

### Introducing Friends
* Access control
    * Can access the public portions directly
    * Can access the private members of an object only by using the public member functions
    * Problems:
        * Public class methods serve as the only access
...


#### Motivation for Friend Functions
    * Left operand is the invoking object
        * Time Time::operator*(double mult) const
        * A = B * 2.75; A = B.operator*(2.75); —> yes
        * A = 2.75 * B; —> how?
    * put before the class: `friend Time operator* (doublem, const Time & t)` 不是类的成员函数！only friends.
        * `Time operator*( double mult, Time in)`
        * cannot use `Time::operator*()`





## Strange Bugs & Problem Shooting



When you think the compiler is wrong, and you have tried everything you can ever think of, maybe it is your algorithm itself that is wrong from the beginning. 



- When leaving blanks in `for()`…

```cpp
long long p = 0; double i = 0;
for (; p < 5; i++)
    p += pow((double)2, i);
```

Seems like p = 7, and i will be ==2== when leaves the `for()`;

However **i = 3** at last. It do the `i++` once more after comparing it. 



- While participating CCPC…

```c++
struct node {
    int key;
    int day;
} a, b, c, d;

node v[4] = {a, b, c, d};

int main() {
    scanf("%d%d%d%d", &a.key, &b.key, &c.key, &d.key);
    scanf("%d%d%d%d", &a.day, &b.day, &c.day, &d.day);
    something...
}
```

When checking v[0].day, I found it equals to 0. But a.day is OK. 

It is because v[4] has copied 4 new nodes into itself, not a, b, c, d. 









