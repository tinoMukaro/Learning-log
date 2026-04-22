###### java uses clases every code stays inside a class

methods are used to hold different code sections

class name should be same as files name else it wont run

to take use input use Scanner which is imported from the java.util class

printf is used to format text inside a print statement

it consist of % flags width .precision and specifier characters

specifier characters

%d for int

%f for double

%s for string

%b for Boolean

string methods these are actions you can do with strings

just type your string name.

in IntelliJ it will list all the methods you can do

###### substrings

this is a method use to capture part of a string

.substring(start, end)

it takes a start and an end index

you can use other methods inside it for flexibility

###### **ternary operator**

variable = condtion ? iftrue : iffalse

###### enhanced switches

switch(var){

case "something " -> do this;

case "something " -> do this;

case "something " -> do this;

default -> do this if it doest fit all the case

it works to stop using repeated if else

###### methods in java

a method is a block of code that is executed when called()

return type name(){

&nbsp; your code here

}

in java you have to specify the data type of the parameters

method overloading

methods in java can have same name but with different parameters that's what called method overloading

signature is name + parameters

###### **variable scope**

so a variable defined in a method is only known and accessible from that method, other methods are not aware of it

that why we use parameters and arguments to pass variables to methods

class variables which are defined at class level are known and accessible from all the methods

**arrays**
you have to specify data type, in java arrays contain data of the same types
String[] names = {"tion, "taku", "tinashe"};

to declare an empty array you have to seet the size of the amounts its gonna carry arrays are fuckng rigid in java
String names = new String[size];
so to allow for flexibility size should be a variable which user have to input as they are the one who know what they want to input

import java.util.Scanner;

import java.util.Scanner;

public class Main {
public static void main(String[] args) {
Scanner scanner = new Scanner(System.in);

        String[] names;
        int size;

        System.out.println("how many elements do you want to enter");
        size = scanner.nextInt();
        scanner.nextLine();

        names = new String[size];

        for(int i = 0; i< names.length; i++){
            System.out.print("enter something");
            names[i] = scanner.nextLine();

        }

        for(String name: names){
            System.out.println(name);
        }

    }

}

**varags**
these give us the ability to put varuable arguments and make method overloading easy no need to create a lot of overloaded methods

static return_type method_name(datatype...name){
code
}

simple method to return average of given numbers:

static double average(double...numbers){
double sum = 0;
for(double number : number){
sum += number;
}
return sum/numbers.length;
}

**object oriented programming**
everything in java is an object like real world objects
the have attrbutes like name size etc and can perform actions (methods)
a class is a blueprint to create an object

public class Car {

    //attributes
    String name = "raptor";
    String make = "ford";
    int year = 2025;

    //methods
    void drive(){
        System.out.printf("you drive a %s model %s year %d", name, make, year);
    }

}
the problem here is we can create objects out of this class ok but they all will have the same attributes and methods, we cant customise them. that why we need constructors these are specia methods that allow us to create unique objects from the same blueprint

a ###### constructor has the same name as the class name, it can have any arguments or you can set default arguments

public class Car {

    //attributes
    String name;
    String make ;
    int year;

    //methods
    void drive(){
        System.out.printf("you drive a %s model %s year %d", name, make, year);
    }

    //constructor
    Car(String name,String make, int year){
        this.name = name;
        this.make = make;
        this.year = year;

    }

}

this = is a key word that specifies that for this variable use the value passed from usage or whatever
this refers to the current object we are working on

**overloaded contructors**
yes contructors can be overloaded, this allows a class to have a lot of constructors well with different numbers of arguments
enables an object to be initialised in various ways

###### static keyword

makes a variable or method belong to the class
rather to any object
commonly used for utility methods or shared resources

##### inheritance

allows a class to inherit methods and attributes from another class
uses the extends key word
// example
public class Pajero extends Car{

    Pajero(String name, String make, int year) {
        super(name, make, year);
    }

}
super referes to the parent class,
since the parent class car has got a contructor which require us to pass arguments the child contructor uses super keyword
tell the super class to pass the given values to the constructor.
it calls the parent constructor to initialise the object

#### method overridding

is a way in which a child class can overide or modify a parent method
in our car example the pajero inherits from car but the pajero is dead so the drive method doesnt work here
we have to modify it so that you cant drive a pajero even though its a car.

public class Pajero extends Car{

    Pajero(String name, String make, int year) {
        super(name, make, year);
    }
    @Override
    void drive(){
        System.out.println("you cant drive the pajero the filter pump is not working");
    }

}

@override is used to show that youre overriding a certain method, also it you have a naming error in your overriding process it will tell.

abstraction
used to abstract classes and methods
used to hide implimentation details and only showing only essential features
abtracted classes cant be intantiated directly
can contain abtracted methods -> which must be impimented
can contain concerete classed -> which can be inherited

in short an abtract class is used if you want to secure your code so that it only defines what to be done then the sub classes will
do the implimentation same thats the abstraction key word
to create one use abstract class

public abstract class Pajero {

    abstract void area(); //abstract class you have to impliment/override it when creating a subclass of it, doesnt have a body, cant be inherited

     void whatAmI(){ // concrete class, can be inherited
         System.out.println("your a shape");
     }

}

#### interfaces

an interface is a blueprint for a class that specifies abtract methods that implimenting classsess must define
supports multiple inheritance like behavior

uses impliments keyword
public interface Prey{
void flee();
}
public class Jagaur impliments Prey{
by default if its implimenting the Prey interface it should override the flee method

    @override
    void flee(){

    }

}

###### polymophism

objects can identifiesd as other objects

### getters and setter

these are methods that can make objects accessible
getter - methods that make a field readable
setter - methods that make a field writtable

if you make variables private so that they cant be accesible or changeable
you can use stter and getter so that someone can modifie them but only in the way you want

public class Car {

    //attributes

private String name;
private String make ;
private int year;

    //constructor
    Car(String name,String make, int year){
        this.name = name;
        this.make = make;
        this.year = year;

    }

    String getName (){
        return this.name;
    }
    String getmake (){
        return this.make;
    }
    int getYear (){
        return this.year;
    }

}

so in the main function when using them we call the methods instead of the variables directly well because we cant even access them
public class Main {
public static void main(String[] args) {
Car car = new Car("ford", "raptor", 2025);

        System.out.println(car.getName());
    }

}

simmilar to setter functions in the main function you can only set the variable using the set method and it only works if it souits the
method requirements else you cant access or modify the variable

    void setName(String name){
        this.name = name;
    }
    void setMake(String make){
        this.make = make;
    }
    void setYear(int year){
        this.year = year;
    }

    you can add a lot of conditions and tweekss to these functions

#### aggregation

#### composition

#### warpper classes

#### arrayLists

a resixable array that stores objects(autoboxing)

ArrayList list<specify type> name = new ArrayList<>();
types are the wrapper classes since we are dealing with objects
Integer, Double, String, Boolean

array lists got a lot of methods to work with

add() - to add new staff
set() - to set a value on a certain possition
get() - to get an element at a given position
size() - size of the array
collections.sort() - to sort an array

##### exception handling

put your dangerous code in a try block and if it fails the catch will hanle the error gracefully
try{
code to run
}catch( arithmeticException e){
what to do in case of error
}

then finally{
this is optional
you use it to close all resourse because it always run wheater theres an exception or not.
}

#### writting and reading files in java

**write**
there are 4 main techniques:
FileWritter - good for small or medium sized text
BufferWritetr - better perfomance for large amounts of data
PrintWritter - best for structured data like reports or logs
FileOutputStream - best for binary files eg images, audio

using the fileWritter
try(FileWriter writer = new FileWriter("text.txt")){
writer.write("i like java");

     } catch (Exception e) {
         System.out.println("could not write file");
     }

**read**
there are 3 main techniques:
BufferReader + fileReader - best for reading text files line by line
file input stream - beast for binary files
RandomaccessFIle - best for read and write specific portion of large file

using buffer reader

     String filePath = "C:\\Users\\tinot\\OneDrive\\Desktop\\learn java.txt";


        try( BufferedReader reader = new BufferedReader(new FileReader(filePath))){
            String line;
            while((line = reader.readLine()) != null){
                System.out.println(line);
            }

        }catch (FileNotFoundException e){
            System.out.println("file not found");
        }catch(IOException e){
            System.out.println("something went wrong");
        }

#### annonymous classes

these are nameless classes that are used to modify an object for custom behavour
like in the following example the car object has a normal driveCar method but the raptor has to overide it so we modify
we could create a class that inherit and then modify but its too much work rather use an annonymous class
to just mod the objecct zveipapo

the syntax, just create an object and add {} the inside do the jugglings.

        Car car = new Car("mitsubish","pajero", 2001 );

        Car car1 = new Car("Ford","raptor", 2025){
            @Override
            void driveCar(){
                System.out.println(name + " " + "aya madamburo");

            }
        };

        car1.driveCar();
        car.driveCar();

### timer and timer classes

### generics

a concept where you write a class, interface, or method that is compatible with different data types
<T, U, V> type parameter (placeholder that gets replaced with a real type) you can add more ethan one type
<String> this is a specific argument

public class Box <T>{
T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem(){
        return this.item;
    }

}

##### hashmaps

a hashmap is a datastructure that store key pair values, keys should be fucking unique
Hashmap<K, V> map = new Hashmap<>();
k and v are type parameters they specifie the type of key pair value we are gonna store in the hashmaps
just like in generics
example
Hashmap<String, Double> map = new Hashmap<>();

map.put("apple", 0.50);
// trying to put a different object on a key that already exist it will override the previous object
map.remove("key"); - removes the value at the specified key
map.get("key"); - this will return a value at the specified key
map.containsKey(); - check if a key exist
map.containsValue(); - check if a value exist

#### enums

#### threads

these allow a program to run multi taskes simultaneously
how to create a thread

1. extend the thread class
2. impliment the Runnable interface

to impliment the runnable class create a class then impliment the runnable class
and override the run method

public class MyRunnable impliments Runnable{

    @override
    public void run(){

        enter code you want to run
    }

}

in the main class create a runnable object

MyRunnable runnable = new MyRunnable();
Thread thread = new Thread(runnable);
thread.start();

a daemon thread is a thread that ends when the main thread ends
to set a daemon thread

**_thread.setDaemon(true)_**

## lambda functions

## annotations
