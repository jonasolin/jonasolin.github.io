---
layout: post
title: Random decimal c#
---

I stumbled across this issue the other day when I was working on a side project. I was working with coordinates and I wanted to be able to create random coordinates to use in my tests. I tried using [Random](https://docs.microsoft.com/en-us/dotnet/api/system.random) but it does not support `decimal` by default. So I had to come up with a solution for a method that returned a random decimal between a min and max value. How hard could it be? 😂 

It was not as easy as I thought it would be. It was not terribly difficult either but hopefully this post can save someone an hour or two. 

I ended up with the following which I think is pretty sweet.

```c#
public static decimal NextDecimal( this Random rand, int min, int max ) => 
      new decimal( min + ( rand.NextDouble() * ( max - min ) ) );
```

I created an extension method for [Random](https://docs.microsoft.com/en-us/dotnet/api/system.random) which takes a min and a max value. It also works with negative values which was one of my requirements because the min values for latitude/longitude are negative numbers.

### Example

```c#
public void NextDecimalExample()
{
    var rand = new Random();

    for ( int i = 0; i < 5; i++ )
    {
        var randomDecimal = rand.NextDecimal( -90, 90 );
        Console.WriteLine( randomDecimal.ToString() );
    }
}

// The example displays the following output:
//      29,0170268337322
//      -83,9800323983561
//      -64,2345922320311
//      -39,2845007354787
//      14,7174322906497
```