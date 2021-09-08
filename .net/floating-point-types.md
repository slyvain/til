# Floating Point Types

A little reminder on how to declare floating point types, and below, some gotchas when manipulating them.

## Declaration

```c#
double d = 1;
d = 1d;
d = 1D;
d = 1.2; //double is the default if there's no suffix
d = 1.2d; //The suffix is redundant here
d = .2;
d = 12e-12;
d = 12E-12;
d = 1_234e-1_2; //digit separators are allowed since C# 7

float f = 1;
f = 1f;
f = 1F;
f = 1.2f;
f = .2f;
f = 12e-12f;
f = 12E-12f;
f = 1_234e-1_2f;

decimal m = 1;
m = 1m;
m = 1M;
m = 1.2m;
m = .2m;
m = 12e-12m;
m = 12E-12m;
m = 1_234e-1_2m;
```

## Gotchas

A little compilation of traps to be aware of when dealing with floating point variables. Especially when manipulating financial numbers.
Some more reading about [floating points](https://www.extremeoptimization.com/resources/Articles/FPDotNetConceptsAndFormats.aspx).

##### example 1
> Maths are not maths anymore
```c#
double x = 3.65, y = 0.05, z = 3.7;
Console.WriteLine((x + y) == z); // false
```

##### example 2
> Maths are not maths anymore, even simpler example
```c#
double a = 0.1;
Console.WriteLine(a + a + a == 0.3); // false
```

##### example 3
> 1 / 100 * 100 == 1, right? Right!
  ```c#
Console.WriteLine("((double)(1/100.0))*100 < 1 is {0}.", ((double)(1 / 100.0)) * 100 < 1); // false
Console.WriteLine("((float)(1/100.0F))*100 > 1 is {0}.", ((float)(1 / 100.0F)) * 100 > 1); // false
```
> Same with 103, right? RIGHT??
  ```c#
Console.WriteLine("((double)(1/103.0))*103 < 1 is {0}.", ((double)(1 / 103.0)) * 103 < 1); // true
Console.WriteLine("((float)(1/103.0F))*103 > 1 is {0}.", ((float)(1 / 103.0F)) * 103 > 1); // false
```

##### example 4
> Give a 10% discount on 100, we get 89 instead of 90
  ```c#
int fullPrice = 100;
float discount = 0.1F;
Int32 finalPrice = (int)(fullPrice * (1 - discount));
Console.WriteLine("The discounted price is ${0}.", finalPrice); // 89 instead of 90
```
