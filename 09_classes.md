# Classes 
Object-oriented programming is one of the most effective approaches to writing software. When you write a class, you define the general behavior that a whole category of objects can have.

**Creating the dog class**
A function that’s part of a class is a method.
## the __init()__ method 
The self parameter is required in the method definition, and it must come first before the other parameters.
The __init__() Method and Class Basics
- A method is a function that belongs to a class. Everything you know about functions also applies to methods.
- __init__() is a special method that runs automatically when a new object (instance) of a class is created.
- The name __init__ has double underscores on both sides to avoid conflicts with Python’s built-in methods.

Parameters in __init__()
- __init__() usually takes:
- self (always required and always first)
- other values needed to set up the object (e.g. name, age)
- self refers to the current object being created.
- Python automatically passes self, so when creating an object you only provide the other arguments.

Attributes
- Variables that start with self. (like self.name, self.age) are called attributes.
Attributes:
- belong to each individual object
- can be accessed by any method in the class
- self.name = name stores the dog’s name inside that specific dog object.
- self.age = age does the same for age.

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def sit(self):
        print(self.name.title() + " is now sitting")
    def roll_over(self):
        print(self.name.title() + " rolled over")

```
## Making as instance from a class. 

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def sit(self):
        print(self.name.title() + " is now sitting")
    def roll_over(self):
        print(self.name.title() + " rolled over")

my_dog = Dog('willie', 7)
print("my dog's name is " + my_dog.name.title())
print("My dog's age is " + str(my_dog.age))
```
### accessing adn attribute. 
To access the attributes of an instance, you use dot notation
```python
my_dog.name
```
### calling methods. 
After we create an instance from the class Dog, we can use dot notation to call any method defined in Dog.
```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def sit(self):
        print(self.name.title() + " is now sitting")
    def roll_over(self):
        print(self.name.title() + " rolled over")

my_dog = Dog('willie', 7)
my_dog.sit()
my_dog.roll_over()
```
### Creating multiple instances. 

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def sit(self):
        print(self.name.title() + " is now sitting")
    def roll_over(self):
        print(self.name.title() + " rolled over")

my_dog = Dog('willie', 7)
my_dog1 = Dog("johnie", 5)
my_dog2 = Dog("paul", 4)

print("my dogs name is " + my_dog.name.title())
print("my dogs age is " + str(my_dog.age))
my_dog.sit()
print("\n")
print("_______________this is the second dog____________________")
print("\n")
print("my dogs name is " + my_dog1.name.title())
print("my dogs age is " + str(my_dog1.age))
my_dog1.sit()
```
This will print 
```python
my dogs name is Willie
my dogs age is 7
Willie is now sitting


_______________this is the second dog____________________


my dogs name is Johnie
my dogs age is 5
Johnie is now sitting
```

## Working with Classes and Instacee 

The Car Class 
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make 
        self.model = model 
        self.year = year 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    
my_new_car = Car("audi", "a4", 2016)
print(my_new_car.get_descriptive_name())
```
output 
```python
2016 Audi A4
```
### setting a default value for an attribute 
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    def read_odometer(self): 
        print("This car has " + str(self.odometer_reading) + " miles on it.")    

my_new_car = Car("audi", "A4", 2020)
print(my_new_car.get_descriptive_name())
my_new_car.read_odometer()
```
output 
```python
2020 Audi A4
This car has 0 miles on it
```
### modifying attribute values 
You can change an attribute value in three ways. 
- Change the value directly
- Set the value through a method
- Increment the value 

Modifying an Attribute’s Value Through a Method
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    def read_odometer(self): 
        print("This car has " + str(self.odometer_reading) + " miles on it.")    
    def upate_odometer(self, mirage): 
        self.odometer_reading = mirage

my_new_car = Car("audi", "A4", 2020)
print(my_new_car.get_descriptive_name())
my_new_car.upate_odometer(25)
my_new_car.read_odometer()
```
Output. 
```python
2020 Audi A4
This car has 25 miles on it.
```
Adding a way to make sure you can never rollback the odometer reading. 
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    def read_odometer(self): 
        print("This car has " + str(self.odometer_reading) + " miles on it.")    
    def upate_odometer(self, mirage): 
        if mirage >= self.odometer_reading: 
            self.odometer_reading = mirage
        else:
            print("you cannot rollback an odometer!")
