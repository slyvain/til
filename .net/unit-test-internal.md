# Unit test internal classes

:notebook: Official documentation: https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.internalsvisibletoattribute

Internal classes are, by definition, not visible from outside the project in which they reside. It is therefore not possible to test them from a unit test project.
Exposing those classes as public for the sole purpose of testing them is not recommended.

Here's a simple example, say we have an assembly `MyProject` with an internal class `HelloWorld`. We also have an unit test assembly usually named after the assembly we want to test, eg. `MyProject.Tests`.


```csharp
namespace MyProject
{
   internal class HelloWorld
   {
     public string ReturnHelloWorld()
     {
       return "Hello World!";
     }
   }
}
```

```csharp
namespace MyProject.Tests
{
   public class HelloWorldTests
   {
     [Fact]
     public void ReturnHelloWorld_Should_Return_HelloWorld()
     {
        var sut = new HelloWorld();
        var result = sut.ReturnHelloWorld();
        result.Should().Be("Hello World!");
     }
   }
}
```

The code above, in the unit test, would show compilation errors:
 ```
 CS0122: 'HelloWorld' is inaccessible due to its protection level
 CS0122: 'ReturnHelloWorld' is inaccessible due to its protection level
 ```

## Attribute on the namespace

Setting an attribute above the namespace of `MyProject` would fix the issue.

```csharp
[assembly: InternalsVisibleTo("MyProject.Tests")]
namespace MyProject
```

## Attribute in the .csproj

We can also enable the attribute to the whole project by setting the attribute in the `.csproj` file:

```csharp
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.1</TargetFramework>
    </PropertyGroup>

    <ItemGroup>
        <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
            <_Parameter1>$(AssemblyName).Tests</_Parameter1>
        </AssemblyAttribute>
    </ItemGroup>

</Project>
```