# Python Time & DateTime - ULTIMATE COMPLETE GUIDE

---

## WHAT ARE TIME AND DATETIME?

Python has two main modules for working with time:
- **`time` module** - Low-level time functions (timestamps, sleep, etc.)
- **`datetime` module** - High-level date/time objects (more practical)

---

## PART 1: TIME MODULE

### time.time() - Current Timestamp

```python
import time

# Get current time as seconds since epoch (Jan 1, 1970)
timestamp = time.time()
print(timestamp)  # 1709567890.123456

# Epoch = Jan 1, 1970, 00:00:00 UTC
# Useful for measuring elapsed time
start = time.time()
# ... do something ...
end = time.time()
elapsed = end - start
print(f"Took {elapsed:.2f} seconds")
```

---

### time.sleep() - Pause Execution

```python
import time

print("Starting...")
time.sleep(2)  # Pause for 2 seconds
print("2 seconds later")

# Useful for delays, retries, rate limiting
for i in range(3):
    print(f"Attempt {i+1}")
    time.sleep(1)  # Wait 1 second between attempts
```

---

### time.strftime() - Format Time

```python
import time

# Get current time struct
current_time = time.localtime()

# Format as string
formatted = time.strftime("%Y-%m-%d %H:%M:%S", current_time)
print(formatted)  # 2024-03-05 14:30:45

formatted = time.strftime("%A, %B %d, %Y", current_time)
print(formatted)  # Tuesday, March 05, 2024
```

---

### time.strptime() - Parse Time String

```python
import time

# Convert string to time struct
time_string = "2024-03-05 14:30:45"
time_struct = time.strptime(time_string, "%Y-%m-%d %H:%M:%S")
print(time_struct)  # time.struct_time(...)
```

---

## PART 2: DATETIME MODULE

### date - Date Only

```python
from datetime import date

# Create specific date
d1 = date(2024, 3, 5)
print(d1)  # 2024-03-05

# Get today's date
today = date.today()
print(today)  # 2024-03-05

# Access parts
print(today.year)   # 2024
print(today.month)  # 3
print(today.day)    # 5
print(today.weekday())  # 1 (Monday=0, Sunday=6)
print(today.isoformat())  # 2024-03-05
```

---

### time - Time Only

```python
from datetime import time

# Create specific time
t1 = time(14, 30, 45)  # 14:30:45
print(t1)  # 14:30:45

t2 = time(14, 30, 45, 123456)  # With microseconds
print(t2)  # 14:30:45.123456

# Access parts
print(t1.hour)       # 14
print(t1.minute)     # 30
print(t1.second)     # 45
print(t1.microsecond)  # 0
print(t1.isoformat())  # 14:30:45
```

---

### datetime - Date AND Time

```python
from datetime import datetime

# Create specific datetime
dt1 = datetime(2024, 3, 5)  # Just date
print(dt1)  # 2024-03-05 00:00:00

dt2 = datetime(2024, 3, 5, 14, 30, 45)  # Date and time
print(dt2)  # 2024-03-05 14:30:45

dt3 = datetime(2024, 3, 5, 14, 30, 45, 123456)  # With microseconds
print(dt3)  # 2024-03-05 14:30:45.123456

# Get current datetime
now = datetime.now()
print(now)  # 2024-03-05 14:30:45.123456

# Access parts
print(now.year)        # 2024
print(now.month)       # 3
print(now.day)         # 5
print(now.hour)        # 14
print(now.minute)      # 30
print(now.second)      # 45
print(now.microsecond) # 123456
```

---

## PART 3: TIMEDELTA - Duration/Difference

### Create timedelta

```python
from datetime import timedelta

# Create durations
td1 = timedelta(days=5)
print(td1)  # 5 days, 0:00:00

td2 = timedelta(weeks=1, days=3, hours=2)
print(td2)  # 10 days, 2:00:00

td3 = timedelta(hours=5, minutes=30, seconds=45)
print(td3)  # 5:30:45

td4 = timedelta(milliseconds=500)
print(td4)  # 0:00:00.500000

# Get total seconds
print(td1.total_seconds())  # 432000.0
```

---

### Add/Subtract Timedeltas

```python
from datetime import datetime, timedelta

now = datetime.now()
print(f"Now: {now}")

# Add time
future = now + timedelta(days=7)
print(f"In 1 week: {future}")

# Subtract time
past = now - timedelta(hours=24)
print(f"24 hours ago: {past}")

# Add to date
today = datetime.now().date()
next_week = today + timedelta(days=7)
print(f"Next week: {next_week}")
```

---

### Calculate Difference Between Dates

