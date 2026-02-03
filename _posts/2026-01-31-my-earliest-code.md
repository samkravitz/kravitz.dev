---
layout: post
title: My Earliest Code
date: 2026-01-31
categories: programming
---
I was going through an old drive and came across a folder with some projects containing code I wrote over a decade ago. These Java programs are dated October 6th, 2015, and written for the Computer Science course I took in 11th grade. This isn't the first code I've written, which would have been a good 6-9 months prior to this, but those earliest programs are probably lost to time. I'm posting these untouched Java programs as they were in 2015 for the enjoyment of all. No code formatters, no problems. Good times.

### Program 1 - RemainderDivide
#### Reimplementation of the modulo operator?

{% highlight java %}
package remainderdivide;

/**
 * Sam Kravitz block 7
 * AP CS P. 146 project 4-1
 * 
 */
import java.util.Scanner;

public class RemainderDivide {
    
    public static void main(String[] args) {
        int num1;
        int num2;
        int bigNum;
        int littleNum;
        int quotient;
        int remainder;
        
        Scanner reader= new Scanner(System.in);
        System.out.print("Enter the first number: ");
        num1= reader.nextInt();
        
        System.out.print("Enter the second number: ");
        num2= reader.nextInt();
        
        if (num1 >= num2){
            bigNum= num1;
            littleNum= num2;
        }else{
            bigNum= num2;
            littleNum=num1;
                    }
        
        quotient= bigNum/littleNum;
        remainder= bigNum % littleNum;
        
        System.out.println("The quotient of the numbers with a remainder is " + 
                quotient + " R " + remainder);
        
    }
} 
{% endhighlight %}

### Program 2 - RightTriangle
#### Determines if 3 given side lengths form a right triangle or not.

{% highlight java %}
package righttriangle;
/*
 * Sam Kravitz Block 76
 * AP CS P. 146 Project 4-2
 */

import java.util.Scanner;

public class RightTriangle {

    public static void main(String[] args) {
        
        int sideA;
        int sideB;
        int sideC;
        
        Scanner reader= new Scanner(System.in);
        System.out.print("Enter the length of side A: ");
        sideA= reader.nextInt();
        
        System.out.print("Enter the length of side B: ");
        sideB= reader.nextInt();
        
        System.out.print("Enter the length of the Hypotenuse: ");
        sideC= reader.nextInt();
        
        if (sideC == Math.sqrt((sideA * sideA) + (sideB * sideB))){
            System.out.println("A triangle with the side lengths " + sideA + ", "
            + sideB + ", and " + sideC + " is a right triangle.");
        }else{
            System.out.println("A triangle with the side lengths " + sideA + ", "
            + sideB + ", and " + sideC + " is not a right triangle.");
        }
    }
}
{% endhighlight %}

### Program 3 - PopGrowth
#### Some sort of "Conway's Game of Life" simulation.

{% highlight java %}
/*
 * Sam Kravitz Block 7
 * AP CS P.147 Project 4-6
 */
package popgrowth;

import java.util.Scanner;

public class PopGrowth {

    public static void main(String[] args) {
       int organisms, growthRate, rateHours, growHours;
       
       Scanner reader= new Scanner(System.in);
       
       System.out.print("Enter # of organisms: ");
       organisms= reader.nextInt();
       
       System.out.print("Enter growth rate: ");
       growthRate= reader.nextInt();
       
       System.out.print("Enter # of hours required to acheive growth rate: ");
       rateHours= reader.nextInt();
       
       System.out.print("Enter # of hours that the population can grow: ");
       growHours= reader.nextInt();
       
       if (growthRate <= 0)
           System.out.print("The growth rate needs to be positive!");
       else{
           if(rateHours > growHours)
               System.out.print("The population needs more time to grow!");
           else{
           organisms = organisms * growthRate * growHours;
           System.out.print("After " + growHours + " hours, the population"
            + " has reached " + organisms + " Organisms!");
               }
       
           }
    }
}
{% endhighlight %}

### Program 4 - JavaApplication13
#### Beautiful stuff here. Determines the total cost of an inventory of items given their unit price, tax rate, and markup.

{% highlight java %}

 /* SAM KRAVITZ BLOCK 7
 * AP CS P. 181 PROJECT 4
 */
package javaapplication13;

import java.util.Scanner;

public class JavaApplication13 {

    public static void main(String[] args) {
        Scanner reader= new Scanner(System.in);
        double markup= 0.1;
        double bookPrice = .25;
        double totalPrice=0;
        double totalTax= 0;
        
        
        while (bookPrice != 0){
            System.out.println("Enter a book price. 0 to exit. ");
            bookPrice= reader.nextDouble();
            System.out.println("This book costs " + bookPrice + ". But with tax it costs " + (bookPrice + bookPrice * markup));
            bookPrice = bookPrice + (bookPrice * markup);
            totalPrice += bookPrice;
            totalTax +=  (bookPrice * markup);
               
            
        
        }
        System.out.println("The total amount of books purchased is " + " $" + totalPrice + ". The total amount of"
                  + " tax on those books is" + " $" + totalTax );
    }
    
}

{% endhighlight %}
