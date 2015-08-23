## Open-Closed Principle
**Creds to Samuel Oloruntoba(@kayandrae07) from Scotch IO**
> Objects or entities should be open for extension, but closed for modification.
This simply means that a class should be easily extendable without modifying the class itself. Let’s take a look at the AreaCalculator class, especially it’s sum method.

```PHP
public function sum() {
    foreach($this->shapes as $shape) {
        if(is_a($shape, 'Square')) {
            $area[] = pow($shape->length, 2);
        } else if(is_a($shape, 'Circle')) {
            $area[] = pi() * pow($shape->radius, 2);
        }
    }

    return array_sum($area);
}   
```

If we wanted the sum method to be able to sum the areas of more shapes, we would have to add more if/else blocks and that goes against the Open-closed principle.

A way we can make this sum method better is to remove the logic to calculate the area of each shape out of the sum method and attach it to the shape’s class.

```PHP
class Square {
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }

    public function area() {
        return pow($this->length, 2);
    }
}   
```

The same thing should be done for the Circle class, an area method should be added. Now, to calculate the sum of any shape provided should be as simple as:

```PHP
public function sum() {
    foreach($this->shapes as $shape) {
        $area[] = $shape->area;
    }

    return array_sum($area);
}
```

Now we can create another shape class and pass it in when calculating the sum without breaking our code. However, now another problem arises, how do we know that the object passed into the AreaCalculator is actually a shape or if the shape has a method named area?

Coding to an interface is an integral part of S.O.L.I.D, a quick example is we create an interface, that every shape implements:

```PHP
interface ShapeInterface {
    public function area();
}

class Circle implements ShapeInterface {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function area() {
        return pi() * pow($this->radius, 2);
    }
}  
```

In our AreaCalculator sum method we can check if the shapes provided are actually instances of the ShapeInterface, otherwise we throw an exception:

```PHP
public function sum() {
    foreach($this->shapes as $shape) {
        if(is_a($shape, 'ShapeInterface')) {
            $area[] = $shape->area();
            continue;
        }

        throw new AreaCalculatorInvalidShapeException;
    }

    return array_sum($area);
}    
```