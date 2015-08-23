## Interface segregation principle
**Creds to Samuel Oloruntoba(@kayandrae07) from Scotch IO**

> A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.

Still using our shapes example, we know that we also have solid shapes, so since we would also want to calculate the volume of the shape, we can add another contract to the ShapeInterface:

```PHP
interface ShapeInterface {
    public function area();
    public function volume();
}  
```

Any shape we create must implement the volume method, but we know that squares are flat shapes and that they do not have volumes, so this interface would force the Square class to implement a method that it has no use of.

ISP says no to this, instead you could create another interface called SolidShapeInterface that has the volume contract and solid shapes like cubes e.t.c can implement this interface:

```PHP
interface ShapeInterface {
    public function area();
}

interface SolidShapeInterface {
    public function volume();
}

class Cuboid implements ShapeInterface, SolidShapeInterface {
    public function area() {
        // calculate the surface area of the cuboid
    }

    public function volume() {
        // calculate the volume of the cuboid
    }
}
```

This is a much better approach, but a pitfall to watch out for is when type-hinting these interfaces, instead of using a ShapeInterface or a SolidShapeInterface.

You can create another interface, maybe ManageShapeInterface, and implement it on both the flat and solid shapes, this way you can easily see that it has a single API for managing the shapes. For example:

```PHP
interface ManageShapeInterface {
    public function calculate();
}

class Square implements ShapeInterface, ManageShapeInterface {
    public function area() { /*Do stuff here*/ }

    public function calculate() {
        return $this->area();
    }
}

class Cuboid implements ShapeInterface, SolidShapeInterface, ManageShapeInterface {
    public function area() { /*Do stuff here*/ }
    public function volume() { /*Do stuff here*/ }

    public function calculate() {
        return $this->area() + $this->volume();
    }
}    
```

Now in AreaCalculator class, we can easily replace the call to the area method with calculate and also check if the object is an instance of the ManageShapeInterface and not the ShapeInterface.