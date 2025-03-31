## Errors
- Syntax errors
- Logical errors: Output doesn't match the expectation
- Runtime error or exception
  - An event occurring during the execution disrupts the normal flow

```py
raise ExceptionType(args)
raise Exception(args) # Exception is also a class
raise Exception('error message')
# Indicates that Exception is a direct consequence of AnotherException
raise Exception('error') from AnotherException
```

## Try Block
```py
try:
  # Perform operations
except ExceptionOne as e:
  print(e) # Contains the reason for exception
  repr(e.__context__)
  repr(e.__cause__)
except ExceptionTwo, ExceptionThree:
  # Perform operations
except:
  # Any other exceptions
else:
  # If there is no exception

# Try-Finally
# Finally clause cannot be used with else clause
try:
  # Perform operations
finally:
  # Must execute whether an exception is raised or not

try:
  # Perform operations
except ExceptionOne:
  # Perform operations
finally:
  # Must execute whether an exception is raised or not
```

## Custom Exceptions
```py
class CustomException(Exception):
  pass
```

## Logging
```py
import logging
logging.debug('message')
logging.info('message')
logging.warning('message')
logging.error('message')
logging.critical('message')
```
