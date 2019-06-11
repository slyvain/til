# Out Var

Since C# 7.0 it is no more necessary to pre-declare a variable and its type in a separate statement before passing it as an `out` parameter.

### Before C# 7

```cs
string str = "2019";
int value;
int.TryParse(str, out value);

Console.WriteLine(value); 
```

### After C# 7

```cs
string str = "2019";
int.TryParse(str, out int value);

Console.WriteLine(value);
```

Here, `value` is still strongly typed, but it can also be declared as a `var`.

```cs
string str = "2019";
int.TryParse(str, out var value);

Console.WriteLine(value);
```