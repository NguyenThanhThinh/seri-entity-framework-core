## Entity Framework Core: Overview

The standard ORM framework for .NET and .NET Core

Provides LINQ-based data queries and CRUD operations

Automatic change tracking of in-memory objects

## Entity Framework Core: Setup

To add EF Core support to a project in **Visual Studio**:

Install it from **Package Manager Console**

```Install-Package Microsoft.EntityFrameworkCore```

EF Core is modular – any data providers must be installed too:

```Install-Package Microsoft.EntityFrameworkCore.SqlServer```

## Database First Model: Setup
Scaffolding DbContext from DB with Scaffold-DbContext command in Package Manager Console:

```Scaffold-DbContext -Connection "Server=.;Database=…;Integrated Security=True"-Provider Microsoft EntityFrameworkCore.SqlServer -OutputDir Data```

Scaffolding requires the following packages beforehand:

```Install-Package Microsoft.EntityFrameworkCore.Tools```

```Install-Package Microsoft.EntityFrameworkCore.SqlServer.Design```

## The DbContext Class

DbContext provides:

CRUD Operations

A way to access entities

Methods for creating new entities (Add() method)

Ability to manipulate database data by modifying objects

Easily navigate through table relations

Executing LINQ queries as native SQL queries

Managing database creation/deletion/migration

## Using DbContext Class

First create instance of the DbContext:
```var context = new HRDbContext();```
In the constructor you can pass a database connection string
DbContext properties:

Database - EnsureCreated/Deleted methods, DB Connection

ChangeTracker - Holds info about the automatic change tracker

All entity classes (tables) are listed as properties

e.g. DbSet<Employee> Employees { get; set; }

## Reading Data with LINQ Query

Executing LINQ-to-Entities query over EF entity:


```js
 using (var context = new HRDbContext())
{
  var employees = context.Employees
    .Where(e => e.JobTitle == "Design Engineer")
    .ToArray();
}
```

Employees property in the DbContext:

```js
public partial class HRDbContext : DbContext
{
  public DbSet<Employee> Employees { get; set; }
  public DbSet<Project> Projects { get; set; }
  public DbSet<Department> Departments { get; set; }
}

```

We can also use **extension methods** for constructing the query

```js
using (var context = new HRDbContext())
{
  var employees = context.Employees
      .Where(c => c.JobTitle == "Design Engineering")
      .Select(c => c.FirstName)
      .ToList();
}

```

Find element by ID

```js
using (var context = new HRDbContext())
{
  var project = context.Projects.Find(2);
  Console.WriteLine(project.Name);
}
```
## LINQ: Simple Operations

**Where()**

Searches by given condition

**First/Last() / FirstOrDefault/LastOrDefault()**

Gets the first/last element which matches the condition

Throws InvalidOperationException without OrDefault

**Select()**
Projects (conversion) collection to another type

**OrderBy() / ThenBy() / OrderByDescending()**

Orders a collection... etc

## What is the Code First Model?

Code First means to write the .NET classes and let EF Core create the database from the mappings


## Why Use Code First?

- Write code without having to define mappings in XML or create database tables

- Define objects in C# format

- Data Annotations or Fluent API describe properties

- Changes to code can be reflected (migrated) in the schema





