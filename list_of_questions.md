# sizeof (\<empty>)?
**complexity:** professional
```cpp
class Empty {};

int main()
{
    cout << sizeof(Empty) << endl;
}
```
Regarding code above what should be present in output?
- A. 0
- B. 1
- C. 2
- D. 4
- F. compiler dependent, but definitely greater than 0

**Answer:** F(B)

When instantiating an instance of any type (including "empty"), the physical memory must be allocated to it, since `operator &` (address acquisition) can be applied to every object in C ++. The Standard does not specify size of "empty" object. The minimum possible amount of memory is 1 byte and VC++ allocates it.

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 


# virtual destructor
**complexity:** basic
```cpp
struct shape
{
    virtual ~shape()       { cout << "shape destructor" << endl; }
};
struct rectangle : shape
{
   ~rectangle()            { cout << "rectangle destructor" << endl; }
};
struct decorated_rectangle : rectangle
{
   ~decorated_rectangle()  { cout << "decorated rectangle destructor" << endl; }
};

int main()
{
   rectangle* r = new decorated_rectangle();
   delete r;
}
```
Regarding code above what should be present in output?
- A.
    - rectangle destructor
- B.
    - decorated rectangle destructor
    - rectangle destructor
- C.
    - decorated rectangle destructor
    - rectangle destructor
    - shape destructor
- D.
    - shape destructor
    - rectangle destructor
    - decorated rectangle destructor


**Answer:** C

