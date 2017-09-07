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
 
**Relatives:**  [function overloading, nullptr](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#function-overloading-parameter-type-null-vs-nullptr)

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

In C++, __there is no overloading across scopes__ - derived class scopes are not an exception to this general rule
The compiler looks into the scope of _'derived'_, finds the single function "foo(const char*)" and calls it. It never bothers with the (enclosing) scope of _'base'_.

**See also:** [S. Meyers. Effective C++, item 33](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT184&dq=Avoid+hiding+inherited+names.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false), [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#overloadderived)

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

**See also:** [S. Meyers. Effective C++, item 40](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT220&dq=Item+40:+Use+multiple+inheritance+judiciously.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false), [isocpp.org,FAQ](https://isocpp.org/wiki/faq/multiple-inheritance#mi-vi-ctor-order)


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

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#ctor-exceptions), [Herb Sutter’s Mill](https://herbsutter.com/2008/07/25/constructor-exceptions-in-c-c-and-java/)

**Relatives:** [ctor::member](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction), [delegating ctor](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction-exception-in-delegating-constructor)


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

**See also:** [Bjarne Stroustrup's FAQ](http://www.stroustrup.com/bs_faq2.html#vcall), [C++ Coding Standards, rule 49](https://books.google.com.ua/books?id=mmjVIC6WolgC&pg=PT238&lpg=PT238&dq=C%2B%2B+Sutter+destructor+direct+call&source=bl&ots=ceOoHQiMZ4&sig=s0DhJh0lBmNK8CqDNJUVgUe3bBg&hl=en&sa=X&ved=0ahUKEwiO6IDA9-zVAhUosFQKHdIABZMQ6AEISjAG#v=onepage&q&f=false)

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

# new/delete vs. new[]/delete[].
**complexity:** basic
```cpp
struct widget
{
   widget()  { cout << "ctor" << endl; }
   ~widget() { cout << "dtor" << endl; }
};

int main()
{
   auto p = new widget[2];
   delete p;
}
```
Regarding code above what should be present in output? 
- A. 
    - ctor
    - ctor
    - dtor
    - dtor
- B. 
    - ctor
    - ctor
    - dtor
- C. run-time error 

**Answer:** C (B)

According to the Standatd, the program's behaviour is undefined.
At the very least, only one of 2 (N, the arbitrary number of  array dimension) widgets pointed to by 'p' will be properly destroyed, because destructors for rest of them will never be called.

**See also:** [S. Meyers. Effective C++, item 16](https://books.google.com.ua/books?id=U7lTySXdFk0C&pg=PT101&dq=Use+the+same+form+in+corresponding+uses+of+new+and+delete.&hl=en&sa=X&redir_esc=y#v=onepage&q&f=false)

**Relatives:** 

# Object lifetime. Destructor direct call. 
**complexity:** basic
```cpp
struct widget
{
   ~widget() { cout << "dtor" << endl; }
};

int main()
{
   widget w;
   w.~widget(); // line:A
}
```
Regarding code above what should be present in output? 
- A. 
    - dtor
- B. 
    - dtor
    - dtor
- C. run-time error: undefined behaviour
- D. compile-time error: incorrect syntax at _//line:A_  

**Answer:** B 

Statement _w.~widget()_ is legal code, dcor likes a normal member-function. But it's a [bad habit](http://www.gotw.ca/gotw/022.htm) to get into.

**See also:** [H. Sutter. GotW #22](http://www.gotw.ca/gotw/022.htm)

**Relatives:** 

# Object lifetime. const-reference to the temporary. 
**complexity:** professional
```cpp
string foo() { return "abc"; }

int main()
{
   const string& s = foo();   // line A
   cout << s << endl;
}
```
Regarding code above what should be present in output? 
- A. abc
- B. compiler error: [line A] reference cannot be bound to an lvalue

**Answer:**  A

Function 'foo()' returns a temporary object of `string` type. Normally, a temporary object lasts only until the end of the full expression in which it appears. However, C++ deliberately specifies that binding a temporary object to a reference to const on the stack lengthens the lifetime of the temporary to the lifetime of the reference itself, and thus avoids what would otherwise be a common dangling-reference error.
In the example above, the temporary returned by 'foo()' lives until the closing curly brace of `main`.

**See also:** [H. Sutter. GotW #88](https://herbsutter.com/2008/01/01/gotw-88-a-candidate-for-the-most-important-const/)

**Relatives:** 

# Object lifetime. const-reference to the base. 
**complexity:** expert
```cpp
struct base
{
   ~base() { cout << "~base()" << endl; }
};

struct derived : base
{
   ~derived() { cout << "~derived()" << endl; }
};

derived make_instance()
{
   return derived{};
}

int main()
{
   {
      const base& b = make_instance(); // line A
      cout << "end of the local scope" << endl; 
   }
}
```
Regarding code above what should be present in output? 
- A. compiler error: [line A] reference cannot be bound to an lvalue
- B. 
    - ~deriver
    - ~base
    - end of the local scope
- C
    - end of the local scope
    - ~deriver
    - ~base
- D
    - end of the local scope
    - ~base

**Answer:** C

If we have `const` 'base' reference to 'derived' temporary, it will be destroyed _without virtual dispatch on the destructor call_. 

**See also:** [H. Sutter. GotW #88](https://herbsutter.com/2008/01/01/gotw-88-a-candidate-for-the-most-important-const/)

**Relatives:** [const-reference to the temporary](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-lifetime-const-reference-to-the-temporary)

# std::shared_ptr. non-virtual destructor. 
**complexity:** expert
```cpp
struct base_virtual
{
   virtual ~base_virtual() {}
};

struct base_nonvirtual
{
   ~base_nonvirtual() {}
};

struct derived1 : base_virtual
{
   ~derived1() { cout << "~derived1" << endl; }
};

struct derived2 : base_nonvirtual
{
   ~derived2() { cout << "~derived2" << endl; }
};

int main()
{
   shared_ptr<base_virtual>      d1 { new derived1 {} };
   shared_ptr<base_nonvirtual>   d2 { new derived2 {} };
}
```
Regarding code above what should be present in output? 
- A.
    - ~derived2
    - ~derived1
- B. 
    - ~derived1
- C
    - ~derived1
    - ~derived2

**Answer:** A

Class template `shared_ptr<T>` has function template (constructor) `shared_ptr(Y*)`, where Y* must be convertible to T*.
This constructor with _Y=derived_ creates the appropriate deleter object based on Y type, not T type.
This deleter is called when the shared_ptr object is about to free the pointed resource.

**See also:** [stackoverflow,shared_ptr magic](https://stackoverflow.com/questions/3899790/shared-ptr-magic), [cppreference,shared_ptr ctor3](http://en.cppreference.com/w/cpp/memory/shared_ptr/shared_ptr)

**Relatives:** 

# Type conversion. const-reference to the temporary. 
**complexity:** professional
```cpp
void display(string& s)
{
   cout << "[string&]-> " << s << endl;    
}

void display(const string& s)
{
   cout << "[const string&]-> " << s << endl;    
}

int main()
{
   display("hello, world!");
}
```
Regarding code above what should be present in output? 
- A. [string&]-> hello, world!
- B. [const string&]-> hello, world!
- C. compiler error: 'display' ambiguous call to overloaded function
- D. compiler error: 'display' cannot convert parameter 1 from char* to string&

**Answer:** B 

Function 'display' parameter type (`string&`) and type (`const char*`) of the argument passed in it is mismatch.
This call can succeed only if the type mismatch can be eliminated, the compiler will be happy to eliminate it by creating a temporary object of type `string`.
The 's' parameter of 'display' is then bound to this temporary `string` object.
But binding a temporary object to reference is only possible if this reference itself is `const`.  
When 'display' returns, the temporary object is automatically destroyed. 

**See also:** [S. Meyers. Effective C++, item 5,19](http://www.physics.rutgers.edu/~wksiu/C++/MoreEC++_only.pdf), [H. Sutter. GotW #88](https://herbsutter.com/2008/01/01/gotw-88-a-candidate-for-the-most-important-const/) 

**Relatives:** [object lifetime referenced by const](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-lifetime-const-reference-to-the-temporary)

# Function overriding. member template & virtuality. 
**complexity:** expert
```cpp
struct base
{
   template <typename T>
   virtual void foo(const T& a) const { cout << "base::foo -> " << a << endl; }
   virtual ~base() {}
};

struct derived : base
{
   template <typename T>
   void foo(const T& a) const override { cout << "derived::foo -> " << a << endl; }
};

int main()
{
   unique_ptr<base> b { new derived{} };
   b->foo("hello,world!");    
}
```
Regarding code above what should be present in output? 
- A. base::foo -> hello,world! 
- B. derived::foo -> hello,world!
- C. compiler error: 'void base::foo(const T &) const': member function templates cannot be virtual

**Answer:** C  

> This _must_ be illegal, otherwise we would have to add another entry to the virtual table
for class 'base' each time someone called base::foo() with a new
argument type. This would imply that __only the linker__ could make virtual function
tables and assign table positions to functions. Consequently, a member template cannot
be `virtual`. — Bjarne Stroustrup, [D&E Of C++, ch 15.9.3](http://doc.imzlp.me/viewer.html?file=docs/cpp/TheDesignAndEvolutionOfCpp.pdf)

**See also:** [StackOverflow](https://stackoverflow.com/questions/2354210/can-a-c-class-member-function-template-be-virtual) 

**Relatives:** 

# Function overloading. Parameter type: NULL vs. nullptr
**complexity:** basic
```cpp
void foo(char*) {cout << "pointer argument" << endl; }
void foo(int)   {cout << "integer argument" << endl; }

int main()
{
   foo(nullptr);
}
```
Regarding code above what should be present in output?
- A. pointer argument
- B. integer argument
- C. compiler error: ambiguous call to overloaded function

**Answer:** A

`nullptr` (C++11 keyword) is a literal denoting the null pointer; it is not an integer.

**See also:** [Bjarne Stroustrup's C++11 FAQ](http://www.stroustrup.com/C++11FAQ.html#nullptr), [S. Meyers. Effective Modern C++, item 8](https://books.google.com.ua/books?id=rjhIBQAAQBAJ&lpg=PP1&dq=meyers%20modern%20c%2B%2B%20NULL%20nullptr&pg=PA58#v=onepage&q&f=false)

**Relatives:**  [function overloading, NULL](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#function-overloading-parameter-type-integer-vs-pointer)

# Inheritance. Virtual base & dominance.
**complexity:** expert
```cpp
struct grandparent
{
   virtual void foo() { cout << "grandparent::foo" << endl; }
};

struct mother : virtual grandparent
{
};

struct father : virtual grandparent
{
   void foo() override { cout << "father::foo" << endl; }
};

struct child : mother, father
{
   child() { foo(); } // Line A
};

int main()
{
   child c;
}
```
Regarding code above what should be present in output?
- A. grandparent::foo 
- B. father::foo
- C. compiler error: [Line A], 'foo()' - ambigious call

**Answer:** B

Above, the classic _'diamond inheritance'_ is shown. Whome kind of behaviour has the child inherited: either from the grandpa indirectly through mother's branch or from the father directly?
'father::foo' predominates because of an inheritance path for the child is twice as shorter (one degree) as path to grandpa (two degreees). 

**See also:** [wiki:dominance](https://en.wikipedia.org/wiki/Dominance_%28C%2B%2B%29#Example_with_virtual_inheritance), [stackoverflow:dominance](https://stackoverflow.com/questions/7210860/dominance-in-virtual-inheritance)

**Relatives:**

# Object construction. Exception in delegating constructor
**complexity:** professional
```cpp
struct unit
{
   unit(size_t n)  { cout << "unit::ctor (principal)" << endl; }
   unit() : unit(0) // Line A
   { 
      cout << "unit::ctor (delegating)" << endl;       
      throw std::logic_error("something wrong");
   }
   ~unit() { cout << "unit::dtor" << endl; }
};

int main()
{
   try
   {
      unit obj;
   }
   catch(const exception& e)
   {
      cout << e.what() << endl;
   }
}
```
Regarding code above what should be present in output? 
- A.  
    - unit::ctor (principal)
    - unit::ctor (delegating)
    - unit::dtor
    - something wrong
- B. 
    - unit::ctor (delegating)
    - unit::ctor (principal)
    - unit::dtor
    - something wrong
- C. 
    - unit::ctor (principal)
    - unit::ctor (delegating)
    - something wrong
- D. 
    - compiler error: // Line A, syntax error

**Answer:** A

- unit::unit(size_t n) is a _'principal constructor'_
- unit::unit() is a _delegating constructor_ which calls the principal one directly.
- unit::~unit(), upon scope unwinding, the destructors of fully-constructed object is called. 

An unit object's lifetime begins when some constructor (in our case _'principal constructor'_) has finished. (See C++ Standard, 15.2/2)

**See also:** [stackoverflow, delegating constructor throws](https://stackoverflow.com/questions/14386840/destructor-called-after-throwing-from-a-constructor)

**Relatives:** [ctor::exception](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#object-construction-exception) 

# Exception. Catch by value
**complexity:** professional
```cpp
struct my_exception
{
   my_exception()                   { cout << "exception::ctor(default)" << endl; }   
   my_exception(const my_exception&){ cout << "exception::ctor(copy)" << endl; }   
   my_exception(my_exception&&)     { cout << "exception::ctor(move)" << endl; }   
   my_exception& operator=(const my_exception&)
                                    { cout << "exception::assigment(copy)" << endl; }   
   my_exception& operator=(my_exception&&)
                                    { cout << "exception::assigment(move)" << endl; }   
};

int main()
{
   try
   {
      // ...
      my_exception e;
      throw e;
   }
   catch(my_exception e)
   {
      // e.what();
   }
}
```
Regarding code above what should be present in output? 
- A.  
    - exception::ctor(default)
    - exception::ctor(copy)
- B. 
    - exception::ctor(default)
    - exception::ctor(copy)
    - exception::ctor(copy)
- C. 
    - exception::ctor(default)
    - exception::ctor(move)
    - exception::ctor(copy)
- D. 
    - exception::ctor(default)
    - exception::assigment(copy)
    - exception::ctor(copy)
- E. 
    - exception::ctor(default)
    - exception::assigment(move)
    - exception::ctor(copy)


**Answer:** C

To avoid unnecessary copying of the exception object and object slicing, the best practice for catch clauses is to catch by reference

**See also:** [C++ Coding Standards, item 73](https://doc.lagout.org/programmation/C/CPP101.pdf), [CppCoreGuidelines, E.15](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#e15-catch-exceptions-from-a-hierarchy-by-reference)

**Relatives:** 

# Exception. Multiple inheritance
**complexity:** expert
```cpp
struct my_exc1    : std::exception
{ 
   char const* what() const { return "my_exc1"; } 
};

struct my_exc2    : std::exception 
{ 
   char const* what() const { return "my_exc2"; } 
};

struct your_exc3  : my_exc1, my_exc2 
{
   char const* what() const { return "your_exc3"; } 
};

int main()
{
   try { throw your_exc3{}; }
   catch(const std::exception& e) 
   {
      cout << e.what() << endl; 
   }
   catch(...) 
   { 
      cout << "whoops!" << endl; 
   }
}
```
Regarding code above what should be present in output? 
- A. my_exc1  
- B. my_exc2
- C. your_exc3
- D. whoops!
- E. compiler error: std::'exception' is ambiguous.

**Answer:** D

The program above prints "whoops" because the C++ runtime can't resolve which `exception` instance to match in the first _catch_ clause.

**See also:** [D. Abrahams, boost::exception_handling](http://www.boost.org/community/error_handling.html) 

**Relatives:** [Multiple Inheritance. Type conversion to base](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#type-conversion-base-in-multiple-inheritance)

# Type conversion. Base in multiple inheritance
**complexity:** basic
```cpp
struct grandparent
{
   virtual void foo() const { cout << "grandparent::foo" << endl; }
   virtual ~grandparent() {}
};

struct mother : grandparent
{
   void foo() const override { cout << "mother::foo" << endl; }
};

struct father : grandparent
{
   void foo() const override { cout << "father::foo" << endl; }
};

struct child : mother, father
{
   void foo() const override { cout << "child::foo" << endl; }
};

int main()
{
   grandparent* p = new child{}; 
   p->foo();
   delete p;
}
```
Regarding code above what should be present in output? 
- A. grandparent::foo 
- B. mother::foo
- C. father::foo
- D. child::foo
- E. compiler error: ambiguous conversions from 'child *' to 'grandparent *' 

**Answer:** E

The compiler does not know what kind of subobject of whole 'child' you want to point out. It can be subobject of 'grandparent' comes from either 'mother' branch or 'father' branch.

**See also:** [S.Meyers. Effective C++, item 40](https://books.google.com.ua/books?id=Qx5oyB49poYC&pg=PA192&dq=Use+multiple+inheritance+judiciously.&hl=en&sa=X&ved=0ahUKEwio6Ou_ypLWAhWIKGMKHVZMCGoQ6AEILjAB#v=onepage&q&f=false)

**Relatives:** [exception::multiple_inheritance](https://github.com/nikolaAV/Storehouse-Of-Knowledge/blob/master/list_of_questions.md#exception-multiple-inheritance)