my_new_car = Car("audi", "A4", 2020)
print(my_new_car.get_descriptive_name())
my_new_car.upate_odometer(25)
my_new_car.read_odometer()
```
## inheritance
When one class inherits from another, it automatically takes on all the attributes and methods of the first class. The original class is called the parent class, and the new class is the child class.

```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    def read_odometer(self): 
        print("This car has " + str(self.odometer_reading) + " miles on it.")    
    def upate_odometer(self, mirage): 
        if mirage >= self.odometer_reading: 
            self.odometer_reading = mirage
        else:
            print("you cannot rollback an odometer!")
    def increment_odometer(self,miles):
        self.odometer_reading += miles
class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)

my_tesla = ElectricCar('tesla','model-s', 2020)
print(my_tesla.get_descriptive_name())
```
this will produce. 
```python
2020 Tesla Model-S
```
The super() function at x is a special function that helps Python make connections between the parent and child class.
This line tells Python to call the __init__() method from ElectricCar’s parent class, which gives an ElectricCar instance all the attributes of its parent class.
The name super comes from a convention of calling the parent class a superclass and the child class a subclass.

### Defining Attributes and Methods for the Child Class
```python 
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0 
    def get_descriptive_name(self):
        long_name = str(self.year) + ' ' + self.make + ' ' + self.model
        return long_name.title()
    def read_odometer(self): 
        print("This car has " + str(self.odometer_reading) + " miles on it.")    
    def upate_odometer(self, mirage): 
        if mirage >= self.odometer_reading: 
            self.odometer_reading = mirage
        else:
            print("you cannot rollback an odometer!")
    def increment_odometer(self,miles):
        self.odometer_reading += miles
class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.batter_size = 70
    def describe_battery(self):
        print("This car has a " + str(self.batter_size) + "-kwh battery")

my_tesla = ElectricCar('tesla','model-s', 2020)
print(my_tesla.get_descriptive_name())
my_tesla.describe_battery()
```
There’s no limit to how much you can specialize the ElectricCar class. You can add as many attributes and methods as you need to model an electric car to whatever degree of accuracy you need.

### Overriding Methods from the Parent Class

```python
class ElectricCar(Car):
    def fill_gas_tank(self):
        """Electric cars don't have gas tanks."""
        print("This car doesn't need a gas tank!")
```
### Instances as Attributes

When a class starts getting too big, with many attributes and methods, it’s a sign that some parts should be moved into their own class. Breaking a large class into smaller ones makes the code easier to read and manage.

For example, if an ElectricCar class has many battery-related details, those should be moved into a separate Battery class. The ElectricCar class can then use a Battery object as one of its attributes.


## Importing Classes 
Python lets you store classes in modules and then import the classes you need into your main program.

my car class
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0
    def get_descripive_name(self):
        long_name = str(self.year) + ' ' +  self.make + ' ' + self.model 
        return long_name.title()
    def read_odometer(self):
        print("This car has " + str(self.read_odometer) + "miles on it")
    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self, miles):
        self.odometer_reading += miles
```
Importing the car class and using it. 
```python
from car import Car

my_new_car = Car('audi', 'a4', 2016)
print(my_new_car.get_descripive_name())
```
output. 
```python
2016 Audi A4
```

### Storing Multiple Classes in a Module
You can store as many classes as you need in a single module, although each class in a module should be related somehow
The parrent class.(supe)

