## Errors
- Syntax errors
- Logical errors: Output doesn't match the expectation
- Runtime error or exception
  - An event occuring during the execution disrupts the normal flow

```rb
raise ExceptionType.new(message)
raise StandardError.new('error message')
raise(StandardError, 'error message')
```

## Try Block
```rb
begin:
  # Peform operations
rescue ExceptionOne => e
  p(e.message)
  p(e.backtrace)
rescue ExceptionTwo, ExceptionThree
  # Peform operations
rescue
  # Any other exceptions
else
  # If there is no exception
ensure
  # Must execute whether an exception is raised or not
end
```

## Custom Exceptions
```rb
class CustomException < Exception
end
```

## Logging
```py
require 'logging'
logger = Logger.new(STDOUT)
logger = Logger.new('log_file.log')

logger.debug('message')
logger.info('message')
logger.warn('message')
logger.error('message')
logger.fatal('message')
```
