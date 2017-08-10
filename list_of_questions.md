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

При инстанцировании экземпляра любого типа (в том числе и "пустого"), под него должна быть выделена **обязательно** физическая память, поскольку к каждому объекту в С++ может быть применен `operator &` (получения адреса). Следуя Стандарту под «пустой» объект выделяется минимально возможный объем памяти – 1 байт.

**See also:** [Why is the size of an empty class not zero?](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 



