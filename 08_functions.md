# Functions

## Defining a Function
A simple function that prings greetings 
```python
def greet_user():
    print("Hello!")

greet_user()
```

## Passing Information to a Function
By adding infomration to the parenthesis of the function 
```python
def great_user(name):
    print("Hello", name.title() + "!")

great_user('alex')
```
## Arguments and Parameters
The name in the above function is the definition of parameter, a piece of information that a function needs to do it's job. 
### Positional Arguments
When you call a function, Python must match each argument in the function call with a parameter in the function definition. The values matched up this way are what are called positional arguments
```python
def describe_pet(animal_type, pet_name):
    print("I have a " + animal_type )
    print("The name of pet is " + pet_name)

describe_pet('german shepherd', 'fury')
```

### Multiple Function Calls
You can call a function as many times as you need. 
```python
def describe_pet(animal_type, pet_name):
    print("I have a " + animal_type )
    print("The name of pet is " + pet_name)

describe_pet('german shepherd', 'fury')
describe_pet('pit bull', 'newt')
describe_pet('husky', 'elon')
```


### Order Matters in Positional Arguments
- You can get unexpected results if you mix up the order of the arguments in a function call when using positional arguments:

### Keyword Arguments
A keyword argument is a name-value pair that you pass to a function. You directly associate the name and the value within the argument, so when you pass the argument to the function, there’s no confusion.
```python
def describe_pet(animal_type, pet_name):
    print("i have a " + animal_type)
    print("The name of my pet is " + pet_name)

describe_pet(pet_name="luke", animal_type="husky")
```

### Default Values
When writing a function, you can define a default value for each parameter.
Parameters with default values must come after parameters without default values.
```python
def describe_pet(pet_name,animal_type='dog'):
    print("i have a " + animal_type)
    print("The name of my pet is " + pet_name)

describe_pet(pet_name='nelson')
# you can also do
describe_pet('nelson')
```
### Equivalent Function Calls
This is depending on if you have use default values, keyword arguments or positional arguements.
```python
describe_pet('willie')
describe_pet(pet_name='willie')
# A hamster named Harry.
describe_pet('harry', 'hamster')
describe_pet(pet_name='harry', animal_type='hamster')
describe_pet(animal_type='hamster', pet_name='harry')
```

### Avoiding Argument Errors
When you start to use functions, don’t be surprised if you encounter errors about unmatched arguments. Unmatched arguments occur when you provide fewer or more arguments than a function needs to do its work.



## Return Values
### Returning a Simple Value
A function doesn’t always have to display its output directly. Instead, it can process some data and then return a value or set of values. the value retreund is called the return value. 
```python
def get_fromatted_name(first_name, last_name):
    full_name = first_name + ' ' + last_name
    return full_name

musician = get_fromatted_name('alex', 'maina')
print(musician)
```
When you call a function that returns a value, you need to provide a variable where the return value can be stored.

### Making an Argument Optional
To make the middle name optional, we can give the middle_name argument an empty default value and ignore the argument unless the user provides a value.
```python
def get_fromatted_name(first_name, last_name, middle_name=''):
    if middle_name:
        full_name = first_name + ' ' + middle_name + ' ' + last_name
    else: 
        full_name = first_name + ' ' + last_name
    return full_name.title()

musician = get_fromatted_name('alex', 'maina')
print(musician)
musician = get_fromatted_name('alex','kigunda', 'maina')
print(musician)
```
### Returning a Dictionary
```python
def build_person(first_name, last_name, middle_name=''):
    if middle_name:
        person = {'first': first_name, 'middle': middle_name, 'last': last_name}
    else:
        person ={'first': first_name, 'last':last_name}
    return person

info = build_person('alex', 'kigunda', 'maina')
print(info)
info = build_person('alex','Maina')
print(info)
``` 
### Using a Function with a while Loop
```python
def get_formatted_name(first_name, last_name):
    person = first_name + " " + last_name
    return person
value = True

while value:
    print("\n Please tell me your name")
    f_name = input("First Name: ")
    l_name = input("last Name: ")
    f_name = get_formatted_name(f_name, l_name)
    print("\n Proceed request")
    proceed = input("Do you want to prceed Yes/No? ")
    if proceed == 'Yes': 
        value = True
    else:
        value = False
    print('hello ' + f_name + " Welcome to the team!")
```


## Passing a List
Passing a list of users to great each other in a function. 
```python
def greet_user(names):
    for name in names:
        msg = "Hello " + name.title()+ "!"
        print(msg)
usernmes = ["paul","isaac","jake","ian"]
greet_user(usernmes)
```
### Modifying a List in a Function
```python
unprinted_design = ['iphone_case', 'robot pendant', 'file']
completed_models = []

while unprinted_design:
    current_deisgn = unprinted_design.pop()
    print('Printing: ' + current_deisgn)
    completed_models.append(current_deisgn)
print("The following models have been completed")
for c in completed_models:
    print(c)
```


### Preventing a Function from Modifying a List
You can send a copy of a list to a funcion 
```python
function_name(list_name[:])
```
```python
original = ["a","b","c"]
copy = original[:]

copy.pop()
print(original)
print(copy)
```

Code	Meaning
list_a = list_b	Same list, two names
list_a = list_b[:]	Two separate lists
print(list[:])	“Show me a snapshot”