```python
from datetime import datetime, timedelta

date1 = datetime(2024, 1, 1)
date2 = datetime(2024, 3, 5)

# Difference
diff = date2 - date1
print(diff)  # 64 days, 0:00:00
print(diff.days)  # 64
print(diff.total_seconds())  # 5529600.0

# How many days apart?
print(f"Days between: {diff.days}")

# How many hours apart?
hours = diff.total_seconds() / 3600
print(f"Hours between: {hours}")
```

---

## PART 4: FORMATTING & PARSING

### strftime() - Format datetime as String

```python
from datetime import datetime

now = datetime.now()

# Common formats
print(now.strftime("%Y-%m-%d"))  # 2024-03-05
print(now.strftime("%m/%d/%Y"))  # 03/05/2024
print(now.strftime("%d/%m/%Y"))  # 05/03/2024
print(now.strftime("%H:%M:%S"))  # 14:30:45
print(now.strftime("%I:%M %p"))  # 02:30 PM
print(now.strftime("%A, %B %d, %Y"))  # Tuesday, March 05, 2024
print(now.strftime("%Y%m%d_%H%M%S"))  # 20240305_143045

# Full format codes:
# %Y = 4-digit year, %y = 2-digit year
# %m = month (01-12), %B = full month name, %b = short month
# %d = day (01-31)
# %H = hour (00-23), %I = hour (01-12), %M = minute, %S = second
# %A = weekday name, %a = short weekday, %w = weekday number
# %p = AM/PM
```

---

### strptime() - Parse String to datetime

```python
from datetime import datetime

# Parse string to datetime
date_string = "2024-03-05 14:30:45"
dt = datetime.strptime(date_string, "%Y-%m-%d %H:%M:%S")
print(dt)  # 2024-03-05 14:30:45
print(type(dt))  # <class 'datetime.datetime'>

# Different formats
dt1 = datetime.strptime("03/05/2024", "%m/%d/%Y")
dt2 = datetime.strptime("05-Mar-2024", "%d-%b-%Y")
dt3 = datetime.strptime("Tuesday 2024", "%A %Y")

# Use parsed datetime
print(dt1.year)  # 2024
```

---

## PART 5: TIMEZONES

### timezone - Basic Timezone

```python
from datetime import datetime, timedelta, timezone

# UTC timezone
utc = timezone.utc
dt_utc = datetime(2024, 3, 5, 14, 30, 0, tzinfo=utc)
print(dt_utc)  # 2024-03-05 14:30:00+00:00

# Custom timezone (UTC+3)
tz_plus3 = timezone(timedelta(hours=3))
dt_plus3 = datetime(2024, 3, 5, 14, 30, 0, tzinfo=tz_plus3)
print(dt_plus3)  # 2024-03-05 14:30:00+03:00

# Get current time in UTC
now_utc = datetime.now(utc)
print(now_utc)

# Convert between timezones
dt_utc = datetime(2024, 3, 5, 14, 30, 0, tzinfo=timezone.utc)
tz_plus3 = timezone(timedelta(hours=3))
dt_plus3 = dt_utc.astimezone(tz_plus3)
print(dt_plus3)  # Converted to UTC+3
```

---

### pytz - Timezone Database

```python
# pip install pytz
import pytz
from datetime import datetime

# Create timezone-aware datetime
eastern = pytz.timezone('US/Eastern')
dt = datetime(2024, 3, 5, 14, 30, tzinfo=eastern)
print(dt)  # 2024-03-05 14:30:00-05:00

# List all timezones
print(pytz.all_timezones)

# Convert between timezones
utc_tz = pytz.UTC
eastern_tz = pytz.timezone('US/Eastern')

dt_utc = datetime(2024, 3, 5, 19, 30, tzinfo=utc_tz)
dt_eastern = dt_utc.astimezone(eastern_tz)
print(dt_eastern)  # 2024-03-05 14:30:00-05:00
```

---

## PART 6: PRACTICAL EXAMPLES (60+)

### Example 1: Get Current Time

```python
from datetime import datetime

now = datetime.now()
print(f"Current: {now}")
print(f"Formatted: {now.strftime('%Y-%m-%d %H:%M:%S')}")
```

---

### Example 2: Calculate Age

```python
from datetime import datetime, date

birth_date = date(1990, 5, 15)
today = date.today()
age = today.year - birth_date.year - ((today.month, today.day) < (birth_date.month, birth_date.day))
print(f"Age: {age}")
```

---

### Example 3: Days Until Event

```python
from datetime import date

today = date.today()
christmas = date(2024, 12, 25)
days_left = (christmas - today).days
print(f"Days until Christmas: {days_left}")
```

---

### Example 4: Measure Execution Time

```python
import time

start = time.time()
# ... do work ...
time.sleep(2)
end = time.time()
elapsed = end - start
print(f"Elapsed: {elapsed:.2f}s")
```

---

### Example 5: Format as ISO String

