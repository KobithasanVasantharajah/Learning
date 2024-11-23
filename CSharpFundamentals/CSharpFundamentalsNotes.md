# C# Basics for Beginners

## Section 1: Introduction to C# and .NET Framework

- Nothing major

## Section 2: Introduction to C# and .NET Framework

### C# vs .NET
-	.NET is a framework for building applications on Windows
-	.NET has 2 components: CLR (Common Language Runtime), Class Library- used for building applications

### What is CLR?
-	When you compile an application, C# compiler compiles the code to IL (Intermediate language) code.
-	IL code is platform agnostic, which makes it possible to take a C# program on a different computer with different hardware architecture and OS and run it.
-	CLR compiles IL code into native machine code for the computer on which it is running- process is called Just-in-time Compilation (JIT)
     Architecture of .NET Applications
-	A namespace is a container for related classes
-	An assembly is a file (DLL or EXE) that contains one or more namespaces.
     o	A DLL- dynamic linked library- is a file that includes code that can be re-used across different programs
     o	An EXE- executable- file represents a program that can be executed
     o	Application made of one or more Assembly

### Getting Visual Studio
-	Visual Studio is not supported by Mac, so I am using Rider instead.

### Our First C# Application
-	Printing “Hello World” in the command line.

### What is a ReSharper?
-	ReSharper is a commercial plug-in for Visual Studio that allows you to write code a lot quicker with less work.

###  Quiz 1: Fundamentals of C# and .NET
- What is a namespace? 
    - A container for classes. 
- What is JIT Compilation? 
  - The compilation of IL code to native machine code at runtime. 
- What is an assembly?
  - A single unit of deployment of .NET applications.
  - It’s a file, in the form of a executable or DLL, that contains one or more namespaces and classes.

## Section 3: Primitive Types and Expressions

###  Variables and Constants
-	Naming Conventions 
  - For local variables: `Camel Case => int number`;
  - For constants: `Pascal Case => const int MaxZoom = 5`;

### Overflowing

In C#
```csharp
byte number = 255;
number = number + 1; // 0
```
to stop overflowing you need to use the checked keyword
```csharp
checked
{
    byte number = 255;
    number = number + 1;
}
```

### Scope
Where a variable/constant has meaning and is accessible.

### Demo: Variables and Constants

```csharp
    Console.WriteLine("{0} {1}", byte.MinValue, byte.MaxValue); // Prints out the range that can be stored in a byte
    Console.WriteLine("{0} {1}", float.MinValue, float.MaxValue); // Prints out the range that can be stored in a float
```
### Type Conversion
#### Implicit Type Conversion:
    - When you copy a byte to an integer there is no data loss as the types are compatible.
    - When you copy an integer to a byte there might be data loss if the integer is larger than the byte for example, "300" cannot be stored in a byte. In this situation you need to explicitly tell the compiler that you are aware of the data loss and you still want to go ahead with the conversion.
#### Explicit Type Conversion:
  - You need to prefix the variable with the target type.
```csharp
  int i = 1;
  byte b = byte(i); //Known as Casting
  
  float f = 1.0f;
  int i = (int)f;
```
#### Non-compatible types:
```csharp
  string s = "1";
  int i = (int)s; //won't compile
 ```
So you need to do the following:
```csharp
  string s = "1";
  int i = Convert.ToInt32(s);
  in j = int.Parse(s);
 ```

### Operators
Increment `++`
Decrement `--`
Postfix increment 
```csharp
int a = 1;
int b = a++;
//a = 1, b = 2
```
Prefix increment
```csharp
int a = 1;
int b = ++a;
//a = 2, b = 2
```

## Section 4: Non-primitive Types

###  Arrays

```csharp
int number1;
int number2;
int number3;

int[] numbers = new int[3]; //no. of elementts cannot be changed
```
The default value of any integer that haven't been initialised in an array are set to 0.
Similarly, for an array of booleans they are defaulted to false if they haven't been initialised.
```csharp
var names = new string[3] {"Jack", "Jones", "Mary" };
```

### Strings
In C# strings are immutable.

```csharp
string name = firstName + " " + lastName;
string name = string.Format("{0} {1}", firstName, lastName);
var numbers = new int[3] {1, 2, 3};
string list = string.Join(",", numbers);
string name = "Mosh";
char firstChar = name[0];
    
```

### Verbatim Strings
```csharp
string path = "c:\\projects\\project1\\folder1";
string path = @"c:\projects\project1\folder1";
```

### Enums

```csharp
const int RegularAirMail = 1;
const int RegisteredAirMail = 2;
const int Express = 3;

public enum ShippingMethod : byte
{
    RegularAirMail = 1;
    RegisteredAirMail = 2;
    Express = 3;
}

var method = ShippingMethod.Express;
Console.WriteLine((int) method);

var methodId = 3;
Console.WriteLine((ShippingMethod)methodId);

Console.WriteLine(method.toString());

var methodName = "Express";
var shippingMethod = (ShippingMethod) Enum.Parse(typeof (ShippingMethod), methodName);
```

### Reference Types and Value Types

