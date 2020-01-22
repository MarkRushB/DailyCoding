# INFO6205-Program Structure & Algorithms
## Introduction
- **Professor**: Robin  
- **Grading**: Your grade for the course will be based on the following components (see Blackboard for details—please note that this may very slightly)
  1. Programming assignments (17%)
  2. In class quizzes—typically executed in HackerRank (18%)
  3. Mid term exam (17%)
  4. Term project (usually in teams of two) (22%)
  5. Final exam (22%)
  6. Class participation via TopHat (4%)
- **Related Reference**:
  - [Self-Study](DataStructuresandAlgorithms.md)
## 1st Week
### Thinking Question by Robin
- Something for you to think about between now and the next lecture. Do you recall that I talked about how the purpose of an algorithm to solve a "search problem" is to eliminate the original entropy? And, then, I said that the original entropy for sorting (a particular type of search problem) is the logarithm of N!, right? In particular, we say that it is lg(N!) or log to the base 2 of N!. By the way, we will later on actually evaluate this expression in a simpler form. Meanwhile, any guesses?
- OK, now I want you to think about a search problem in which you actually search for a particular element in a list. What's the original entropy for this problem? Remember, to calculated the entropy, we take the logarithm of the total number of possibilities. What's that in this case? Answers welcome. (edited)  
### content
- what did we do in the sorting?
  - compare & swap
- Logarithm notation
  - [notation](DataStructuresandAlgorithms.md)
  - ln(x) is the natural logarithm of x, i.e. loge(x). 
  - lg(x) is the binary logarithm of x, i.e. log2(x).
  - log(x) is simply used when we don’t really care to distinguish—remember that the only difference between logarithms with different bases is a constant factor. We typically ignore constant factors when we study complexity.
  - We will never ever use logarithms to the base 10.
## 2nd Week
- hashCode, compareTo,and equals
  - Object methods
    - Everyobject in Java must implement various methods, including:
      - public booleanequals(Object x)
      - public int hashCode()
    - Instances of class Ximplementing Comparable < X > must implement:
      - public int compareTo(X x)
    - **Thus all objects implement equalsand hashCodebut only objects that have an intrinsic ordering implement compareTo.**
  - If we want to know if this and that are the sameobject, we can use == which tests that the references are the same
  - Implementing equals
    - The signature of equalsin Java is:
      - **boolean equals (Object x)**
    - When implementing equals, we need to check for the equality of each field which forms part of the “primary key” of an object. If any pair of fields is unequal, then the objects are unequal.
    - Before we can compare the fields, we must establish that both objects have the same class otherwise it makes no sense to talk about comparing fields.
    - And before doing that we might as well check a couple of other things that can give us an immediate result.
  - Hashcode
    - A hash code is a 32-bit digestof an object.
    - It is required, by API “contract”,that hashCodebe consistent with equalssuch that:
      - if a.equals(b) then a.hashCode==b.hashCode
      - It also follows that: if a.hashCode!= b.hashCode then !a.equals(b)
    - ```java
        //Example: java.lang.String:
        public inthashCode() {
          int h = hash; // cached value: defaults to 0
          if (h == 0 && value.length> 0) {
              char val[] = value;
              for (inti= 0; i< value.length; i++) {
                  h = 31 * h + val[i];
                }
                hash = h;
            }
            return h;
        }
      ```
- what is data type
  - Data types
    - A data type is a set of values and a set of operations on those values.
  - Abstract data types
    - An abstract data type is a data type whose internal representation is hidden from the client (encapsulation).
  - Objects
    - An object is an entity that can take on a data-type value. Objects are characterized by three essential properties: The state of an object is a value from its data type; the identity of an object distinguishes one object from another; the behavior of an object is the effect of data-type operations. In Java, a reference is a mechanism for accessing an object.
- Application Programming Interface(API)
  - We briefly referred to an API earlier, without defining it. It stands for Application Programming Interface and forms the contractbetween the programmer of a class and its clients.
  - To specify the behavior of an abstract data type, we use an application programming interface (API), which is a list of constructors, instance and class methods, and public fields (instance/class)—if any—with an informal description of the effect of each.
    - **Client**: A client is a program that uses a data type.
    - **Implementation**: An implementation is the code that implements the data type specified in an API.
- Implementing ADTs
  - Encasulation
  - Designnig APIs
  - Interface inheritance
    - Java provides language support for defining relationships among objects, known as inheritance. Interfaces are the answer to too much coupling. We use interface inheritance for comparisonand for iteration.
  - Inheritance via sub-classing
    - Any (non-final) class can be sub-classed. However, people are not that good at recognizing such situations. In Java, every class is a sub-class of **Object** which defines several important methods, including **toString, equals and hashCode**.
    - `interface List`
    - `Class ArrayList implement List`
  - Coupling(耦合)
    - Well-architected software systems have low coupling (and high cohesion—where related concepts are in the same module)
    - If every little change you make to a software module ripples through many other modules: that’s tightly coupled (bad).
    - Encapsulation also helps to avoid coupling by hiding implementation details.
    - Interfaces are the chief way to reduce coupling.

## 3rd Week
  - Bag, Stack, Queue
  - Double LinkedList
