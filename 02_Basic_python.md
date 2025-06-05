# Basic Python
1. Python Syntax.

   Python syntax can be executed by writing directly in the Command Line:
   ```console
   >>> print("Hello, Everyone!")
   Hello, Everyone!
   ```
   Or by creating a python file on the server, using the .py file extension, and running it in the Command Line:
   ```console
   >> python myfile.py
   ```
2. Python Comments.

   Comments starts with a #, and Python will ignore them. For example make a simple python file (test.py) and type this example!
   ```console
   #This is a comment
   print("Hello, Everyone!")
   print("Hello, You!") #This is a comment
   ```
   Execute this script!
   ```console
   >> python test.py
   Hello, Everyone!
   Hello, You!"
   ```
3. Python Variables.

   Variables are containers for storing data values. Python has no command for declaring a variable. A variable is created the moment you first assign a value to it.
   ```console
   x = 7
   y = "Paijo"
   print(x)
   print(y)
   ```
   Variables do not need to be declared with any particular type, and can even change type after they have been set.
   ```console
   x = 2        # x is of type integer
   x = "Painem" # x is now of type string
   print(x)
   ```
   If you want to specify the data type of a variable, this can be done with casting.
   ```console
   x = str(7)    # x will be '7'
   y = int(7)    # y will be 7
   z = float(7)  # z will be 7.0
   ```
4. Python Data Types
   
   In programming, data type is an important concept. Variables can store data of different types, and different types can do different things. Python has the following data types built-in by default, in these categories:
   -   Text Type:	      str
   -   Numeric Types:	   int, float, complex
   -   Sequence Types:	list, tuple, range
   -   Mapping Type:	   dict
   -   Set Types:	      set, frozenset
   -   Boolean Type:	   bool
   -   Binary Types:	   bytes, bytearray, memoryview
   -   None Type:	      NoneType
   You can get the data type of any object by using the type() function:
   ```console
   x = 7
   print(type(x))
   ```
5. Python Numbers
   
   There are three numeric types in Python:
   - int
   - float
   - complex
      
   Variables of numeric types are created when you assign a value to them:
   ```console
   x = 1    # int
   y = 2.8  # float
   z = 1j   # complex
   ``` 
   To verify the type of any object in Python, use the type() function:
   ```console
   print(type(x))
   print(type(y))
   print(type(z))
   ```
   Int, or integer, is a whole number, positive or negative, without decimals, of unlimited length.
      ```console
   x = 1
   y = 35656222554887711
   z = -3255522
   
   print(type(x))
   print(type(y))
   print(type(z))
   ```
   Float, or "floating point number" is a number, positive or negative, containing one or more decimals.
   Float can also be scientific numbers with an "e" to indicate the power of 10.
   ```console
   x = 1.10
   y = 1.0
   z = -35.59
   a = 35e3
   b = 12E4
   c = -87.7e100
   
   print(type(x))
   print(type(y))
   print(type(z))
   print(type(a))
   print(type(b))
   print(type(c))
   ```
6. Python Booleans

   Booleans represent one of two values: True or False. In programming you often need to know if an expression is True or False. You can evaluate any expression in Python, and get one of two answers, True or False. When you compare two values, the expression is evaluated and Python returns the Boolean answer:
   ```console
   print(10 > 9)
   print(10 == 9)
   print(10 < 9)
   ```
   When you run a condition in an if statement, Python returns True or False:
   ```console
   a = 99
   b = 66
   
   if b > a:
     print("b is greater than a")
   else:
     print("b is not greater than a")
   ```

7. Python Operators

   Operators are used to perform operations on variables and values. In the example below, we use the + operator to add together two values:
   ```console
   print(20 + 15)
   print(250 + 105)
   print(2 + 1)
   ```
   Operator precedence describes the order in which operations are performed.
   ```console
   print((6 + 3) - (6 + 3))
   print(6 + 3 - 6 + 3)
   print(100 + 5 * 3)
   print((100 + 5) * 3)
   ```
8. Python Lists

   Lists are used to store multiple items in a single variable. Lists are one of 4 built-in data types in Python used to store collections of data, the other 3 are Tuple, Set, and Dictionary, all with different qualities and usage.
   
   Lists are created using square brackets:
   ```console
   mylist = ["iphone", "android", "mac", "windows"]
   print(mylist)
   ```
   List items are ordered, changeable, and allow duplicate values. List items are indexed, the first item has index [0], the second item has index [1] etc.
   ```console
   mylist = ["iphone", "android", "mac", "windows"]
   print(mylist[0])
   print(mylist[2])
   ```
   To determine how many items a list has, use the len() function:
   ```console
   mylist = ["iphone", "android", "mac", "windows"]
   print(len(mylist))
   ```
9. Python Tuples

   Tuples are used to store multiple items in a single variable. Tuple is one of 4 built-in data types in Python used to store collections of data, the other 3 are List, Set, and Dictionary, all with different qualities and usage. A tuple is a collection which is ordered and unchangeable. Tuples are written with round brackets.
   ```console
   mytuple = ("iphone", "android", "mac", "windows")
   print(mytuple)
   ```
   Tuple items are ordered, unchangeable, and allow duplicate values. Tuple items are indexed, the first item has index [0], the second item has index [1] etc.
   ```console
   mytuple = ("iphone", "android", "mac", "windows")
   print(mytuple[0])
   ```
   To determine how many items a tuple has, use the len() function:
   ```console
   mytuple = ("iphone", "android", "mac", "windows")
   print(len(mytuple))
   ```
