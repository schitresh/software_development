## Module
- Related entities like functions, classes, variables, etc. grouped together
- Enhances modularity by easily making them available to other programs
- Some built-in modules: os, string, math, datetime, collections, random, venv

```py
# process_module.py
def process(data):
  pass

def validate(data):
  pass

# program.py
import process_module
process_module.process(data)

# To import only selected entities
from process_module import process, validate
validate(data)

# To import all entities without module namespace
from process_module import *
validate(data)

# Alias module
import process_module as pr
pr.process(data)
```

## Attributes
- `module.__name__` returns the name of the module
- `module.__file__` returns the path of the module file
- `dir(module)` returns the names (variables & functions) defined by module
- `importlib.reload(module)` to reload module using the reload method in importlib
- When a module is imported, only the functions should be imported
  - And not the executable statements
  - This can be done by checking `__name__`
  - Since it returns `__main__` when the script is executed

```py
# module.py
def add(a, b):
  return a + b

# Do not run executable statements if this module is imported somewhere
if __name__ == '__main__':
  print(add(10, 20))
```

## Package
- Heirarchical file directory consisting of multiple modules
- Need to create `__init__.py` to initialize a directory as package
  - Put explicit import statements that are required

```py
# project/custom_package/__init__.py
import validator
import data_processor

# project/test.py
import custom_package
```

### Installing Package
- To use a package anywhere in a system, it needs to be installed using PIP utility
- Save the following script in the parent folder
  - And run the pip utility from command prompt in package folder

```py
# setup.py
from setuptools import setup
setup(
  name='custom_package',
  version='0.1',
  description='Package setup script',
  url='#',
  author='anonymous',
  author_email='test@gmail.com',
  license='MIT',
  packages=['custom_package'],
  zip_safe=False
)
```
