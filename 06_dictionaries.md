# Simple Dictonary 
A dictionary in Python is a collection of key-value pairs. Each key is connected to a value, and you can use a key to access the value associated with that key.
 a dictionary is wrapped in braces, {}.
 A key-value pair is a set of values associated with each other. 
```python
alien_0 = {'color': 'green', 'points': 5}
print(alien_0['color'])
print(alien_0['points'])
```
The simplest dictionary has exactly one key-value pair. Example 
```python
alien_0 = {'color': 'green'}
```
** Accessing values in a dictionary ** 
Give the name of the dictionary and then place the key inside a set of square brackets.
```python
alien_0 = {'color': 'green'}
print(alien_0['color'])
```
You have  unlimited number of key value pairs in a dictionary 
```python
alien_0 = {'color': 'green', 'points': 5}
new_points = alien_0['points']
print("For shooting down the alien you have earned " + str(new_points)+ ".")
```
** Adding New Key-Value Pairs ** 
```python
alien_0 = {'color': 'green', 'points': 5}
# this is a sample of adding co-ordinate values 
alien_0['x_position'] = 0
alien_0['y_position'] = 25
print(alien_0)
```
This will give you this values. 
```python
{'color': 'green', 'points': 5, 'x_position': 0, 'y_position': 25}
PS C:\Users\klax7>
```
Starting with an empty dictionary 
```python
alien_0 = {}
alien_0['color'] = 'green'
alien_0['points'] = 10 
alien_0['age'] = 26
print(alien_0)
```
This will produce 
```python
{'color': 'green', 'points': 10, 'age': 26}
```

Modifying Values in a Dictionary
```python
alien_0 = {'color': 'green'}
print(alien_0)
alien_0['color'] = 'black'
print(alien_0)
```
Sample program that will increment position based on alien speed. 
```python
alien_0 = {'x_position': 0, 'y_position':25, 'speed': 'medium'}
print("Original x-position: " + str(alien_0['x_position']))

if alien_0['speed'] == 'slow':
    x_increment = 1
elif alien_0['speed'] == 'medium':
    x_increment = 2
else: 
    x_increment = 3

alien_0['x_position'] = alien_0['x_position'] + x_increment

print ("Nex x-position: " + str(alien_0['x_position']))
```
Removing key-value pairs
```python 
alien_0 = {'x_position': 0, 'y_position':25, 'speed': 'medium'}
print(alien_0)

del alien_0['speed']
print(alien_0)
```
This will produce 
```python
{'x_position': 0, 'y_position': 25, 'speed': 'medium'}
{'x_position': 0, 'y_position': 25}
PS C:\Users\klax7>
```
### Looping Through a Dictionary

```python
user_0 = {
    "username": 'admin',
    "first": "tester",
    "last": "last_tester"

}
for key, value in user_0.items():
    print("\n key: " + key)
    print("value: " + value)
```
this will produce.
```python
 key: username
value: admin

 key: first
value: tester

 key: last
value: last_tester
PS C:\Users\klax7>
```
You can choose any name you want for the variables like k,v 
```python
user_0 = {
    "username": 'admin',
    "first": "tester",
    "last": "last_tester"

}
for k, v in user_0.items():
    print("\n key: " + k)
    print("value: " + v)
```
Python doesn’t care about the order in which key-value pairs are stored; it tracks only the connections between individual keys and their values.

** Looping Through All the Keys in a Dictionary **
The keys() method is useful when you don’t need to work with all of the values in a dictionary.
```python
user_0 = {
    "username": 'admin',
    "first": "tester",
    "last": "last_tester"

}
for first in user_0.keys():
    print(first.title())
```
output 
```
Username
First
Last
```
Looping through the keys is actually the default behavior when looping
through a dictionary, so this code would have exactly the same output if you
wrote . . .
for name in favorite_languages:
rather than . . .
for name in favorite_languages.keys():

** Looping Through a Dictionary’s Keys in Order ** 
One way to return items in a certain order is to sort the keys as they’re returned in the for loop. You can use the sorted() function to get a copy of the keys in order:
```python
user_0 = {
    "username": 'admin',
    "first": "tester",
    "last": "last_tester"

}
for first in sorted(user_0.keys()):
    print(first.title())
```
sample 
```python
user_0 = {
    "paul": 'true',
    "isaac": "false",
    "joseph": "true"

}
for first in sorted(user_0.keys()):
    print("Thank you " + first.title() + " for taking part in the poll")
```
This will produce. 

```python
Thank you Isaac for taking part in the poll
Thank you Joseph for taking part in the poll
Thank you Paul for taking part in the poll
```
Looping Through All Values in a Dictionary
If you are primarily interested in the values that a dictionary contains, you can use the values() method to return a list of values without any keys.

```python
user_0 = {
    "paul": 'true',
    "isaac": "false",
    "joseph": "true",
}

for v in user_0.values():
    print(v.title())
```
This will produce 
```
True
False
True
```
### Nesting 

You can nest a set of dictionaries inside a list, a list of items inside a dictionary, or even a dictionary inside another dictionary.

```python
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}
aliens = [alien_0, alien_1, alien_2]

for a in aliens:
    print(a)
```
output

```python
{'color': 'green', 'points': 5}
{'color': 'yellow', 'points': 10}
{'color': 'red', 'points': 15}
```
Sample 

```python
aliens = []

for alien_number in range(20):
    new_alien = {'color': 'green', 'points': 5, 'speed': 'slow'}
    aliens.append(new_alien)
for alien in aliens[:5]:
    print(alien)
```

A List in a Dictionary

A Dictionary in a Dictionary


















































