---
layout: post
title: Builder pattern c#
---

Today I thought I'd do an example of the design pattern called **Builder pattern**. I've copied the description below from the [Wikipedia page for Builder pattern](https://en.wikipedia.org/wiki/Builder_pattern).

>The Builder is a design pattern designed to provide a flexible solution to various object creation problems in object-oriented programming. The intent of the Builder design pattern is to separate the construction of a complex object from its representation.

As usual I'm gonna keep it real simple. 😎

Suppose we wanted to create different kind of cars with different brands. 

To do this using the builder pattern we need the following components:

#### Builder interface

```c#
public interface ICarBuilder
{
    void SetBrand();
    void SetOrigin();
    Car GetCar();
}
```

#### Builder

```c#
public class VolvoCarBuilder : ICarBuilder
{
    private readonly Car _car = new Car();
    public Car GetCar() => _car;
    public void SetBrand() => _car.Brand = "Volvo";
    public void SetOrigin() => _car.Country = "Sweden";
}
```

#### Director

```c#
public class CarWorkshop
{
    private readonly ICarBuilder _builder;
    public CarWorkshop( ICarBuilder builder ) => _builder = builder;
    public CarWorkshop AssembleTheCar()
    {
        _builder.SetBrand();
        _builder.SetOrigin();
        return this;
    }
    public Car RollItOutFromTheWorkshop() => _builder.GetCar();
}
```

#### Product

```c#
public class Car
{
    public string Brand { get; set; }
    public string Country { get; set; }
    public override string ToString() => 
        $"New car with brand {Brand} from country {Country} created!";
    public void WriteToConsole() => Console.WriteLine( this );
}
```

Simply speaking it works like this:

1. The builder interface defines a template for creating the product
2. The builder(s) implements the builder interface and makes it possible to create the product
3. The director is the one actually creating the product using the builder
4. The product is the end result

To see this in action, all we need to do is this:

```c#
static void Main( string[] args )
{
    // Volvo
    var volvoCarBuilder = new CarWorkshop( new VolvoCarBuilder() );

    volvoCarBuilder.
        AssembleTheCar().
        RollItOutFromTheWorkshop().
        WriteToConsole();

    // BMW
    var bmwCarBuilder = new CarWorkshop( new BmwCarBuilder() );

    bmwCarBuilder.
        AssembleTheCar().
        RollItOutFromTheWorkshop().
        WriteToConsole();

    Console.ReadLine();
}

// The example displays the following output:
//      New car with brand Volvo from country Sweden created!
//      New car with brand BMW from country Germany created!
```

The complete code displayed in this post can be found here: [c# Builder pattern example](https://github.com/jonasolin/designpatterns/tree/master/csharp/BuilderPattern).

Check out the links below for more information regarding the builder pattern.

### Links
[Builder pattern on Wikipedia](https://en.wikipedia.org/wiki/Builder_pattern)  
[Refactoring Guru Builder pattern](https://refactoring.guru/design-patterns/builder/csharp/example)  
