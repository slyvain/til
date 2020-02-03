# Homonymous NuGet and Project names

I wanted to quickly test a piece of code with [BenchmarkDotNet](https://github.com/dotnet/BenchmarkDotNet).
So I opened Visual Studio, created a new Console Application thoughtlessly called `BenchmarkDotNet`, and of course added the `BenchmarkDotNet` NuGet package.

And there lied the problem, the compiler showed the following error:

> NU1108	Cycle detected. 
>           BenchmarkDotNet -> BenchmarkDotNet (>= 0.11.5).

Put simply: a project and a dependency cannot share the same name.

There is an open issue on this: https://github.com/NuGet/Home/issues/6754