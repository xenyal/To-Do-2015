## Single Responsibility Principle
SRP, in short, states that:
> A class should have one and only one reason to change, meaning that a class should only have one job.
For example, say we have a couple of shapes and we want to find the sum of all the areas of the shapes.

```PHP
class Circle {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }
}

class Square {
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }
}
```

First, we create our shapes classes and have the constructors setup the required parameters. Next, we move on to creating the AreaCalculator class and then write up our logic to find the sum of the areas of the provided shapes.

```PHP
class AreaCalculator {

    protected $shapes;

    public function __construct($shapes = array()) {
        $this->shapes = $shapes;
    }

    public function sum() {
        // logic to sum the areas
    }

    public function output() {
        return implode('', array(
            "<h1>",
                "Sum of the areas of provided shapes: ",
                $this->sum(),
            "</h1>"
        ));
    }
}
```

To use the AreaCalculator class, we simply instantiate the class and pass in an array of shapes, and display the output at the bottom of the page.

```PHP
$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);

echo $areas->output();
```

The problem with the output method is that the AreaCalculator handles the logic to output the data. Therefore, what if the user wanted to output the data as json or something else?

All of that logic would be handled by the AreaCalculator class, this is what SRP frowns against; the AreaCalculator class should only sum the areas of provided shapes, it should not care whether the user wants json or HTML.

So, to fix this you can create an SumCalculatorOutputter class and use this to handle whatever logic you need to handle how the sum areas of all provided shapes are displayed.

The SumCalculatorOutputter class would work like this:

```PHP
$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);
$output = new SumCalculatorOutputter($areas);

echo $output->JSON();
echo $output->HAML();
echo $output->HTML();
echo $output->JADE();
```
 
Now, whatever logic you need to output the data to the user is now handled by the SumCalculatorOutputter class.