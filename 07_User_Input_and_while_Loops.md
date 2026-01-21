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
#While loops 


