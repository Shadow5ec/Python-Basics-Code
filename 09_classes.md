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





















