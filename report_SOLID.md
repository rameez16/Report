# Report on SOLID design principles


#### Date  : 11-8-2021
#### Prepared by  : MUHAMMED RAMEES
  
## 1. Introduction
<hr>

A well-written code should be stable,reusable,extendable,logical,easier to read and test. How to device a good software design so that the code posses above mentioned qualities?. SOLID design principle effectively answer this question by coming up with five good design methods that make the code more maintainable ,flexible,testable,extendable and re-usable.Following these principle will save both time and effort in maintaining and managing code. Best benefit of SOLID based design is that we can modify and extend  the codebase without impacting and breaking other part of the code. So let us look at each of the 5 principles in detail.Basic understanding of Object Oriented Design concept is necessary to follow this report.   




## 2. S.O.L.I.D 
<hr>

SOLID design principles were introduced by Robert C. Martin in his paper “Design Principles and Design Patterns” published in 2000 . SOLID architecture gained popularity  after its introduction and developers began to follow these principles to design their programs.Now it has become coding standard in programing. SOLID acronym means the following.

> ### 1.  S: Single Responsibility Principle
> ### 2. O: Open-Closed Principle
> ### 3. L: Liskov Substitution Principle
 > ### 4. I: Interface Segregation Principle
 > ### 5. D: Dependency Inversion Principle

<HR>


## 2.1. single responsibility principle (SRP)

 > ### Each class should have one responsibility, one single purpose / A class should have one, and only one, reason to change.

As the name suggests the class , method or function must be designed to handle single task only.Another way of describing it would be class or function should have only one reason to change. When a class serves multiple purposes or responsibilities, it should be made into a new class.The ultimate goal of the class should be to serve single purpose only. 


Have a look at the class given below. The class 'UserSetting clearly violate the SRP principle because it handle multiple responsibility. It contains two function doing different task,one being change Setting and another verify credentials .So its a clear violation of single responsibility principle.

```javascript 
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```
How to rewrite above class to follow SRP principle. Solution is simple and straightforward. Split the class into two, each doing single task.

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

Examine the above code. The class UserSetting has been disintegrated into two classes.      

1. UserAuth-responsible for verifyCredentials
1. UserSetting -responsible for change settings.
  
   

Benefits of implementing SRP are
* Easier to understand
* Easier to maintain
* More re-usable
 

## 2.2 Open-close principle(OCP)


> ###  The class/functions is open for extension but closed for modification
What does this statement actually imply to?. According to this principle we are allowed to extend a class for enhanced functionality,but we are never allowed to modify it. Any kind of modification on the existing class is prohibited.we cant touch existing code. What to do if we want to update the class for better functionality or we want to add a new requirement by strictly following Open-Close Principle?. It should be achieved by adding new classes , methods and attributes without modifying existing class.


Lets understand this better by an example. Consider the following code. Here the developer has created a class that calculate interest for All savings account based on type of account,they are the following.

1. regular 
2. salary 
  
  
  

```JAVA Public class SavingAccount  
{  
     
    Public decimal CalculateInterest(AccountType accountType)  
    {  
        If(AccountType==”Regular”)  
        {  
            
            Interest = balance * 0.4;  
            If(balance < 1000) interest -= balance * 0.2;  
            If(balance < 50000) interest += amount * 0.4;  
        }  
        else if(AccountType==”Salary”)  
        {  
             
            Interest = balance * 0.5;  
        }  
    }  
}  
```
Now bank has revised its requirement and want to add one more account type named Student. One way to do this is to modify the current class and add one more if-else statement for student type. But this is a direct violation of OCP principle. We will be modifying the class here, which is not allowed. So how to re-design the code so that it obey OCP principle. Have a look at the classes given below.

```JAVA Interface ISavingAccount  
{   
   decimal CalculateInterest();  
}  
Public Class RegularSavingAccount : ISavingAccount  
{  
  
  Public decimal CalculateInterest()  
  {  
 
    Interest = balance * 0.4;  
    If(balance < 1000) interest -= balance * 0.2;  
    If(balance < 50000) interest += amount * 0.4;  
  }  
}  
  
Public Class SalarySavingAccount : ISavingAccount  
{  
    
  Public decimal CalculateInterest()  
  {  
   
    Interest = balance * 0.5;  
  }  
}  
```

Here instead of using if else statements we have created separate class for each Account type. Now ,if the bank needs to add new account type say Student,developer can add a new class 'Student' and achieve the requirements of the bank. This is done without modifying existing class,he just extended the class for new functionality. Now the code obeys OCP principle. So, at any given point of time when there is a requirement change instead of touching the existing functionality it’s always suggested to create new classes and leave the original implementation untouched.

What are the consequences of not following ocp?

1. Testing become tedious because along with new feature we end up testing all the 
   modified classes and function again.

2. Not following OCP brakes SRP.
 
3. Maintenance become  difficult since we are adding more logic to existing class. 
   
## 2.3 Liskov’s Substitution Principle (LSV)

  > ### Derived or child classes must be substitutable for their base or parent classes

Let us suppose  inherit class A to class B. Class A is the parent and class B is the childe.Here the derived class(class B) should be able to replace class A (parent)  without displaying any unexpected behavior. This is the underlying idea behind this principle.

Consider the below given example,here,the class Apple should able to replace class orange where ever it is used and it should work just fine. But here the result will be not at all what is expected, when we call the method get colour for object 'orange' made using 'apple' class and the color returned is red,which is incorrect!    



```JAVA 

  
     public class Orange 
{
  
    public override string GetColor()
    {
        return "Orange Color";
    }
}

public class Apple : Orange
{
    public override string GetColor()
    {
        return "Red color";
    }
}


        
class Program
{
    static void Main(string[] args)
    {
        Apple orange = new Apple();

        Console.WriteLine(orange.GetColor());

       
    }
}

 ```

