## Time
```py
import time

time.sleep(secs)
time.time() # Current epoch time
time.localtime() # Current tuple time: struct object of time

time.asctime(tupletime) # Readable format of tuple time
time.asctime() # Current time in format 'Wed Apr 01 20:15:10 2024'
time.ctime(secs) # Readable format of epoch time
time.ctime() # Current time in format 'Wed Apr 01 20:15:10 2024'

time.strftime(fmt, tupletime) # Tuple time in format fmt
time.strftime(fmt) # Current time in format fmt
time.strptime(string, fmt) # Parse string with format fmt

time.tzname # Timezone name
time.timezone # Offset in seconds of the local time zone from UTC
```

## Date
```py
from datetime import date

date1 = date(year, month, day)
print(date1) # Prints the date object in format '2024-04-01'
date1.year
date1.month
date1.day

date.today()
date.weekday(date1)
date.ctime(date1) # 'Wed Apr 01 00:00:00 2024'
date.strftime(fmt)

from datetime import date
time1 = time(hour, minute, second, microsecond, tzinfo)

from datetime import datetime
dtime1 = datetime(year, month, day, hour, minute, second, microsecond, tzinfo)
datetime.now() # Local datetime
datetime.today() # Local datetime with tzinfo as none
datetime.utcnow()
```

## Calendar
```py
import calendar
print(cal) # Prints a calendar object in pretty format
calendar.calendar(year) # Calendar of the whole year
calendar.month(year, month) # Calendar of the month
calendar.weekday(year, month, day) # Returns weekday code, 0 is for Monday
calendar.monthrange(year, month) # Returns tuple of (weekday_code, no_of_days_in_month)
```
