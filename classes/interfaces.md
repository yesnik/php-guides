# Interfaces in PHP

## Example of using interface

We want to have class Game that can draw different points - in cylindrical and cartesian coordinate systems.

```php
interface Point
{
    public function getCoordinates();
}

class CartesianPoint implements Point
{
    private $x;
    private $y;
    private $z;
    
    public function __construct($x, $y, $z)
    {
        $this->x = $x;
        $this->y = $y;
        $this->z = $z;
    }
    
    public function getCoordinates()
    {
        return [$this->x, $this->y, $this->z];
    }
}

class CylindricalPoint implements Point
{
    private $r;
    private $f;
    private $z;
    
    public function __construct($r, $f, $z)
    {
        $this->r = $r;
        $this->f = $f;
        $this->z = $z;
    }
    
    public function getCoordinates()
    {
        return [
            $this->r * cos($this->f),
            $this->r * sin($this->f),
            $this->z
        ];
    }
}

class Game
{
    public static function draw(Point $point)
    {
        list($x, $y, $z) = $point->getCoordinates();
        return "[$x, $y, $z]";
    }
}

$point = new CartesianPoint(1, 2, 3);
$point2 = new CylindricalPoint(100, 3.14159, 3);

echo Game::draw($point); // [1, 2, 3]
echo Game::draw($point2); // [-99.999999999648, 0.00026535897933527, 3]
```

## Check if an instance's class implements an interface

```php
interface Reservable {};

class Car implements Reservable {};

$bmw = new Car();
var_dump($bmw instanceof Reservable); // bool(true)

class Robot {}
$robot = new Robot();
var_dump($robot instanceof Reservable); // bool(false)
```