**See also:** [S. Meyers. Effective C++, item 7](https://books.google.com.ua/books?id=U7lTySXdFk0C&lpg=PT68&dq=Effective%20C%2B%2B%20declare%20destructors%20virtual&pg=PT68#v=onepage&q&f=false)
 
**Relatives:** [virtuality](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#cost-of-the-virtuality)

# Object construction
**complexity:** basic
```cpp
struct member
{
   member() { cout << "member" << endl; }
};

struct base
{
   base()  { cout << "base" << endl; }
};

struct derived : base
{
   derived() { cout << "derived" << endl; }
private:
   member m_;
};

int main()
{
   derived obj;
}
```
Regarding code above what should be present in output?
- A.
    - member
    - base
    - derived
- B.
    - base
    - member
    - derived
- C.
    - base
    - derived
- D.
    - base
    - member
    - derived
- F.
    - derived

**Answer:** B 

**See also:** ???

**Relatives:** [ctor::exception](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction-exception)
 

# Function overloading. Parameter type: integer vs. pointer
**complexity:** professional
```cpp
void foo(char*) {cout << "pointer argument" << endl; }
void foo(int)   {cout << "integer argument" << endl; }

int main()
{
   foo(NULL);
}
```
Regarding code above what should be present in output?
- A. pointer argument
- B. integer argument
- C. compiler error: ambiguous call to overloaded function

**Answer:** B

**NULL** is predefined macro with integer constant 0 (zero). foo(int) will be resolved due to __exact match__ between argument & parameter type.  

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#null)
 
# Function overriding. Default parameter value.
**complexity:** professional
```cpp
struct base
{
   virtual void print(const char* colour = "red") const 
      { cout << "base::" << colour << endl; }
   virtual ~base() {}
};

struct derived : base
{
   void print(const char* colour = "green") const override 
      { cout << "derived::" << colour << endl; }
};

int main()
{
   base* obj = new derived;
   obj->print();
   delete obj;
}

```
Regarding code above what should be present in output?
- A. base::red
- B. derived::green
- C. base::green
- D. derived::red
- F. compiler error

**Answer:** D

`Static type` of 'obj' is base* but its `dynamic type` is derived*. Missed argument in the point of call is substituted by the compiler in compile-time based on static type information. Function 'print' is virtual (polymorphic) i.e. a decision which of them shoul be called will be resolved at run-time based on dymamic type information.   

**See also:** [S. Meyers. Effective C++, item 37](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT208&dq=Effective+C%2B%2B+Never+redefine+a+function’s+inherited+default+parameter+value&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)
 
# Object construction. Member initialization list.
**complexity:** basic
```cpp
struct part
{
   part()                        { cout << "part::default ctor" << endl; } 
   part(const char*)             { cout << "part::ctor(arg)"   << endl; } 
   part(const part&)             { cout << "part::copy ctor"   << endl; } 
   part& operator=(const part&)  { cout << "part::assignment operator" << endl; return *this; }
};

struct unit
{
   unit(const char* arg) { part_ = arg; }  // [2]
// unit(const char* arg) : part_(arg) {}   // [1]
private:
   part part_;
};

int main()
{
   unit obj;
}
```
Regarding code above what should be present in output? Which of proposed constructors above (`[1]`,`[2]`) is more effective? Why?
- A. 
    - part::default ctor
    - part::copy ctor
    - part::assignment operator
- B. 
    - part::default ctor
    - part::assignment operator
- C. 
    - part::default ctor
    - part::ctor(arg)
    - part::assignment operator
- D. 
    - part::ctor(arg)

**Answer:** C

**See also:** [S. Meyers. Effective C++, item 4](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT56&dq=A+better+way+to+write+the+ABEntry+constructor+is+to+use+the+member+initialization+list+instead+of+assignments:&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)

**Relatives:** [ctor::member_ordering](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction-member-initialization-list-vs-member-declaration-ordering)

# Inheritance. Hiding names.
**complexity:** professional
```cpp
struct base
{
   void foo(double) const { cout << "base::foo(double)" << endl; }  
};

struct derived : base
{
   void foo(const char*) const    { cout << "derived::foo(const char*)" << endl; }  
};

int main()
{
   derived d;
   d.foo(3.14);
}
```
Regarding code above what should be present in output?
- A. base::foo(double)
- B. derived::foo(const char*)
- C. compiler error: 'derived::foo(const char*)': cannot convert argument 1 from 'double' to 'const char*'

**Answer:** C

**See also:** [S. Meyers. Effective C++, item 33](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT184&dq=Avoid+hiding+inherited+names.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)

# Inheritance. Virtual base.
**complexity:** expert
```cpp
class base
{
};

class derived1 : public base
{
};

class derived2 : virtual public base
{
};

int main()
{
   cout << sizeof(derived2) - sizeof(derived1) << endl;
}
```
Regarding code above what should be present in output?
- A. 0
- B. but definitely greater than 0 (compiler dependent)
- C. but definitely greater than 0 (platform dependent)

**Answer:** B

The Standatd does not specify size of `pointer` to a virtual table (vtbl).

**See also:** [S. Meyers. Effective C++, item 40](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT220&dq=Item+40:+Use+multiple+inheritance+judiciously.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false), [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#layout-obj)


# Object construction. Virtual Inheritance
**complexity:** expert
```cpp
struct base1
{
   base1() { cout << "base1" << endl; }
};

struct base2
{
   base2() { cout << "base2" << endl; }
};

struct derived : base1, virtual base2 
{
   derived() { cout << "derived" << endl; }
};

int main()
{
   derived d;
}
```
Regarding code above what should be present in output?
- A. 
    - base1
    - base2
    - derived
- B. 
    - base2
    - base1
    - derived
- C. 
    - derived
    - base1
    - base2

**Answer:** B 

Subobject of virtually inheritable class is **always** Initialized first regardless of the its location in the inheritance list.

**See also:** [S. Meyers. Effective C++, item 40](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT220&dq=Item+40:+Use+multiple+inheritance+judiciously.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)


# Object construction. Member initialization list vs. member declaration ordering
**complexity:** professional
```cpp
class range
{
   size_t  distance_;
   size_t  begin_;
   size_t  end_;

public:
    range(size_t x1, size_t x2)
      : begin_(x1), end_(x2), distance_(end_ - begin_) 
   {}
   size_t distance() const { return distance_; }
};

void main()
{
   range r(1,9);
   cout << r.distance() << endl;
}
```
Regarding code above what should be present in output?
- A. 1
- B. 9
- C. 0
- D. \<undefined\>
- F. 8

**Answer:** D (C) 

Data members are initialized __in the order in which they are declared__. This is true
even if they are listed in a different order on the member initialization
list. __D__ is the right answer in common case. For the the given test, __B__ is also correct, because of members `begin_`, `end_` are of build-in type which are initialized with 0 (zero) before `range::range()` invokation.

**See also:** [S. Meyers. Effective C++, item 4](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT56&dq=A+better+way+to+write+the+ABEntry+constructor+is+to+use+the+member+initialization+list+instead+of+assignments:&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)

**Relatives:** [ctor::member_initialization_list](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction-member-initialization-list)

# Polymorphic objects. Slicing
**complexity:** basic
```cpp
struct base
{
    virtual void foo() const { cout << "base" << endl; }
};

struct derived : base
{
    void foo() const override { cout << "derived" << endl; }
};

void display(const base* obj)
{
    obj->foo();
}

void display(const base obj)
{
    obj.foo();
}

void main()
{
   const derived d;
   display(&d);
   display(d);
}
```
Regarding code above what should be present in output? 
Can a virtual method be virtual?
- A. 
    - derived
    - derived
- B. 
    - derived
    - base
- C. 
    - base
    - base
- D. 
    - base
    - derived

**Answer:** B 

If you have a class with virtual method then you should access it through object pointer or reference. Otherwise you will have type __slicing__ - a copying a portion (base part) of the whole object. If you use **_object itself_** syntax to invoke a virtual method then polymorphic sence is lost i.e. object's `dynamic type` is equal to its `static type`. Thus, for this particular case, a virtual method can be threated as inlined.

**See also:** [CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c145-access-polymorphic-objects-through-pointers-and-references)

**Relatives:** 

# Cost of the virtuality?
**complexity:** professional
```cpp
class point
{   // designed as a terminal class
    size_t x_;
    size_t y_;
public:
    point(size_t x, size_t y) : x_(x), y_(y) {}
    ~point() {}
};

class spot
{   // designed to be used as a base, OOP style
    size_t x_;
    size_t y_;
public:
    spot(size_t x, size_t y) : x_(x), y_(y) {}
    virtual ~spot() {}
};

void main()
{
   cout << sizeof(spot) - sizeof(point) << endl;
}
```
Regarding code above what should be present in output? 
- A. 0 
- B. sizeof(void*)
- C. compiler dependent
- D. platform depentedt

**Answer:** C 

The Standard does not say how the virtual table and the pointer to it should be implemented.

**See also:** [S. Meyers. Effective C++, item 7](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT72&dq=destructors+virtual+in+polymorphic+base+classes.%5C&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false), [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#layout-obj)

**Relatives:** [virtual destructor](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#virtual-destructor)

# Size Of C++ object (members only)?
**complexity:** basic
```cpp
struct point
{
    size_t x_;
    size_t y_;
};

class spot
{
    size_t x_;
    size_t y_;

public:
    spot(size_t x, size_t y) : x_(x), y_(y) {}

    // data accessors
    unsigned long   getX() const   { return x_; }
    void            setX(size_t x) { x_=x; }
    unsigned long   getY() const   { return y_; }
    void            setY(size_t y) { y_=y; }
};

void main()
{
   cout << sizeof(spot) - sizeof(point) << endl;
}
```
Regarding code above what should be present in output? 
- A.  0 
- B. <0
- C. >0
- D. compiler dependent.

**Answer:**  

Function-member does not impact on the object size.

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#layout-obj)

**Relatives:** 

# Object construction. Exception
**complexity:** basic
```cpp
struct part
{
   part()   { cout << "part::ctor" << endl; } 
   ~part()  { cout << "part::dtor" << endl; }
};

class unit
{
   part part_;
public:
   unit()
   { 
      cout << "unit::ctor" << endl;       
      throw std::logic_error("something wrong");
   }
   ~unit() { cout << "unit::dtor" << endl; }
};

int main()
{
   try {
   unit obj;
   } 
   catch(const std::exception& e)
   {
      cout << e.what() << endl;
   }
}
```
Regarding code above what should be present in output? 
- A.  
    - part::ctor
    - unit::ctor
    - unit::dtor
    - part::dtor
    - something wrong
- B. 
    - part::ctor
    - unit::ctor
    - something wrong
- C. 
    - part::ctor
    - unit::ctor
    - part::dtor
    - something wrong
- D. 
    - part::ctor
    - unit::ctor
    - something wrong
    - unit::dtor
    - part::dtor

**Answer:** C

In C++ the lifetime of an object is said to begin when the constructor runs to completion. And it ends right when the destructor is called. If the ctor throws, then the dtor is not called. 

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#ctor-exceptions)

**Relatives:** [ctor::member](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction) 


# Deducing Types. Template parameter.
**complexity:** basic
```cpp
template<typename T>
void f(T param)
{
   cout << "param type: " << typeid(param).name() << endl;
}

const char* const ptr =  "Fun with pointers";

int main()
{
   f(ptr);
}
```
Regarding code above what should be present in output? 
- A. const char* const  
- B. const char*
- C. char* const
- D. char*

**Answer:** B

Qualifiers `const` and `volatile` are ignored only for _by-value_ parameters.
For parameters that are _references-to-_ or _pointers-to-const_, the constness of _expr_ (input argument) is preserved during type deduction.
In our case, _expr_ (ptr) is a const pointer to a const object, and _expr_ is passed to a _by-value_ param.
Thus, the constness of 'ptr' will be ignored, but the constness of what 'ptr' points to is preserved. 

**See also:** [S. Meyers. Effective Modern C++, item 1.1](https://www.safaribooksonline.com/library/view/effective-modern-c/9781491908419/ch01.html)

**Relatives:** 

# 'C' array as function parameter. Decay of types.
**complexity:** professional
```cpp
void foo(int arg[3])
{
   arg[0] = 100;
   arg[1] = 200;
   arg[2] = 300;
}

int main()
{
   int arr[] = {1,2,3};
   foo(arr);
   cout << "arr[3]: " << arr[0] << ", " << arr[1] << ", "<< arr[2] << endl;
}
```
Regarding code above what should be present in output? 
- A. arr[3]: 1, 2, 3 
- B. arr[3]: 100, 200, 300
- C. compiler error. 'foo(int [])' parameter 'arg' passed by-value cannot be copied.

**Answer:** B

It's an illusion that 'arg' passed _by-value_. In this context, an array __decays__ into a pointer to its first element (C language legacy).
A legal syntax of function declaration `void foo(int arg[3])` is equivalent to `void foo(int* arg)`.
Thus, an array argument is passed into 'foo' as a pointer, not by-value i.e. there is no copying.
Moreover, 'C' array is not copyable.

**See also:** [S. Meyers. Effective Modern C++, item 1.1](https://www.safaribooksonline.com/library/view/effective-modern-c/9781491908419/ch01.html)

**Relatives:** 

# Object construction. Virtual call
**complexity:** basic
```cpp
struct base
{
   string name_;

   virtual string name() const { return "base"; }
   base() { name_ = name(); }
   virtual ~base() {}
};

struct derived : base
{
   string name_;

   virtual string name() const override { return "derived"; }
   derived() { name_ = name(); }
};

int main()
{
   derived d;
   cout << d.base::name_ << endl
        << d.name_       << endl;
}
```
Regarding code above what should be present in output? 
- A. 
    - base
    - derived
- B. 
    - base
    - base
- C. 
    - derived
    - derived
 
**Answer:** A 

In a constructor, the virtual call mechanism __is disabled__ because overriding from derived classes hasn't yet happened. Objects are constructed from the base up, "base before derived".

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#vcall)

**Relatives:** [ctor::basic](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction)


# Deducing Types. 'auto' by-value.
**complexity:** basic
```cpp
int main()
{
	const int  theNumber {44};
	
	auto   x  = theNumber;
	auto   y  = &theNumber;

   cout << typeid(x).name() << " - " << typeid(y).name() << endl;
}
```
Regarding code above what should be present in output? 
- A. const int - const int *
- B. int - const int *
- C. int - int *
- D. const int - int *

**Answer:** B

`auto` type deduction is usually the same as template type deduction. types 'x' and 'y' are deduced based on passing _left-hand expression_ `by-value`.
That means: `const` and/or `volatile` _expression_ are treated as non-`const` and non-`volatile`.
In our case, type of _expression_ for 'x' is const int; type of _expression_ for 'y' is a __pointer to__ const int and this pointer is passed `by-value`.

**See also:** [S. Meyers. Effective Modern C++, item 1.1](https://www.safaribooksonline.com/library/view/effective-modern-c/9781491908419/ch01.html)

**Relatives:** [template_param::by_value](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#deducing-types-template-parameter)

# Deducing Types. 'auto' by-reference.
**complexity:** professional
```cpp
int main()
{
	const char name[] = "R. N. Briggs";
	
	auto   x  = name;
	auto&  y  = name;

   cout << typeid(x).name() << " - " << typeid(y).name() << endl;
}
```
Regarding code above what should be present in output? 
- A. const char* - const char*
- B. const char* - const char (&)[13]
- C. const char [13] - const char(&) [13]
- D. const char [13] - const char* 

**Answer:** B

Type of 'x' is deduced `by-value` rule. The expression on the right side of `operator=` is pointer to const char (see [type decaying](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#c-array-as-function-parameter-decay-of-types)).
Type of 'y' is deduced `by-reference` rule, i.e. it is the actual type of the array.

**See also:** [S. Meyers. Effective Modern C++, item 2](https://www.safaribooksonline.com/library/view/effective-modern-c/9781491908419/ch01.html)

**Relatives:** [C array vs pointer](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#c-array-as-function-parameter-decay-of-types), [deducing_type::by-value](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#deducing-types-auto-by-value)
