## Time
```rb
Time.now
Time.new(year, month, day, hour, second, millisec, zone)
Time.local(year, month, day, hour, second, millisec, zone)
Time.utc(year, month, day, hour, second, millisec, zone)
Time.at(epoch_secs)

time.year # month, day, wday, hour, min, sec, zone
time.monday?
time.zone
time.utc?

t1 == t2 # <, >
t1.between?(t2, t3)
```
