## Static Polymorphism Using CRTP

Static polymorphism is a type of polymorphism that is resolved at 
compile time. It is primarily achieved through templates and CRTP 
(Curiously Recurring Template Pattern) in C++. In contrast to 
dynamic polymorphism (which uses virtual functions and 
inheritance), static polymorphism doesn’t rely on runtime checks 
like vtables but instead utilizes compile-time mechanisms.

```cpp
#include <iostream>

// Base class template that uses CRTP
template <typename Derived>
class Shape {
public:
    // Static polymorphism: Calls the derived class's area() method at compile-time
    void draw() const {
        // Calling the derived class's area() method
        static_cast<const Derived*>(this)->draw();
    }

    // Common interface for all derived classes
    double getArea() const {
        // Calling the derived class's area() method using CRTP
        return static_cast<const Derived*>(this)->area();
    }
};

// Derived class for Circle
class Circle : public Shape<Circle> {
public:
    Circle(double radius) : radius_(radius) {}

    // Method specific to Circle
    void draw() const {
        std::cout << "Drawing Circle with radius: " << radius_ << std::endl;
    }

    // Implementation of area() for Circle
    double area() const {
        return 3.14159 * radius_ * radius_;
    }

private:
    double radius_;
};

// Derived class for Square
class Square : public Shape<Square> {
public:
    Square(double side) : side_(side) {}

    // Method specific to Square
    void draw() const {
        std::cout << "Drawing Square with side: " << side_ << std::endl;
    }

    // Implementation of area() for Square
    double area() const {
        return side_ * side_;
    }

private:
    double side_;
};

int main() {
    Circle circle(5.0);
    Square square(4.0);

    Shape<Circle>* s1 = &circle;
    Shape<Square>* s2 = &square;

    // Static polymorphism: Compile-time resolution
    s1->draw();  // Calls Circle's draw()
    s2->draw();  // Calls Square's draw()

    std::cout << "Circle Area: " << s1->getArea() << std::endl;
    std::cout << "Square Area: " << s2->getArea() << std::endl;

    return 0;
}
```
> The Shape class in this static polymorphism example acts as an 
interface-like structure, but at compile-time. Unlike traditional 
dynamic polymorphism (with a virtual base class), Shape is not used 
for runtime polymorphism or to manage different types via base 
class pointers. Instead, it ensures that each derived class (like 
Circle or Square) implements certain methods such as draw() and area().

> It provides a common template for other shapes (like Circle or Square) to 
follow, forcing them to implement specific methods (draw() and area()).