10. Python Sets
   
    Sets are used to store multiple items in a single variable. Set is one of 4 built-in data types in Python used to store collections of data, the other 3 are List, Tuple, and Dictionary, all with different qualities and usage. A set is a collection which is unordered, unchangeable*, and unindexed.
    ```console
    myset = {"iphone", "android", "mac", "windows"}
    print(myset)
    ```
    Set items are unordered, unchangeable, and do not allow duplicate values. Unordered means that the items in a set do not have a defined order. 
   
    Set items can appear in a different order every time you use them, and cannot be referred to by index or key. 
   
    Set items are unchangeable, meaning that we cannot change the items after the set has been created. 
   
    Sets cannot have two items with the same value.
   
    To determine how many items a set has, use the len() function:
    ```console
    mytuple = {"iphone", "android", "mac", "windows"}
    print(len(mytuple))
    ```
11. Python If ... Else

    Python supports the usual logical conditions from mathematics:

    - Equals: a == b
    - Not Equals: a != b
    - Less than: a < b
    - Less than or equal to: a <= b
    - Greater than: a > b
    - Greater than or equal to: a >= b
    
    These conditions can be used in several ways, most commonly in "if statements" and loops.
    
    An "if statement" is written by using the if keyword.
    ```console
    a = 66
    b = 99
    if b > a:
       print("b is greater than a")
    ```
    In this example we use two variables, a and b, which are used as part of the if statement to test whether b is greater than a. As a is 66, and b is 99, we know that 99 is greater than 66, and so we print to screen that "b is greater than a".
   
    Python relies on indentation (whitespace at the beginning of a line) to define scope in the code. Other programming languages often use curly-brackets for this purpose.
    ```console
    a = 66
    b = 99
    if b > a:
    print("b is greater than a")  # you will get an error
    ```
    The elif keyword is Python's way of saying "if the previous conditions were not true, then try this condition".
    ```console
    a = 66
    b = 66
    if b > a:
       print("b is greater than a")
    elif a == b:
       print("a and b are equal")
    ```
    The else keyword catches anything which isn't caught by the preceding conditions.
    ```console
    a = 99
    b = 66
    if b > a:
       print("b is greater than a")
    elif a == b:
       print("a and b are equal")
    else:
       print("a is greater than b")
    ```
12. Python For Loops

    A for loop is used for iterating over a sequence (that is either a list, a tuple, a dictionary, a set, or a string). This is less like the for keyword in other programming languages, and works more like an iterator method as found in other object-orientated programming languages. With the for loop we can execute a set of statements, once for each item in a list, tuple, set etc.
    ```console
    items = ["iphone", "android", "mac", "windows"]
    for x in items:
       print(x)
    ```
    Even strings are iterable objects, they contain a sequence of characters:
    ```console
    for x in "android":
       print(x)
    ```
    With the break statement we can stop the loop before it has looped through all the items:
    ```console
    items = ["iphone", "android", "mac", "windows"]
    for x in items:
       print(x)
       if x == "mac":
           break
    ```
   
13. Python Functions

    A function is a block of code which only runs when it is called. You can pass data, known as parameters, into a function. A function can return data as a result.

    In Python a function is defined using the def keyword:
    ```console
    def my_function():
       print("Hello from a function")
    ```
    To call a function, use the function name followed by parenthesis:
    ```console
    def my_function():
       print("Hello from a function")
    
    my_function()
    ```
    Information can be passed into functions as arguments.

    Arguments are specified after the function name, inside the parentheses. You can add as many arguments as you want, just separate them with a comma.

    The following example has a function with one argument (fname). When the function is called, we pass along a first name, which is used inside the function to print the full name:
    ```console
    def my_function(fname):
       print(fname + " Good")
    
    my_function("Mac")
    my_function("Linux")
    my_function("Windows")
    ```
    By default, a function must be called with the correct number of arguments. Meaning that if your function expects 2 arguments, you have to call the function with 2 arguments, not more, and not less.
        ```console
    def my_function(fname, lname):
       print(fname + " " + lname)
    
    my_function("Mac", "Linux")
    ```
14. Python Arrays

    Arrays are used to store multiple values in one single variable:
    ```console
    cars = ["Audi", "Ford", "Volvo", "BMW"]

    print(cars)
    ```
    An array is a special variable, which can hold more than one value at a time. If you have a list of items (a list of car names, for example), storing the cars in single variables could look like this:
    ```console
    car1 = "Ford"
    car2 = "Volvo"
    car3 = "BMW"
    ```
    However, what if you want to loop through the cars and find a specific one? And what if you had not 3 cars, but 300? The solution is an array! An array can hold many values under a single name, and you can access the values by referring to an index number.
15. Python Dates
    
    A date in Python is not a data type of its own, but we can import a module named datetime to work with dates as date objects.

    Import the datetime module and display the current date:
    ```console
    import datetime

    x = datetime.datetime.now()
    print(x)
    ```
    When we execute the code from the example above the result will be:

    2025-06-05 14:12:08.400933
    The date contains year, month, day, hour, minute, second, and microsecond.
   
    The datetime module has many methods to return information about the date object.
   
    Here are a few examples, you will learn more about them later in this chapter:
    ```console
    import datetime
   
    x = datetime.datetime.now()
   
    print(x.year)
    print(x.strftime("%A"))
    ```
    To create a date, we can use the datetime() class (constructor) of the datetime module.

    The datetime() class requires three parameters to create a date: year, month, day.
    ```console
    import datetime

    x = datetime.datetime(2020, 5, 17)
   
    print(x)
    ```

