# C project - library database management system

1. [Summary](#1-summary)
2. [Implementation Detais](#2-implementation-details)
3. [Showcase](#3-showcase)

## 1. Summary

This project aims to create a data management system that includes the salary of a person along with the additional information (such as the amount of money the person spends on self-education) and performs analysis of the data entered into the database. Analysis of the existing data in the database is implemented by a linear regression algorithm. It models the relationship between a dependent variable (salary) and an independent variable (spending).

The application implenets command-line user interface, allowing end-user to add, view, analyze and delete data. 

## 2. Implementation details

My program starts with the menu, that promtps user to select a command to execute: 

1. Add data
2. View data
3. Analyze data
4. Delete data
0. Exit

The user is required to enter a number from 0 to 4, and based on the choice, and the program will perform an appropriate action. 

### 2.1. Add data

When you add data, it will ask you to provide:

| Name          | Description                                          | 
| ------------- |:-----------------------------------------------------|
| **id**        | integer number, uniquely identifying a record        |
| **age**       | age of a person                                      |
| **salary**    | salary of a person                                   |
| **spending**  | amount of money person spends on self-education      |

In order to add a new record into the database, a user must select the "Add data" command. Then on-screen instructions guide the user through the process of adding a record.

All of the following fields are required to be filled in.

**Id** is an integer number, uniquely identifying a record in the database. It must be greater than 0 and less than 9,999. Further operations can be performed on existing records by id.

**Age** is validated, and it must be from 18 to 70. Since 18 is the minimum age to start work in many countries legally. Also, I decided that in this system, I need to limit the data only for 70-year-old people or younger.  

The **salary** also is validated at the time it's entered into the system. Since we may have young people in our dataset, it can be a minimum paycheck - $15,000. For this example, I don't want to focus on outliers data, so a maximum salary can not be more than $300,000 in this system.

Also, **spending** cannot be less than 0 and more than max salary, which must be intuitive in this situation. 

### 2.2. View data

View data function just shows all records from the program from the database in an order of creation of those records.

### 2.3. Analyze data

At this time I have implemented a linear regression algorythm to analyze data in the database.

I will be enhancing this part of the program in the future by implementing other algorythms for more advanced data analysis.

For example, I need to predict my salary (dependent variable, denoted as y). All that I know it's the salaries of other people and how much they spend on self-education (independent variable, denoted as x), for example, Coursera, Edx, Linda courses.  I want to know my potential compensation if I spend $1000 on self-education? Or, another situation, I want to earn $100000; how much I need to spend on some activities? 

### 2.3.1. Mathematical model

The mathematical equation of linear regression is:

`y = b0+b1*x`

Here:
- x - Independent variable
- y - Dependent variable
- b0 - Intercept
- b1 - Slope

For our problem, I'll use this equation for b1:

<img src="https://render.githubusercontent.com/render/math?math=b_1 = \sum_{i=1}^{m} (X_i - X_{mean})(Y_i - Y_{mean})/ \sum_{i=1}^{m} (X_i - X_{mean})^2">

And other that is show below for b0:

<img src="https://render.githubusercontent.com/render/math?math=b_0 = Y_{mean} - b_1 X_{mean}">

In code, I have implemented this general equation in a following way: 

`b1 = (n*sum_XY-sum_X*sum_Y)/(n*sum_X*sum_X-sum_X*sum_X);`

`b0 = (sum_Y-b1*sum_X)/n;`

The main result will be produced as the coefficients b0 and b1. Also, it will show equation of best fit.

### 2.4. Delete data

In this function, you can choose an ID of a record that you would like to delete, and this function will remove it from the system. 

## 3. Showcase

Some examples of my inputs are:
```
id = 1; 
age = 18;
salary = 50000;
spending = 1000;
```

... 

```
id = 4; 
age = 35;
salary = 100000;
spending = 1000;
```

An output that for my sample data set was calculated as: 

```
Coefficient b0 is 553.60 and b1 is 107.00
Equation of best fit is: y = 553.59 + 107.00 x
```

As you can see, linear regression helps us make predictions and show the relationship between two variables or factors. In general, our program is an example of a good tool to add, view, analyze and delete data and make some operation on it. I will continue to explore and add different algorithms and methods to this database system in the next parts.
