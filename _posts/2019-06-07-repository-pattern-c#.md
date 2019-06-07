---
layout: post
title: Repository pattern c#
---

Today I thought I'd do an example of the design pattern called **Repository pattern**. Some of the advantages of the repository pattern are:

* Business logic can be tested without need for an external source
* Database access logic can be tested separately
* No duplication of code
* Caching strategy for the datasource can be centralized
* Domain driven development is easier
* Centralizing the data access logic, so code maintainability is easier

To implement a repository pattern we need the following:

#### Repository interface

```c#
public interface IRepository<T> where T : class
{
    T FindById( int id );
}
```

The interface holds the operations you want the repositories to be able to perform. Usually these are the `CRUD` __(Create, Read, Update, Delete)__ operations. I've only added a simple find by id method in this example.

#### Entity

```c#
public class Product { 
    public int Id { get; set; }
}
```

This is the entity which will be found by the repository.

#### Repository implementation

```c#
public class ProductRepository : IRepository<Product>
{
    Context _context;

    public ProductRepository() => 
        _context = new Context();

    public Product FindById( int id ) => 
        ( from p in _context.Products where p.Id == id select p ).FirstOrDefault();
}
```

The repository implementation is the actual implemantiation of the repository interface and is the thing that does all the work for us.

To use our repository we simple do like this:

```c#
public static Product FindProduct( int id )
{
    var productRepository = new ProductRepository();
    return productRepository.FindById( id );
}
```

The complete code displayed in this post can be found here: [c# Repository pattern example](https://github.com/jonasolin/designpatterns/tree/master/csharp/RepositoryPattern).

Check out the links below for more information regarding the builder pattern.

### Links
[Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)  
[Repository Design Pattern in C#](https://dotnettutorials.net/lesson/repository-design-pattern-csharp/)  