Structures: 
- Primitive Types
- Custom structures

Classes:
- Arrays
- Strings
- Custom classes


Value Types i.e. Structures:
- Allocated on stack
- Memory allocation done automatically
- Immediately removed when out of scope

Reference Types i.e. Classes:
- You need to allocated memory
- Memory allocated on heap
- Garbage collected by CLR

## Section 6: Arrays and Lists

### Arrays

#### Single Dimension Arrays

```csharp
var numbers = new int[5];
var numbers = new int[5] {1, 2, 3, 4, 5};
```

#### Rectangular Array
- Each row has the exact same number of columns
```csharp
//Rectangular 2D
var matrix = new int[3, 5];
var matrix = new int[3, 5] 
{
    {1, 2, 3, 4, 5},
    {6, 7, 8, 9, 10},
    {11, 12, 13, 14, 15}
};

var element = matrix[0, 0];

//Rectangular 3D
var colors = new int[3, 5, 4];
```

#### Jagged Array
- Each row can have a different number of columns
- Think of a jagged array as an array of arrays
```csharp
var array = new int[3][];

array[0] = new int[4] {0, 1, 2, 3};
array[1] = new int[5] {0, 1, 2, 3, 4};
array[2] = new int[3] {0, 1, 2};

array[0][0] = 1;

// Jagged 
var array = new int [3][];

//Rectangular
var array = new int [3, 5]; 
```

### Array methods

```csharp
var numbers = new[] {3, 7, 9, 2, 14, 6};

//Length
Console.WriteLine("Length: " + numbers.Length(); //Outputs: Length: 6
    
//IndexOf()
var index = Array.IndexOf(numbers, 9);
Console.WriteLine("Index of 9: " + index); //Outputs: Length: 2

//Clear()
Array.Clear(numbers, 0, 2);

Console.WriteLine("Effect of Clear()");
foreach (var n in numbers)
    Console.WriteLine(n); //Outputs: 0, 0, 9, 2, 14, 6

//Copy
int[] another = new int[3];
Array.Copy(numbers, another, 3);

Console.WriteLine("Effect of Copy()");
foreach (var n in another)
    Console.WriteLine(n); //Outputs: 0, 0, 9


//Sort
Array.Sort(numbers);

Console.WriteLine("Effect of Sort()");
foreach (var n in numbers)
    Console.WriteLine(n); //Outputs: 0, 0, 2, 6, 9, 14

//Reverse
Array.Reverse(numbers);

Console.WriteLine("Effect of Reverse()");
foreach (var n in numbers)
    Console.WriteLine(n); //Outputs: 14, 9, 6, 2, 0, 0
```

### List
- Array: fixed size
- List: dynamic size
- 
```csharp
var numbers = new List<int>();
var numbers = new List<int>() {1, 2, 3, 4};
```

Useful Methods:
- Add()
- AddRange()
- Remove()
- RemoveAt()
- IndexOf()
- Contains()
- Count

```csharp
var numbers = new List<int>() {1, 2, 3, 4};
numbers.Add(1);
numbers.AddRange(new int[3] {5, 6, 7});

foreach (var number in numbers)
    Console.WriteLine(number); //Ouputs: 1, 2, 3, 4, 1, 5, 6, 7

Console.WriteLine();
Console.WriteLine("Index of 1: " + numbers.IndexOf(1)); //Outputs: 0
Console.WriteLine("Last Index of 1: " + numbers.LastIndexOf(1)); //Outputs: 4

Console.WriteLine("Count: " + numbers.Count); //Outputs: 8

numbers.Remove(1);
foreach (var number in numbers)
    Console.WriteLine(number); //Ouputs: 2, 3, 4, 1, 5, 6, 7
```
If you want to remove all the "1"s the below code will NOT WORK and throw a "Collection was modified" exception.

```csharp

foreach (var number in numbers)
    {
        if (number == 1)
        numbers.Remove(number);
    }
foreach (var number in numbers)
    Console.WriteLine(number); //Ouputs: 2, 3, 4, 5, 6, 7
```

Therefore, you will need use a for loop instead:

```csharp
for (var i = 0; i < numbers.Count; i++)
{
    if (numbers[i] == 1)
        numbers.Remove(numbers[i]);
}
foreach (var number in numbers)
    Console.WriteLine(number); //Ouputs: 2, 3, 4, 5, 6, 7

numbers.Clear(); //Removes all elements from the list
Console.WriteLine("Count: " + numbers.Count); //Outputs: 0

```

## Section 7: Working with Dates

### DateTime

```csharp
var dateTime = new DateTime(2015, 1, 1);
var now = DateTime.Now;
var today = DateTime.Today;

Console.WriteLine("Hour: " + now.Hour);
Console.WriteLine("Minute: " + now.Minute);

var tomorrow = now.AddDays(1);
var yesterday = now.AddDays(-1);

Console.WriteLine(now.ToLongDateString());
Console.WriteLine(now.ToShortDateString());
Console.WriteLine(now.ToLongTimeString());
Console.WriteLine(now.ToShortTimeString());
Console.WriteLine(now.ToString("yyyy-MM-dd HH:mm"));
```
### TimeSpan

```csharp
//Creating TimeSpan objects
var timeSpan = new TimeSpan(1, 2, 3); //The integers represents hours, minutes, and seconds

var timeSpan1 = new TimeSpan(1, 0, 0);
var timeSpan2 = TimeSpan.FromHours(1);

var start = DateTime.Now;
var end = DateTime.Now.AddMinutes(2);
var duration = end - start;
Console.WriteLine("Duration: " + duration);

//Properties
Console.WriteLine("Minutes: " + timeSpan.Minutes); //Outputs: Minutes: 2
Console.WriteLine("Total Minutes: " + timeSpan.TotalMinutes); //Outputs: Total Minutes: 62.05

//Add
Console.WriteLine("Add Example: " + timeSpan.Add(TimeSpan.FromMinutes(8))); //Outputs: Add Example: 01:10:03
Console.WriteLine("Subtract Example: " + timeSpan.Subtract(TimeSpan.FromMinutes(2))); //Outputs: Subtract Example: 01:00:03

//ToString
Console.WriteLine("ToString: " + timeSpan.ToString()); //Outputs: ToString: 01:02:03

//Parse
Console.WriteLine("Parse: " + TimeSpan.Parse("01:02:03")); //Outputs: Parse: 01:02:03
```

## Section 8: Working with Text

### String

#### Formatting 
```csharp
ToLower(); //"hello world"
ToUpper(); //"HELLO WORLD"
Trim(); //Gets rid of the white spaces around the string
```
#### Searching
```csharp
IndexOf('a');
LastIndexOf("Hello");
```

#### Substring
```csharp
Substring(startIndex)
Substring(startIndex, length)
```

#### Replacing
```csharp
Replace('a', '!');
Replace("mosh", "moshfegh");
```

#### Null Checking
```csharp
String.IsNullOrEmpty(str);
String.IsNullOrWhiteSpace(str);
```

#### Splitting
```csharp
str.Split(' ');
```

#### Converting Strings to Numbers
```csharp
string s = "1234";
int i = int Parse(s);
int j = Convert.ToInt32(s);
```

#### Converting Numbers to Strings
```csharp
int i = 1234;
string s = i.ToString(); //"1234"
string t = i.ToString("C"); //"$1,234.00"
string t = i.ToString("C0"); //"$1,234"
```

#### Demo: String

```csharp
var fullName = "Kobithasan Vasantharajah ";
Console.WriteLine("Trim: '{0}' ", fullName.Trim()); //Gets rid of the white space at the end of the string
Console.WriteLine("ToUpper: '{0}' ", fullName.Trim().ToUpper()); //Output: "KOBITHASAN VASANTHARAJAH"

var index = fullName.IndexOf(' ');
var firstName = fullName.Substring(0, index);
var lastName = fullName.Substring(index + 1);
Console.WriteLine("FirstName: " + firstName); //Output: Kobithasan
Console.WriteLine("LastName: " + lastName); //Output: Vasanthararajah

var names = fullName.Split(' ');
Console.WriteLine("FirstName: ", names[0]); //Output: Kobithasan
Console.WriteLine("LastName: ", names[1]); //Output: Vasantharajah

Console.WriteLine(fullName.Replace("Kobithasan", "Kobi")); //Output: Kobi Vasantharajah

if (String.IsNullOrWhiteSpace(" "))
    Console.WriteLine("Invalid");

var str = "26";
var age = Convert.ToByte(str);
Console.WriteLine(age); //Output: 26

float price = 29.95f;
Console.WriteLine(price.ToString("C")); //Output: $29.95
Console.WriteLine(price.ToString("C0")); //Output: $30 - rounds it up
```

### Procedural Programming
A programming paradigm based on procedure calls.

## Section 9: Working with Files

### System.IO Namespace

#### Demo File and FileInfo

```csharp
var path = @"c:\somefile.jpg";

File.Copy(@"c:\temp\myfile.jpg", @"d:\temp\myfile.jpg", true);
File.Delete(path);
if (File.Exists(path));
{
    //
}
var content = File.ReadAllText(path);

var fileInfo = new FileInfo(path);
fileInfo.CopyTo("...");
fileInfo.Delete();
if (fileInfo.Exists)
{
    //
}
```

#### Demo: Path

```csharp
var path = @"C:\Projects\CSharpFundamentals\HelloWorld\HelloWorld/sln";

var dotIndex = path.IndexOf('.');
var extension = path.Substring(dotIndex);

Console.WriteLine("Extension: " + Path.GetExtension(path));
Console.WriteLine("File Name: " + Path.GetFileName(path));
Console.WriteLine("File Name without Extension: " + Path.GetFileNameWithoutExtension(path));
Console.WriteLine("Directory Name: " + Path.GetDirectoryName(path));
```

## Section 10: Debugging Applications

### Debugging Applications
Shortcuts:
- **F5** to run application in debug mode
- **F10** to step over a method
- **Shift + F11** to step outside a method
- **Shift + F5** to stop debugging