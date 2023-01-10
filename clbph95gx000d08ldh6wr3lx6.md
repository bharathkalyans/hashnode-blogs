# Clean Code Practices in Java

### What is Clean Code?

Clean Code is a practice of writing simple and easily understandable code.Clean code enables the developers to understand the code written by others without any hassle.

*   Let's start directly with code snippets.
    
    ## Naming of Variables
    

```java
public class Sample {

    public static void main(String[] args) {
        Scanner obj = new Scanner(System.in);
        int gh = obj.nextInt();
        float pi = 3.14f;
        System.out.println("Radius is :: " + pi * gh * gh);

    }
}
```

*   Take a look at this code which does the same job.
    

```java
public class Sample {

    private static final float PI = 3.1432f;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int radius = scanner.nextInt();
        System.out.println("Radius is :: " + PI * Math.pow(radius, 2));

    }
}
```

*   Now you might argue what is the difference? 🧐 You just changed a couple of variable names and that's it.
    
*   The naming convention itself is one of the most important clean code aspects.
    
*   Now any developer can read and understand the code easily.
    
*   Though the example might be simple, it makes a huge difference when you are working on large code bases that contain hundreds of variable names and functions.
    
*   Always use proper naming for variables, classes, interfaces and packages.
    

> > Developers spend 10% of their time coding and the remaining 90% understanding the WTF the code is doing. 😮‍💨

## DRY

```java

public class Sample {

    public static void main(String[] args) {
        int ageOfJohn = 32;
        int ageOfSmith = 32;

        //Let's swap these two ages;
        int tempAge = ageOfJohn;
        ageOfJohn = ageOfSmith;
        ageOfSmith = tempAge;

        int ageOfCatherine = 32;
        int ageOfJasmine = 32;

        //Let's swap these two ages;
        int tempAge2 = ageOfCatherine;
        ageOfCatherine = ageOfJasmine;
        ageOfJasmine = tempAge;

    }

}
```

*   Always try to extract the common code into functions that make it easier to read the code and reduce the number of lines in your project.
    
*   The above code can be rewritten as
    

```java

public class Sample {

    public static void main(String[] args) {
        int ageOfJohn = 32;
        int ageOfSmith = 32;

        //Let's swap these two ages;
        swapAges(ageOfJohn, ageOfSmith);

        int ageOfCatherine = 32;
        int ageOfJasmine = 32;

        //Let's swap these two ages;
        swapAges(ageOfCatherine, ageOfJasmine);

    }


    public static void swapAges(int ageOne, int ageTwo) {
        int tempAge = ageOne;
        ageOne = ageTwo;
        ageTwo = tempAge;
    }
}
```

## Method Parameters

*   Let's take a look at a sample code.
    

```java

import java.util.Date;

public class Sample {

    public static void main(String[] args) {

    }

    public static void saveOrderInformation(String customerName,                                             Long customerId,  Integer customerPhoneNumber,                           Long orderId, String productName,Date billingDate) {
        //....
    }

}
```

*   If you take a look at the above code we can see there are multiple parameters being used as arguments.
    
*   You must always avoid creating methods like these.
    
*   Limit to passing 3 - 4 parameters!
    
*   To make it cleaner we can use objects as parameters as shown below.
    

```java

import java.util.Date;

class Customer {
    Long customerId;
    String customerName;
    Integer customerPhoneNumber;

    Customer() {

    }
}

class OrderDetails {
    String productName;
    Date billingDate;

    OrderDetails() {

    }
}

public class Sample {

    public static void main(String[] args) {
        Customer customer = new Customer();
        OrderDetails orderDetails = new OrderDetails();
        saveOrderInformation(customer, orderDetails);
    }

    public static void saveOrderInformation(Customer productName,
                                            OrderDetails billingDate) {
        //....
    }

}
```

## Project Structure

*   I declared multiple classes in a single class which is the wrong way of doing things.
    
*   Though it works, never try to implement it this way.
    
*   You can use inner classes if needed for certain implementations but try to segregate these classes with proper `packages`.
    

## Commenting

*   Avoid unnecessary commenting on your code.
    
*   You should write code in such a way that, the code must be self-explanatory.
    

```java
import java.util.Scanner;


public class Sample {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        //Size of array
        int n = scanner.nextInt();
        //Declaring array
        int[] ar = new int[n];
        //Taking input 
        for (int i = 0; i < n; i++) {
            ar[i] = scanner.nextInt();
        }

        System.out.println("Sum = " + add(ar));
    }

    //Function that adds an array
    private static int add(int[] ar) {
        //Variable that stores the result
        int sum = 0;
        for (int i = 0; i < ar.length; i++) {
            sum = sum + ar[i];
        }
        return sum;
    }


}
```

*   The below code shows a clear difference in how using correct `naming`, `spacing` and `indentation` can make the code cleaner and more readable.
    

```java
import java.util.Arrays;
import java.util.Scanner;

public class Sample {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int size = scanner.nextInt();
        int[] array = new int[size];
        for (int i = 0; i < size; i++) {
            array[i] = scanner.nextInt();
        }

        System.out.println("Sum = " + sumOfArray(array));
    }

    private static int sumOfArray(int[] array) {
        int totalSum = 0;
        for (int number : array) {
            totalSum = totalSum + number;
        }
        return totalSum;
        //Using streams -> return Arrays.stream(array).sum();
    }

}
```

*   The above example might be small but when you are working on production it makes a huge difference!
    

### Time to wrap up ‼️

1.  Name variables, classes and interfaces accordingly.
    
2.  Don't Repeat the block of code which can be extracted into a function.
    
3.  Structure your project with packages.
    
4.  Avoid unnecessary commenting.
    
5.  Use whitespaces between the lines of your code.
    
6.  Indent your code properly and give spaces between the variables and where ever applicable.
    
7.  Use a private modifier if it is a helper function.
    
8.  Use Streams and Collection frameworks if you are familiar with them. It is going to save you a bunch of lines!
    
9.  Don't try to use this type of bracket convention. It can be personal preference but I usually don't follow this practice!
    

```java

public class Sample 
{

    public static void main(String[] args)
    {
        
    }
    
    public static void greetUser()
    {
        System.out.println("Hello User!");
    }
    
}
```

The best resource I would suggest to learn more about Clean Coding and practices would be to read this book called `Clean Code by Robert C. Martin` and also look at the open-source projects that are available freely on GitHub.

Hope you liked this blog and if you have any queries or discrepancies in this blog let me know in the comment section! Till then happy coding!🥳👋🏼