To modify the above code to follow LSV, we can use a abstract class called fruit.Now, even if we replace the parent class for the child we can achieve expected result.          


           
 ```java



     public abstract class Fruit
{
    public abstract string GetColor();
}

public class Orange : Fruit
{
    public override string GetColor()
    {
        return "Orange Color";
    }
}

public class Apple : Fruit
{
    public override string GetColor()
    {
        return "Red color";
    }
}

class Program
{
    static void Main(string[] args)
    {
        Fruit fruit = new Orange();

        Console.WriteLine(fruit.GetColor());

        fruit = new Apple();

        Console.WriteLine(fruit.GetColor());
    }
}
  ```



## 2.4 Interface Segregation Principle(LSP)

 Interface segregation principle states:

> ### A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use

Let us better understand above statement with an example. Consider the code given below. Developer created a class ISmartDevice which have the interface for print,fax and scan.

```JAVa 

interface  ISmartDevice
{
    void Print();

    void Fax();

    void Scan();
}
 ```
 lets implement the class in a all in one printer,it work just fine.



``` JAVA                   
   class AllInOnePrinter : ISmartDevice
{
    public void Print()
    {
         // Printing code.
    }

    public void Fax()
    {
         // Beep booop biiiiip.
    }

    public void Scan()
    {
         // Scanning code.
    }
}          
  ```

 Now the client comes with an Electronic printer which has ony Print functionality. Fax and Scan functionality is not there in the printer. Let us see what happens  when we inherit the Electronic Printer class  from  ISmartdevice class. Observe the code below.

 ```JAVA 
         class EconomicPrinter : ISmartDevice
{
    public void Print()
    {
        //Yes I can print.
    }

    public void Fax()
    {
        throw new NotSupportedException();
    }

    public void Scan()
    {
        throw new NotSupportedException();
    }
}
```
   Here the client is forced to implement functions that he never use.So this is a violation of ISP. A better way of design is given below.

 ```JAVA
interface IPrinter{
    void Print();
}

interface IFax{
    void Fax();
}

interface IScanner{
    void Scan();
}  
```
Here each interface is separated. This make the reuse more flexible and client  do not have to worry about implementing irrelevant logic in the design. So its always better to create small interfaces with single functionality rather than creating big interface with lots of methods. Doing this way  make the code more easier to maintain and  provides improved flexibility.


## 2.5. Dependency Inversion Principle (DIP)

Statement for this principle can be written as :

>  ### High-level modules should not depend on low-level modules. Both should depend on abstractions.Abstractions should not depend on details. Details should depend on abstractions.


To understand above statements, let's  understand what is high level and low level module.

* High level module-A high-level module is a module that always depends on other modules
* Low level module- a module that does not depends on other modules

According this principle High-Level modules and low level modules should be loosely coupled or de-coupled.both class shouldn't depend on each other. The main idea of this principle is decoupling the dependencies. So if class A changes the class B doesn’t need to care or know about the changes.


```java

     class MySQLConnection
{
    public function connect()
    {
        // handle the database connection
        return 'Database connection';
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}
    
   ```


Here the class Passwordreminder is a High-level module because it depends on the low level module- MySQLConnection. Here PasswordReminder class is coupled to MySQl connection for proper functioning.
This violates  Dependency Inversion Principle (DIP).


 To address these issues, we can re-implement the class using an interface since high-level and low-level modules should depend on abstraction.



```JAVA
interface DBConnectionInterface
{
    public function connect();
}
```




 
 Let us create an interface named **DBConnectionInterface** with a connect() method. MySQLConnection class implements this interface.now we can avoid using MySQLConnection directly in the class **PasswordReminder**,we can use DBConnectionInterface instead. So this code establishes that both high-level module and low level module depends on abstractions and they do not depend on each other.


```JAVA
          class MySQLConnection implements DBConnectionInterface 
{
    public function connect()
    {
        // handle the database connection
        return 'Database connection';
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}

 ```

 

 ## 3. CONCLUSION
 
 This report makes clear that implementation of SOLID principles are great choice for good software design. Projects adhering to solid principles can be easily extended, tested and maintained,it can be made to effortlessly adapt to changing requirements. Main goal of SOLID type design is  de-coupling the classes and methods so that inter dependency is significantly reduced. To do so we might have to write lengthier code, but once its deployed after production,  it saves lot of time and effort during maintenances and extension. 



 ### Reference

* https://www.educative.io/edpresso/what-are-the-solid-principles-in-java
* https://www.edureka.co/blog/solid-principles-in-java/
* https://blog.logrocket.com/
* https://solid-principles-single-responsibility-in-javascript-frameworks/
* https://levelup.gitconnected.com/javascript-clean-code-solid-9d135f824180
* https://thefullstack.xyz/solid-javascript
* https://khalilstemmler.com/articles/solid-principles/solid-typescript/
* https://stackoverflow.com/questions/56860/what-is-an-example-of-the-liskov-substitution-principle
* https://reflectoring.io/lsp-explained/
* https://blog.ndepend.com/solid-design-the-liskov-substitution-principle/
* https://springframework.guru/principles-of-object-oriented-design/dependency-inversion-principle/
* https://medium.com/@kedren.villena/simplifying-dependency-inversion-principle-dip-59228122649a
* https://www.youtube.com/watch?v=mzvcONnKqmE
* https://www.c-sharpcorner.com/UploadFile/pranayamr/open-close-principle/
* https://dotnettutorials.net/lesson/dependency-inversion-principle/
* https://www.toptal.com/software/single-responsibility-principl
 




