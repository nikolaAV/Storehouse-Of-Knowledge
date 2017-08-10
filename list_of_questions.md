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

**Answer:** B

При инстанцировании экземпляра любого типа (в том числе и "пустого"), под него должна быть выделена обязательно физическая память, поскольку к каждому объекту в С++ может быть применен  `operator &`  (получения адреса). Следуя Стандарту под «пустой» объект выделяется минимально возможный объем памяти – 1 байт.

**See also:** [Why is the size of an empty class not zero?](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 


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

**See also:** 




