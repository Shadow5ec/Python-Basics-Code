# Input 
**How the input() function works**
The input() function pauses your program and waits for the user to enter some text.

```python
message = input("Tell me something about yourself:> ")
print(message)
```
The input() function takes one argument: the prompt, or instructions, that we want to display to the user so they know what to do
**Writing Clear Prompts**
- Add a space at the end of your prompts (after the colon in the preceding example)

prompt longer than one line. You can store your prompt in a variable and pass that variable to the input() function. 
```python
prompt = "If you tell us who you are we can personalize the message you see"
prompt += "\n What is your fisrt name: "

name = input(prompt)
```
**Using int() to Accept Numerical Input** 
The input() function, Python interprets everything the user enters as a string.
We can use int() function to tell python to treat the input as numearical calue. 
```python
age = input("How old are you: ")
age = int(age)
print(age)
```
# While loops 
This loops runs as long as a certain condition is met. 
While loop to count a series of numbers. 
```python
count_number = 1
while count_number <=50:
    print(count_number)
    count_number += 1
```

**Letting the User Choose When to Quit**

```python
prompt = "\nTell me something and i will repeat it back to you: "
prompt += "\t Enter quite at the end to break the program: "

message = ""
while message != "quite":
    message = input(prompt)
    print(message)
```
**using a flag**
```python
prompt = "Enter your test: "
active = True 

while active: 
    message = input(prompt).strip().lower()
    if message == 'stop':
        active = False
    else: 
        print("proceeding with program execution")

```
**Using break to Exit a Loop**
```python
prompt = "Please enter the name of the city you visted: "

while True: 
    city = input(prompt)
    if city == 'quit':
        break
    else: 
        print("I'd love to visit the city myself too " + city.title())
```

**Using continue in a loop**

**Avoiding inifite loops**


**Using a while loop with a dictonary or list**

