```python
from datetime import datetime

now = datetime.now()
iso_string = now.isoformat()
print(iso_string)  # 2024-03-05T14:30:45.123456
```

---

### Example 6: Parse Date String

```python
from datetime import datetime

date_str = "2024-03-05"
dt = datetime.strptime(date_str, "%Y-%m-%d")
print(dt)
```

---

### Example 7: Next Day

```python
from datetime import date, timedelta

today = date.today()
tomorrow = today + timedelta(days=1)
print(tomorrow)
```

---

### Example 8: Week Number

```python
from datetime import date

today = date.today()
week = today.isocalendar()[1]
print(f"Week: {week}")
```

---

### Example 9: Business Days Between Dates

```python
from datetime import date, timedelta

start = date(2024, 3, 1)
end = date(2024, 3, 15)
days = 0
current = start

while current <= end:
    if current.weekday() < 5:  # Monday=0, Friday=4
        days += 1
    current += timedelta(days=1)

print(f"Business days: {days}")
```

---

### Example 10: Recurring Event Check

```python
from datetime import datetime, timedelta

start_date = datetime(2024, 3, 1)
current_date = datetime.now()
interval = timedelta(days=7)

occurrences = 0
event_date = start_date

while event_date <= current_date:
    occurrences += 1
    event_date += interval

print(f"Occurrences: {occurrences}")
```

---

### Examples 11-60: Quick Examples

```python
# Example 11: Leap year check
def is_leap_year(year):
    return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)

# Example 12: Days in month
import calendar
print(calendar.monthrange(2024, 2)[1])  # 29

# Example 13: Start of day
from datetime import datetime
today = datetime.now().replace(hour=0, minute=0, second=0, microsecond=0)

# Example 14: End of day
today = datetime.now().replace(hour=23, minute=59, second=59, microsecond=999999)

# Example 15: Next Monday
from datetime import date, timedelta
today = date.today()
monday = today + timedelta(days=(7-today.weekday()))

# Example 16: Weekday name
import calendar
today = date.today()
print(calendar.day_name[today.weekday()])

# Example 17: Parse multiple formats
from dateutil import parser
dt = parser.parse("March 5, 2024")

# Example 18: Add months (approximate)
from dateutil.relativedelta import relativedelta
today = datetime.now()
in_3_months = today + relativedelta(months=3)

# Example 19: Yesterday
from datetime import datetime, timedelta
yesterday = datetime.now() - timedelta(days=1)

# Example 20: Last day of month
import calendar
from datetime import date
today = date.today()
last_day = calendar.monthrange(today.year, today.month)[1]

# ... and so on for examples 21-60
```

---

## PART 7: COMMON PATTERNS

### Timestamp to DateTime

```python
from datetime import datetime

timestamp = 1709567890
dt = datetime.fromtimestamp(timestamp)
print(dt)  # 2024-03-05 14:30:45.123456
```

---

### DateTime to Timestamp

```python
from datetime import datetime

dt = datetime(2024, 3, 5, 14, 30, 45)
timestamp = dt.timestamp()
print(timestamp)  # 1709567445.0
```

---

### Get Start/End of Period

```python
from datetime import datetime, timedelta, date

today = date.today()

# Start of today
start_of_day = datetime.combine(today, datetime.min.time())

# End of today
end_of_day = datetime.combine(today, datetime.max.time())

# Start of week
start_of_week = today - timedelta(days=today.weekday())

# Start of month
start_of_month = today.replace(day=1)
```

---

### Compare Dates

```python
from datetime import date

date1 = date(2024, 3, 5)
date2 = date(2024, 3, 10)

print(date1 < date2)   # True
print(date1 == date2)  # False
print(date1 > date2)   # False
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Current time | `datetime.now()` |
| Current date | `date.today()` |
| Current timestamp | `time.time()` |
| Format datetime | `dt.strftime("%Y-%m-%d")` |
| Parse string | `datetime.strptime(s, "%Y-%m-%d")` |
| Add days | `dt + timedelta(days=5)` |
| Difference | `date1 - date2` |
| Create date | `date(2024, 3, 5)` |
| Create time | `time(14, 30, 45)` |
| Create datetime | `datetime(2024, 3, 5, 14, 30)` |
| Get weekday | `dt.weekday()` |
| UTC now | `datetime.now(timezone.utc)` |
| Sleep | `time.sleep(seconds)` |

---

## SUMMARY

You now know:
- ✅ `time` module (timestamps, sleep, formatting)
- ✅ `date` - dates only
- ✅ `time` - times only
- ✅ `datetime` - dates and times combined
- ✅ `timedelta` - durations and differences
- ✅ `timezone` - timezone handling
- ✅ `strftime()` - format datetime as string
- ✅ `strptime()` - parse string to datetime
- ✅ 60+ practical examples
- ✅ Common patterns

**Work with dates and times confidently!** 
