## Characteristics
- Interpreted language
    - No need to compile the program before executing
        - Where machine version of the entire source program is generated
        - And it fails if a single erroneous statement occurs
    - Processed at runtime by the interpreter
        - Interpreter reads the source code statement by statement
        - It takes one instruction from the code at a time
        - And translates it to machine code & executes it
- Dynamically typed
    - Doesn't know about the data type of a variable until the code runs
    - No need to explicitly state the data type during variable declaration
    - Altering data type of the variable is allowed
    - While in statically typed languages, type checking occurs at compile time
- Strongly typed
    - Doesn't allow automatic type conversion between unrelated data types
    - That is, 1 + '2' will raise an error
    - Languages like javascript are weakly typed (1 + '2' = '12')
- Blocks of code are denoted by line indentation which is rigidly enforced
- Automatic garbage collection
- Supports imperative, structured and OOP methodology
- PEP (Python Enhancement Proposal)
    - Facilitates new features and maintains readability
    - Allows anyone to submit a PEP for a new feature or library

## Applications
- Data science
    - Libraries like NumPy, Pandas, Matplotlib are extensively used
    - To apply mathematical algorithms to data and generate visualizations
- Machine learning
    - Libraries like Scikit-learn and TensorFlow help in building models
    - For prediction of trends based on past data
    - Like customer satisfaction, projected values of stocks
- Web Development
    - Django, Flask, Pyramid facilitate rapid web application development
- Computer vision & image processing
    - OpenCV is a library used for capturing & processing images
        - It is a C++ library and its python port is used
    - Used for face detection & pattern recognition
- Also used in embedded systems, internet of things (IoT), desktop GUI, scripting

## Python Interpreter
- Works on the principle of REPL (Read, Evaluate, Print, Loop)
- Can be opened by running the command: python
- IPython (Interactive Python)
    - More enhanced and powerful interactive environment than the standard python shell
    - Provides many features like syntax highlight, autocompletion, tracks history

## Virtual Environment
- Used to create a virtual installation of python inside a project directory
- Helps to install and manage python packages for each project
    - Without breaking other environments
- Supported by `venv` module in standard python distribution

```sh
python -m venv new_env
source new_env/bin/activate
deactivate
rm -rf new_env # To delete the virtual env
```

## Basics
```py
# Single line comment
'''
Multiline comment
'''

# Standard Input
# Reads the user input (as a string)
# If the input is required as integer, need to cast it like int(value)
value = input('Enter input value')

# Standard Output
print('Hello')
print(val1)
print(val1, val2) # Seperated using space by default
print(val1, val2, sep=', ') # Seperated by comma
print(val1, val2, sep=', ', end='\n') # Separated by comma and ends with newline

# Performance
from timeit import timeit
# timeit(callable, number = num)
# 'number' is the number of times it will be executed
timeit(lambda: test_function(arg1, arg2), number = 5)
```