```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model 
        self.year = year 
        self.odometer_reading = 0
    def get_descripive_name(self):
        long_name = str(self.year) + ' ' +  self.make + ' ' + self.model 
        return long_name.title()
    def read_odometer(self):
        print("This car has " + str(self.read_odometer) + "miles on it")
    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self, miles):
        self.odometer_reading += miles
    

class Battery:
    """A simple attempt to model a battery for an electric car."""

    def __init__(self, battery_size=60):
        """Initialize the battery's attributes."""
        self.battery_size = battery_size

    def describe_battery(self):
        """Print a statement describing the battery size."""
        print(f"This car has a {self.battery_size}-kWh battery.")

    def get_range(self):
        """Print a statement about the range this battery provides."""
        if self.battery_size == 70:
            car_range = 240
        elif self.battery_size == 85:
            car_range = 270
        else:
            car_range = 200  # default range

        message = f"This car can go approximately {car_range} miles on a full charge."
        print(message)


class ElectricCar(Car):
    """Models aspects of a car, specific to electric vehicles."""

    def __init__(self, make, model, year):
        """
        Initialize attributes of the parent class.
        Then initialize attributes specific to an electric car.
        """
        super().__init__(make, model, year)
        self.battery = Battery()
```
Importing multiple classes from a module. 

```python
from car import Car, ElectricCar


my_new_car = Car('audi', 'a4', 2016)
print(my_new_car.get_descripive_name())
```

Importing an Entire Module

```python
import car

my_new_car = Car('audi', 'a4', 2016)
print(my_new_car.get_descripive_name())
```
Importing All Classes from a Module
```python
from module_name import *
```

Importing a Module into a Module
Sometimes you’ll want to spread out your classes over several modules to keep any one file from growing too large and avoid storing unrelated classes in the same module.
For example, let’s store the Car class in one module and the ElectricCar and Battery classes in a separate module. We’ll make a new module called electric_car.py—replacing the electric_car.py file we created earlier—and copy just the Battery and ElectricCar classes into this file:

Goal (in simple words)
Put Car in one file (car.py)
Put Battery and ElectricCar in another file (electric_car.py)
Import and use them in a third file (my_cars.py)
This is how Python modules work in real projects.

car 
```python
"""A class that can be used to represent a car."""

class Car:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year

    def get_descriptive_name(self):
        return f"{self.year} {self.make} {self.model}"
```
electric_car 
```python
"""A set of classes that can be used to represent electric cars."""

from car import Car


class Battery:
    def __init__(self, battery_size=60):
        self.battery_size = battery_size

    def describe_battery(self):
        print(f"This car has a {self.battery_size}-kWh battery.")


class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery = Battery()

```
my_cars.py
```python
from car import Car
from electric_car import ElectricCar

my_beetle = Car("Volkswagen", "Beetle", 2016)
print(my_beetle.get_descriptive_name())

my_tesla = ElectricCar("Tesla", "Roadster", 2016)
print(my_tesla.get_descriptive_name())
my_tesla.battery.describe_battery()
```
What to remember (exam + real life)
- One file = one module
- Use from module import Class to reuse code
- Parent classes must be imported where inheritance is used
- This structure keeps projects clean and scalable

# Python Standard Library – Summary

## What Is the Python Standard Library

The Python standard library is a collection of built-in modules included with every Python installation.
It provides prewritten classes and functions that can be used by importing them into a program.
Modules from the standard library are imported using the `import` statement.

---

## OrderedDict (collections module)

Dictionaries store key-value pairs but do not track the order in which items are added in older Python versions.
The `OrderedDict` class keeps items in the order they were inserted.
`OrderedDict` behaves like a normal dictionary but preserves insertion order.

---

## OrderedDict Example

```python
from collections import OrderedDict

favorite_languages = OrderedDict()
favorite_languages['jen'] = 'python'
favorite_languages['sarah'] = 'c'
favorite_languages['edward'] = 'ruby'
favorite_languages['phil'] = 'python'

for name, language in favorite_languages.items():
    print(f"{name.title()}'s favorite language is {language.title()}.")











