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



### Using a Function with a while Loop

## Passing a List
### Modifying a List in a Function
### Preventing a Function from Modifying a List
### Passing an Arbitrary Number of Arguments
### Mixing Positional and Arbitrary Arguments
### Using Arbitrary Keyword Arguments

## Storing Your Functions in Modules
### Importing an Entire Module
### Importing Specific Functions
### Using as to Give a Function an Alias
### Using as to Give a Module an Alias
### Importing All Functions in a Module

## Styling Functions