### Passing an Arbitrary Number of Arguments
Sometimes you won’t know ahead of time how many arguments a function needs to accept.
```python
def make_pizza(*toppings):
    print(toppings)
make_pizza('pepperoni', 'green peper', 'extra cheese')
make_pizza('cheese', 'pepperoni')
```
imporved in a loop 
```python
def make_pizza(*toppings):
    print("making pizza with the following ingrideints")
    for t in toppings:
        print("-" + t)

make_pizza('pepperoni', 'green pepper', 'extra cheese', 'tomatoo')
```
The asterisk in the parameter name *toppings tells Python to make an empty tuple called toppings and pack whatever values it receives into this tuple.

### Mixing Positional and Arbitrary Arguments
If you want a function to accept several different kinds of arguments, the parameter that accepts an arbitrary number of arguments must be placed last in the function definition.
```python
def make_pizza(size, *toppings):
    print("making pizza size " + str(size) + " with the following ingrideints")
    for t in toppings:
        print("-" + t)

make_pizza(20, 'pepperoni', 'green pepper', 'extra cheese', 'tomatoo')
```

### Using Arbitrary Keyword Arguments
Sometimes you’ll want to accept an arbitrary number of arguments, but you won’t know ahead of time what kind of information will be passed to the function.
One example involves building user profiles: you know you’ll get information about a user, but you’re not sure what kind of information you’ll receive. The function build_profile() in the Functions   153 following example always takes in a first and last name, but it accepts an arbitrary number of keyword arguments as well.
```python
def build_profile (first, last, **user_info):
    profile = {}
    profile['first name'] = first
    profile['last name'] = last 
    for k, v in user_info.items():
        profile[k] = v
    return profile

user_profile = build_profile('alex', 'jones', location='south africa', age=12, major='computer science')
print(user_profile)
```





The double asterisks before the parameter **user_info cause Python to create an empty dictionary called user_info and pack whatever name-value pairs it receives into this dictionary.


## Storing Your Functions in Modules
You can go a step further by storing your functions in a separate file called a module and then importing that module into your main program.
- Allows you to hide implementation details and focus on the program’s high-level logic
- Makes it easy to reuse functions across multiple programs
- Enables sharing useful functionality without exposing your entire codebase
- Improves code organization and readability
- Encourages modular design (separation of concerns)
- Allows you to import and use libraries written by other programmers
- Makes programs easier to maintain and update
- Supports collaboration by sharing only what’s needed
We start by first creating a module, a module is a file ending with .py that contains the code you wnat to import into your program.

We start by creating our module. 
### Importing an Entire Module

```python
def make_pizza(size, *toppings):
    print("making pizza of sise"  + str(size) + "With the following toppings")
    print("The toppings are: ")
    for t in toppings:
        print("-" + t)
```
We then save it in the same folder as the file we will import it to and import the pizza. 
```python
import pizza 
pizza.make_pizza(23, 'green_pepper', 'cheese', 'white onions', 'tomatos')
```
Now if we call our function we'll see that it works. 

```python
making pizza of sise23With the following toppings
The toppings are: 
-green_pepper
-cheese
-white onions
-tomatos
PS C:\Users\klax7>
```
When Python reads this file, the line import pizza tells Python to open the file pizza.py and copy all the functions from it into this program.
If you use this kind of import statement to import an entire module named module_name.py, each function in the module is available through the following syntax:
```python
module_name.function_name()
```

### Importing Specific Functions
The general syntax and approach for importing a specific module is 
```python
from module_name import function_name
```
You can import as many functions as you want from a module by separating each function’s name with a comma
```python
from module_name import function_0, function_1, function_2
```
Our example form the pizza module. 
```python
from pizza import make_pizza
make_pizza(23, 'green_pepper', 'cheese', 'white onions', 'tomatos')
```

### Using as to Give a Function an Alias
If the name of a function you’re importing might conflict with an existing name in your program or if the function name is long, you can use a short, unique alias—an alternate name similar to a nickname for the function
```python
from pizza import make_pizza as mp
mp(23, 'green_pepper', 'cheese', 'white onions', 'tomatos')
```
The import statement shown here renames the function make_pizza() to mp() in this program.

### Using as to Give a Module an Alias
You can also provide an alias for a module name. Giving a module a short alias, like p for pizza, allows you to call the module’s functions more quickly.
```python
import pizza as p
p.make_pizza(23, 'green_pepper', 'cheese', 'white onions', 'tomatos')
```
### Importing All Functions in a Module
You can tell Python to import every function in a module by using the asterisk (*) operator.
The asterisk in the import statement tells Python to copy every function from the module pizza into this program file. Because every function is imported, you can call each function by name without using the dot notation. However, it’s best not to use this approach when you’re working with larger modules that you didn’t write: if the module has a function name that matches an existing name in your project, you can get some unexpected results.
```python
from pizza import *
make_pizza(23, 'green_pepper', 'cheese', 'white onions', 'tomatos')
```

## Styling Functions

- Use clear, descriptive function names
- Function and module names use lowercase + underscores
- Every function should have a docstring explaining what it does
- Docstrings go right after the function definition
- Default parameters use no spaces around `=`
- Keyword arguments follow the same rule
- Keep lines under 79 characters (PEP 8)
- If a function is long, split parameters across lines
- Separate functions with two blank lines
- Put imports at the top of the file (after file comments)
