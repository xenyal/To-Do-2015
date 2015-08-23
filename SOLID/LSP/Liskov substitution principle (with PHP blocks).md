## Liskov substitution principle
**Creds to Samuel Oloruntoba(@kayandrae07) from Scotch IO**

> Let **q(x)** be a property provable about objects of **x** of type **T**. Then **q(y)** should be provable for objects **y** of type **S** where **S** is a subtype of **T**.

All this is stating is that every subclass/derived class should be substitutable for their base/parent class.

Still making use of out AreaCalculator class, say we have a VolumeCalculator class that extends the AreaCalculator class:

```PHP
class VolumeCalculator extends AreaCalulator {
    public function __construct($shapes = array()) {
        parent::__construct($shapes);
    }

    public function sum() {
        // logic to calculate the volumes and then return and array of output
        return array($summedData);
    }
}    
```

In the SumCalculatorOutputter class:

```PHP
class SumCalculatorOutputter {
    protected $calculator;

    public function __constructor(AreaCalculator $calculator) {
        $this->calculator = $calculator;
    }

    public function JSON() {
        $data = array(
            'sum' => $this->calculator->sum();
        );

        return json_encode($data);
    }

    public function HTML() {
        return implode('', array(
            '<h1>',
                'Sum of the areas of provided shapes: ',
                $this->calculator->sum(),
            '</h1>'
        ));
    }
}    
```

If we tried to run an example like this:

```PHP
$areas = new AreaCalculator($shapes);
$volumes = new AreaCalculator($solidShapes);

$output = new SumCalculatorOutputter($areas);
$output2 = new SumCalculatorOutputter($volumes);
```

The program does not squawk, but when we call the HTML method on the $output2 object we get an E_NOTICE error informing us of an array to string conversion.

To fix this, instead of returning an array from the VolumeCalculator class sum method, you should simply:

```PHP
public function sum() {
    // logic to calculate the volumes and then return and array of output
    return $summedData;
}
```

The summed data as a float, double or integer.