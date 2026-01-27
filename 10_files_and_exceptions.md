# Reading from a file
when working with information in a text file the first step is to read the file into memory. 
Program to open a file. 
```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read()
    print(contents)
```
The open() function needs one argument: the name of the file you want to open.
The open() function returns an object representing the file. 
We use the read() method in the second line of our program to read the entire contents of the file and store it as one long string in contents. 

The extra blank line at the end of the output appears because read() returns an empty string when it reaches the end of the file
We can use the rstrip() method which removes any whitespaces characters from the right side of a string. 

```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read()
    print(contents.rstrip())
```

File Paths 
To get Python to open files from a directory other than the one where your program file is stored, you need to provide a file path, which tells Python to look in a specific location on your system.

```python
#linux
with open('text_files/filename.txt') as file_object:
#Windows 
with open('text_files\filename.txt') as file_object:
```
**You can store this in a variable.**
```python
file_path = '/home/ehmatthes/other_files/text_files/filename.txt'
with open(file_path) as file_object:
```
Windows 
```python
file_path = 'C:\Users\ehmatthes\other_files\text_files\filename.txt'
with open(file_path) as file_object:
```
**Reading Line by Line**
We can use a loop on the file object to examine each line fromn a file one at a time

```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    for line in file_object:
        print(line)
```
Output
```python
3.1415926535

  8979323846

  2643383279
```
These blank lines appear because an invisible newline character is at the end of each line in the text file. The print statement adds its own newline each time we call it, so we end up with two newline characters at the end of each line

**We can again use the rstrip to fix this**
```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    for line in file_object:
        print(line.rstrip())
```
**Making a List of Lines from a File**

If you want to retain access to a file’s contents outside the with block, you can store the file’s lines in a list inside the block and then work with that list
```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines()
for line in lines:
    print(line.rstrip())
```
**Workinng with file contents**
lets try to build a single string containing all the data in the file. 
```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines()

pi_string = ''

for line in lines:
    pi_string += line.rstrip()

print(pi_string)
print(pi_string.rstrip())
```
output
```python
3.1415926535  8979323846  2643383279
3.1415926535  8979323846  2643383279
```
We can use strip instead of rstrip. 
```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines()

pi_string = ''

for line in lines:
    pi_string += line.strip()

print(pi_string)
print(len(pi_string))
```
output. 
```python
3.141592653589793238462643383279
32
```
Opening large files foe exmapl here we can print up to 50. 
```python
file_name = 'one_million_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines()

pi_string = ''

for line in lines:
    pi_string += line.strip()

print(pi_string[:50] + "...")
print(len(pi_string))
```
This will print
```python
01234567890123456789012345678901234567890123456789...
1000000
```
Birthday program 

```python
file_name = 'one_million_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines()

pi_string = ''

for line in lines:
    pi_string += line.strip()

birthday = input("Enter our birthday in form mmddyy: ")
if birthday in pi_string:
    print('Your bithday apprears in the file')
else:
    print("Your birthday does not appear in the file")
```

# Writing a file 
To write text to a file, you need to call open() with a second argument telling Python that you want to write to the file.

```python
file_name = 'opened_file.txt'

with open(file_name, 'w') as file_object:
    file_object.write("I love programming")
```
The first argument is still the name of the file we want to open. The second argument, 'w', tells Python that we want to open the file in write mode. You can open a file 198   Chapter 10 in read mode ('r'), write mode ('w'), append mode ('a'), or a mode that allows you to read and write to the file ('r+'). If you omit the mode argument, Python opens the file in read-only mode by default.

Writing multiple file. 
```python
file_name = 'opened_file.txt'

with open(file_name, 'w') as file_object:
    file_object.write("I love programming \n")
    file_object.write("I love hacking and learning about computers \n")
```
**appending to a file** 
When you open a file in append mode, Python doesn’t erase the file before returning the file object. Any lines you write to the file will be added at the end of the file. If the file doesn’t exist yet, Python will create an empty file for you.

```python
file_name = 'apended_text3.txt'

status = True
while status:
    for i in range (30):
        with open(file_name, 'a') as file_object:
            file_object.write(str(i) + " This will be a fun loop to see \n")
        if i == 10:
            status = False
            break
```
# Exceptions 
Whenever an error occurs that makes Python unsure what to do next, it creates an exception object. If you write code that handles the exception, the program will continue running. If you don’t handle the exception, the program will halt and show a traceback, which includes a report of the exception that was raised.

Handling the ZeroDivision Error Exception 
using try-except blocks.

```python
try:
    print(5/0)
except ZeroDivisionError:
    print("you cannot divide by zero")
```
If more code followed the try-except block, the program would continue running because we told Python how to handle the error

Using Exceptions to Prevent Crashes
simple calculator. 

```python
print("Give the numbers and i'll divide them: ")
print("Enter 'q' to quit. ")

while True: 
    first_number = input("enter the first number: ")
    if first_number == 'q':
        break
    second_number = input("enter the second number: ")
    try:
        answer = int(first_number) / int(second_number)
    except ZeroDivisionError:
        print("You cannot divide by 0")
    else:
        print(answer)
```

Analyzing Text
We’ll use the string method split(), which can build a list of words from a string. Here’s what split() does with a string containing just the title "Alice in Wonderland".

```python
>>> title = "Alice in Wonderland" 
>>> title.split() 
['Alice', 'in', 'Wonderland']
>>>
```

The split() method separates a string into parts wherever it finds a space and stores all the parts of the string in a list

sample program 






















