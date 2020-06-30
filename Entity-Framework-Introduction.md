## Entity Framework Core: Overview

- The standard ORM framework for .NET and .NET Core

- Provides LINQ-based data queries and CRUD operations

- Automatic change tracking of in-memory objects

## Entity Framework Core: Setup

To add EF Core support to a project in **Visual Studio**:

Install it from **Package Manager Console**

```Install-Package Microsoft.EntityFrameworkCore```

EF Core is modular – any data providers must be installed too:

```Install-Package Microsoft.EntityFrameworkCore.SqlServer```

## Database First Model: Setup
Scaffolding DbContext from DB with Scaffold-DbContext command in Package Manager Console:

```js
Scaffold-DbContext -Connection "Server=.;Database=…;Integrated Security=True"
-Provider Microsoft EntityFrameworkCore.SqlServer -OutputDir Data
```

Scaffolding requires the following packages beforehand:

```js
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.EntityFrameworkCore.SqlServer.Design
```

## The DbContext Class

DbContext provides:

- CRUD Operations

- A way to access entities

- Methods for creating new entities (Add() method)

- Ability to manipulate database data by modifying objects

- Easily navigate through table relations

- Executing LINQ queries as native SQL queries

- Managing database creation/deletion/migration

## Using DbContext Class

First create instance of the DbContext:
```var context = new HRDbContext();```
In the constructor you can pass a database connection string
DbContext properties:

- Database - Created/Deleted methods, DB Connection

- ChangeTracker - Holds info about the automatic change tracker

- All entity classes (tables) are listed as properties

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
public class HRDbContext : DbContext
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

Code First means to write the .NET classes and let EF Core create the database from the mappings


## Why Use Code First?

- Write code without having to define mappings in XML or create database tables

- Define objects in C# format

- Data Annotations or Fluent API describe properties

- Changes to code can be reflected (migrated) in the schema

## Code First with EF Core: Setup

```js
Install-Package Microsoft.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.SqlServer

```

## How to Connect to SQL Server?

- One way to connect is to create a **Configuration** class with your connection string:

```js
public static class Configuration
{
  public const string ConnectionString = "Server=.;Database=…;";
}

```
- Then add the connection string in the OnConfiguring method in the DbContext class

```js
protected override void OnConfiguring(DbContextOptionsBuilder builder)
{
  if (!builder.IsConfigured)
    builder.UseSqlServer(Configuration.ConnectionString);
}

```

## Fluent API

- The OnModelCreating Method let us use the Fluent API to describe our table relations to EF Core

```js
protected override void OnModelCreating(ModelBuilder builder)
{
  builder.Entity<Category>()
    .HasMany(c => c.Posts)
    .WithOne(p => p.Category);

}

```

## What Are Database Migrations?

Updating database schema without losing data

 - Adding/dropping tables, columns, etc.

Migrations in EF Core keep their history 

 - Entity Classes, DB Context versions are all preserved

Automatically generated

## Migrations in EF Core

To use migrations in EF Core, we use the Add-Migration command from the Package Manager Console

```js
Add-Migration {MigrationName}
 ```

 To undo a migration, we use Remove-Migration  
 ```Remove-Migration```

Commit changes to the database, using Update-Database

```Update-Database```

## Summary

- ORM frameworks maps database schema to objects in a programming language
- Entity Framework Core is the standard .NET ORM
- LINQ can be used to query the DB through the DB context







