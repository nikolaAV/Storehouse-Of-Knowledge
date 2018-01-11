[What goes wrong with software?](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf)

## Principles
### Class Design
__SOLID__ is the acronym for a [set of practices](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod) [(in Russian)](http://sergeyteplyakov.blogspot.com/search/label/SOLID) that, when implemented
together, make code adaptive to change. The SOLID
practices were introduced by [Bob Martin](https://en.wikipedia.org/wiki/Robert_C._Martin) almost 15 years ago.
Even so, these practices are not as widely known as they could
be — and perhaps should be.

acronym | name | description
-------------|---------------| ------------
SRP | [The Single Responsibility Principle](https://drive.google.com/file/d/0ByOwmqah_nuGNHEtcU5OekdDMkk/view), [[ru]](http://sergeyteplyakov.blogspot.com/2014/08/single-responsibility-principle.html) | A class should have one, and only one, reason to change.
OCP	| [The Open Closed Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgN2M5MTkwM2EtNWFkZC00ZTI3LWFjZTUtNTFhZGZiYmUzODc1/view), [[ru]](http://sergeyteplyakov.blogspot.ca/2014/08/open-closed-principle.html)	| You should be able to extend a classes behavior, without modifying it.
LSP	| [The Liskov Substitution Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgNzAzZjA5ZmItNjU3NS00MzQ5LTkwYjMtMDJhNDU5ZTM0MTlh/view), [[ru]](http://sergeyteplyakov.blogspot.ca/2014/09/liskov-substitution-principle.html)	| Derived classes must be substitutable for their base classes.
ISP	| [The Interface Segregation Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgOTViYjJhYzMtMzYxMC00MzFjLWJjMzYtOGJiMDc5N2JkYmJi/view), [[ru]](http://sergeyteplyakov.blogspot.com/2014/08/interface-segregation-principle.html) |	Make fine grained interfaces that are client specific.
DIP	| [The Dependency Inversion Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgMjdlMWIzNGUtZTQ0NC00ZjQ5LTkwYzQtZjRhMDRlNTQ3ZGMz/view), [[ru]](http://sergeyteplyakov.blogspot.ca/2013/04/blog-post.html) |	Depend on abstractions, not on concretions.

### package cohesion
acronym | name | description
-------------|---------------| ------------
REP	| [The Release Reuse Equivalency Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgOGM2ZGFhNmYtNmE4ZS00OGY5LWFkZTYtMjE0ZGNjODQ0MjEx/view) |	The granule of reuse is the granule of release.
CCP	| [The Common Closure Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgOGM2ZGFhNmYtNmE4ZS00OGY5LWFkZTYtMjE0ZGNjODQ0MjEx/view) |	Classes that change together are packaged together.
CRP	| [The Common Reuse Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgOGM2ZGFhNmYtNmE4ZS00OGY5LWFkZTYtMjE0ZGNjODQ0MjEx/view) |	Classes that are used together are packaged together.

### couplings between packages
acronym | name | description
-------------|---------------| ------------
ADP	| [The Acyclic Dependencies Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgOGM2ZGFhNmYtNmE4ZS00OGY5LWFkZTYtMjE0ZGNjODQ0MjEx/view) |	The dependency graph of packages must have no cycles.
SDP	| [The Stable Dependencies Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgZjI3OTU4ZTAtYmM4Mi00MWMyLTgxN2YtMzk5YTY1NTViNTBh/view) |	Depend in the direction of stability.
SAP	| [The Stable Abstractions Principle](https://drive.google.com/file/d/0BwhCYaYDn8EgZjI3OTU4ZTAtYmM4Mi00MWMyLTgxN2YtMzk5YTY1NTViNTBh/view) |	Abstractness increases with stability.

## [Patterns](https://en.wikipedia.org/wiki/Software_design_pattern)
* [Design Patterns, GoF](https://en.wikipedia.org/wiki/Design_Patterns) a classical book, collectively known as the "Gang of Four", or GoF for short. 
* [GRASP](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design))  are really a mental toolset, a learning aid to help in the design of object-oriented software. __G__ eneral __R__ esponsibility __A__ ssignment __S__ oftware __P__ atterns (or principles) were introduced by [Craig Larman](https://en.wikipedia.org/wiki/Craig_Larman) in his [book](https://www.utdallas.edu/~chung/SP/applying-uml-and-patterns.pdf).  
* [P of EAA](https://martinfowler.com/eaaCatalog/) by [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler).

## Idioms
* [More C++ Idioms - Wikibooks](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms)