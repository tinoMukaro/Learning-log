Java

# Java Basics

When we write a Java program, we normally write the code inside a file that ends with `.java`.
For example:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
This file would be saved as:
Main.java
The reason it is called Main.java is because the public class inside the file is called Main.
When the program is still in the .java file, it is just normal Java source code. The computer does not run that directly.
So before running it, Java uses a compiler called javac.
javac simply means Java compiler.
Its job is to take your .java file and convert it into a .class file.
So the flow is like this:
Main.java  ->  javac  ->  Main.class
The .class file contains something called bytecode.
Bytecode is not normal Java code anymore, but it is also not machine code for one specific computer. It is code that the JVM understands.
So after compiling, we run the program using:
java Main
Not:
java Main.class
Because when running the program, Java wants the class name, not the file name.
So the full process is:
Write code in Main.java
Compile it using javac
This creates Main.class
Run it using java Main
The JVM runs the bytecode
```

##

Now the JVM is the Java Virtual Machine.
The JVM is the thing that actually runs Java bytecode.
This is one of the main reasons Java is popular. Java was designed so that you can write your code once and run it on different machines.
For example, the same .class file can run on Windows, Linux, or Mac, as long as that machine has a JVM.
That is where the idea comes from:
Write once, run anywhere
You do not compile Java separately for every operating system. You compile it into bytecode, then the JVM for that operating system runs it.

##

Then there is the JRE.
JRE means Java Runtime Environment.
The JRE is for running Java programs.
It contains the JVM plus the libraries needed for Java programs to run.
So you can think of it like this:
JRE = JVM + things needed to run Java apps
If someone only wants to run a Java app, they need the JRE.

##

Then there is the JDK.
JDK means Java Development Kit.
This is what developers use.
The JDK contains the JRE, but it also includes development tools like javac.
So:
JDK = JRE + tools for building Java apps

##

```java
Now for variables.
A variable is just a named place where we store a value.
For example:
int age = 25;
This means we are creating a variable called age, and we are storing the value 25 inside it.
The int part tells Java what type of value the variable will store.
So in this example:
int  -> the type
age  -> the variable name
25   -> the value
The general format is:
type variableName = value;
For example:
int quantity = 10;
double price = 5.99;
char grade = 'A';
boolean isActive = true;

Java has primitive data types.
Primitive types are the basic/simple types that come built into Java.
There are 8 primitive types:
byte
short
int
long
float
double
char
boolean
These are used to store simple values like whole numbers, decimal numbers, single characters, and true/false values.
For whole numbers, Java gives us byte, short, int, and long.
A byte is for very small whole numbers.
byte age = 25;
It can store values from -128 to 127.
You will not use byte every day, but it is there when you want to save memory.
A short is also for whole numbers, but it can store bigger values than byte.
short year = 2026;
It can store values from -32,768 to 32,767.
Again, you will not use it most of the time, but it is part of the primitive types.
The one you will use most for whole numbers is int.
int quantity = 10;
int is the normal/default choice for whole numbers in Java.
So if you want to store things like age, quantity, count, number of users, or stock amount, you will usually use int.
Then there is long.
long is used when the whole number is very big.
long population = 15000000000L;
Notice the L at the end.
For long, it is good to add L so Java knows the number should be treated as a long value.

Example:
long phoneNumber = 263771234567L;
For decimal numbers, Java gives us float and double.
A float stores decimal numbers, but it is less precise than double.
float price = 10.5f;
Notice the f at the end.
If you use float, you must add f, because decimal numbers are treated as double by default in Java.
The decimal type you will normally use is double.
double totalPrice = 99.99;
double is more precise than float, and it is the default choice for decimal numbers.
So for prices, measurements, percentages, and calculations with decimals, you will usually use double.
Then there is char.
char is used to store one character only.
char grade = 'A';
A char uses single quotes.
So this is correct:
char letter = 'T';
This is wrong:
char letter = "T";
Double quotes are for strings, not characters.
A char can store one letter, one digit, or one symbol.
char grade = 'A';
char number = '1';
char symbol = '@';
Then there is boolean.
A boolean stores only two possible values:
true
false

Example:
boolean isLoggedIn = true;
boolean isDeleted = false;
Booleans are very common in conditions.

Example:

if (isLoggedIn) {
    System.out.println("Welcome back");
}

So we use boolean when something is either yes or no, true or false, active or not active, paid or not paid.

So in normal daily Java coding, you will mostly use:

int      -> whole numbers
double   -> decimal numbers
boolean  -> true or false
char     -> one character
long     -> very big whole numbers

Example:

int quantity = 5;
double price = 24.99;
boolean paid = false;
char grade = 'A';
long phoneNumber = 263771234567L;
```

# Java Conditionals and Loops — Casual Explanation Notes

Conditionals and loops are used to control how a program behaves.

A program does not always run from top to bottom only.

Sometimes we want to say:

```txt
If this is true, do this.
If not, do something else.

And sometimes we want to say:

Repeat this code several times.
Keep doing this while something is true.

That is where conditionals and loops come in.

1. Conditionals

Conditionals help Java make decisions.

The most common conditional is if.

if

An if statement runs code only when a condition is true.

Example:

int age = 20;

if (age >= 18) {
    System.out.println("You are an adult");
}

Here, Java checks:

Is age greater than or equal to 18?

If yes, it prints:

You are an adult

If no, it skips that block.

if else

if else is used when we want one thing to happen if the condition is true, and another thing to happen if it is false.

Example:

int age = 16;

if (age >= 18) {
    System.out.println("You are an adult");
} else {
    System.out.println("You are under age");
}

The flow is:

If age >= 18
    print "You are an adult"
Otherwise
    print "You are under age"
else if

else if is used when we have more than two possible conditions.

Example:

int mark = 65;

if (mark >= 75) {
    System.out.println("Distinction");
} else if (mark >= 50) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}

Java checks from top to bottom.

The first condition that is true will run.

After that, Java skips the rest.

So with mark = 65, this runs:

Pass

Because 65 is not greater than or equal to 75, but it is greater than or equal to 50.

2. Comparison Operators

These are used to compare values.

Operator	Meaning	Example
==	equal to	age == 18
!=	not equal to	age != 18
>	greater than	age > 18
<	less than	age < 18
>=	greater than or equal to	age >= 18
<=	less than or equal to	age <= 18

Important:

age = 18;   // assigning value
age == 18;  // comparing value

Use one = when giving a value.

Use double == when comparing.

3. Logical Operators

Logical operators are used when checking more than one condition.

Operator	Meaning	Example
&&	AND	both conditions must be true
`		`
!	NOT	reverses true/false
&& means AND

Both conditions must be true.

int age = 20;
boolean hasId = true;

if (age >= 18 && hasId) {
    System.out.println("Allowed");
}

This means:

If age is 18 or above AND hasId is true, allow.
|| means OR

At least one condition must be true.

boolean hasCash = false;
boolean hasCard = true;

if (hasCash || hasCard) {
    System.out.println("Can pay");
}

This means:

If the person has cash OR card, they can pay.
! means NOT

It reverses the value.

boolean isActive = false;

if (!isActive) {
    System.out.println("Account is not active");
}

This means:

If isActive is false, run this block.
4. Nested if

A nested if is an if inside another if.

Example:

boolean loggedIn = true;
boolean isAdmin = true;

if (loggedIn) {
    if (isAdmin) {
        System.out.println("Welcome admin");
    }
}

This works, but too many nested if statements can make code hard to read.

In real projects, developers usually try to avoid deep nesting.

5. Switch Statement

A switch is useful when checking one value against many possible values.

Example:

int day = 3;

switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    default:
        System.out.println("Unknown day");
}

Here, Java checks the value of day.

Since day is 3, it prints:

Wednesday

The break stops Java from continuing into the next cases.

The default runs when none of the cases match.

6. Modern Switch

Newer Java versions allow cleaner switch syntax.

Example:

int day = 3;

switch (day) {
    case 1 -> System.out.println("Monday");
    case 2 -> System.out.println("Tuesday");
    case 3 -> System.out.println("Wednesday");
    default -> System.out.println("Unknown day");
}

This is cleaner because you do not need break.

You can also return a value from a switch.

Example:

int day = 3;

String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    default -> "Unknown day";
};

System.out.println(dayName);

This is common in modern Java code.

7. Loops

Loops are used to repeat code.

Instead of writing the same code many times, we use a loop.

Example:

Print numbers from 1 to 5

Without a loop:

System.out.println(1);
System.out.println(2);
System.out.println(3);
System.out.println(4);
System.out.println(5);

With a loop:

for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
8. for Loop

A for loop is usually used when you know how many times you want to repeat something.

Example:

for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}

This means:

Start i at 1
Keep looping while i <= 5
After every loop, increase i by 1

Output:

1
2
3
4
5

The structure is:

for (start; condition; update) {
    // code to repeat
}

Example:

for (int i = 0; i < 3; i++) {
    System.out.println("Hello");
}

Output:

Hello
Hello
Hello
9. while Loop

A while loop runs as long as a condition is true.

Example:

int count = 1;

while (count <= 5) {
    System.out.println(count);
    count++;
}

This means:

While count is less than or equal to 5, keep printing count.

A while loop is useful when you do not know exactly how many times the loop will run.

Example idea:

Keep asking user for password while password is wrong.
Keep retrying while connection has failed.
Keep reading data while there is still data.
10. do while Loop

A do while loop runs at least once, then checks the condition.

Example:

int count = 1;

do {
    System.out.println(count);
    count++;
} while (count <= 5);

The difference is that the code runs first before the condition is checked.

So even if the condition is false, a do while loop runs once.

Example:

int number = 10;

do {
    System.out.println("Runs once");
} while (number < 5);

Even though number < 5 is false, it still prints once.

11. Enhanced for Loop

The enhanced for loop is used when looping through arrays or collections.

Example:

int[] numbers = {10, 20, 30};

for (int number : numbers) {
    System.out.println(number);
}

This means:
For each number inside numbers, print the number.

Output:

10
20
30

This is cleaner when you do not care about the index.

12. break

break stops a loop.

Example:

for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        break;
    }

    System.out.println(i);
}

Output:

1
2

When i becomes 3, the loop stops completely.

13. continue

continue skips the current loop round and moves to the next one.

Example:

for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;
    }

    System.out.println(i);
}

Output:

1
2
4
5

When i is 3, Java skips printing and continues with the next number.

14. Normal Style vs Developer Shortcuts

When learning, it is good to write code clearly first.

Example:

int age = 20;

if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}

This is easy to understand.

But in real code, developers sometimes use shorter versions.

Shortcut 1: Ternary Operator

The ternary operator is a short version of if else.

Normal way:

int age = 20;
String result;

if (age >= 18) {
    result = "Adult";
} else {
    result = "Minor";
}

Shortcut:

int age = 20;
String result = age >= 18 ? "Adult" : "Minor";

Read it like this:

If age >= 18, result is "Adult", otherwise result is "Minor".

Structure:

condition ? valueIfTrue : valueIfFalse;

Use ternary when the condition is simple.

Do not use it for big logic because it can become hard to read.

Good:

String status = paid ? "Paid" : "Unpaid";

Bad:

String result = age > 18 ? isActive ? "Allowed" : "Blocked" : "Denied";

That one is too packed.

Shortcut 2: Direct Boolean Check

Normal way:

boolean isActive = true;

if (isActive == true) {
    System.out.println("Active");
}

Better developer way:

if (isActive) {
    System.out.println("Active");
}

For false:

if (!isActive) {
    System.out.println("Not active");
}

Instead of:

if (isActive == false) {
    System.out.println("Not active");
}
Shortcut 3: Early Return

Early return means you stop the method early instead of nesting many if statements.

Normal nested style:

public void processOrder(boolean paid, boolean inStock) {
    if (paid) {
        if (inStock) {
            System.out.println("Processing order");
        } else {
            System.out.println("Item out of stock");
        }
    } else {
        System.out.println("Payment required");
    }
}

Cleaner developer style:

public void processOrder(boolean paid, boolean inStock) {
    if (!paid) {
        System.out.println("Payment required");
        return;
    }

    if (!inStock) {
        System.out.println("Item out of stock");
        return;
    }

    System.out.println("Processing order");
}

This is easier to read because you handle the bad cases first, then continue with the main logic.

This is common in backend code.

Shortcut 4: Enhanced for Instead of Index Loop

Normal loop:

String[] names = {"Tino", "John", "Mary"};

for (int i = 0; i < names.length; i++) {
    System.out.println(names[i]);
}

Cleaner version:

for (String name : names) {
    System.out.println(name);
}

Use enhanced for when you just want the values.

Use normal for when you need the index.

Example where index is needed:

for (int i = 0; i < names.length; i++) {
    System.out.println(i + ": " + names[i]);
}
Shortcut 5: forEach

In modern Java, collections can use forEach.

Example:

List<String> names = List.of("Tino", "John", "Mary");

names.forEach(name -> System.out.println(name));

Even shorter:

names.forEach(System.out::println);

This is common in modern Java.

But first understand normal loops before jumping too much into this.

Shortcut 6: Modern Switch Expression

Normal old switch:

String role = "ADMIN";
String access;

switch (role) {
    case "ADMIN":
        access = "Full access";
        break;
    case "USER":
        access = "Limited access";
        break;
    default:
        access = "No access";
}

Modern switch:

String role = "ADMIN";

String access = switch (role) {
    case "ADMIN" -> "Full access";
    case "USER" -> "Limited access";
    default -> "No access";
};

This is cleaner and reduces mistakes with missing break.
```

## interfaces

its a blueprint for creating classes,
cant instantiate, rather you shoukd impliment it
types

1. Normal interface
   this can have multiple methods, and its a contract if you impliment me, you have to provide the methods too

2. Functional Interface
   this one is intended to have 1 method only and usually annotated with
   @FunctionalInterface

##lambda
is a short way of implimenting a functional interface,instead of creating a whole class, then instantiating it just to call one method. use lambda
anatomy

```java
Greeting greeting = new Greeting(){
    @Override
    public void sayHello(){
        system.out.println("hello")
    }
}
normal way of impl an interface with sayHello

with lamda it becomes

Greeting = () -> {
    system.out.println("hello");
}

of if theres no parameters you can skip the {} and it becomes,
Greeting = () -> system.out.println("hello");

```
