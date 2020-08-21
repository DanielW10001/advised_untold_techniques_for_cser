# datetime

```python3
# Aware: Unambiguously located
# Naive: Ambiguously located; Treated as in system time zone
# UTC should be aware rather than naive

# Constant
datetime.MINYEAR
datetime.MAXYEAR

# Type
# Immutable, Hashable, Picklable
datetime.date
datetime.time
datetime.datetime
datetime.timedelta
datetime.tzinfo
datetime.timezone

# timedelta
# Construct
delta = datetime.timedelta([days=days][,  # Normalized; Immutable
                           seconds=seconds][,
                           microseconds=microseconds][,
                           milliseconds=milliseconds][,
                           minutes=minutes][,
                           hours=hours][,
                           weeks=weeks])
# Class Attribute
datetime.timedelta.min  # Most negetive value
datetime.timedelta.max  # Most positive value
datetime.timedelta.resolution  # Smallest value difference
# Read-only Attribute
delta.days
delta.seconds
delta.microseconds
# Operation
delta = delta + delta  # Sum
delta = delta - delta  # Difference
delta = delta * number; number * delta  # Product
number = delta / delta  # Division
delta = delta / number  # Division
number = delta // delta  # Integer division
delta = delta // number  # Integer division
delta = delta % delta  # Remainder
quotient, remainder_delta = divmod(delta, delta)  # Division and modulo
delta = + delta  # Same delta
delta = - delta  # Reversed delta
delta = abs(delta)  # Absolute
string = str(delta)
repr_string = repr(delta)
# Boolean
delta  # `True` when `delta != datetime.timedelta(0)`
delta == delta
delta != delta
delta < delta
delta <= delta
delta >= delta
delta > delta
# Method
delta.total_seconds()  # Total seconds equivalence

# date
# Construct
date = datetime.date(year=year,
                     month=month,
                     day=day)
date = datetime.date.today()
date = datetime.date.fromtimestamp(timestamp)
date = datetime.date.fromordinal(ordinal)  # Jan 1 of year 1 has ordinal 1
date = datetime.date.fromisoformat('YYYY-MM-DD')
date = datetime.date.fromisocalendar(year=isoyear,
                                     week=isoweeknumber,
                                     day=isoweekday)
# Class Attribute
datetime.date.min  # Earliest date
datetime.date.max  # Latest date
datetime.date.resolution  # Smallest value difference
# Read-only Attribute
date.year
date.month
date.day
# Operation
date = date + delta  # Apply delta
date = date - delta  # Apply -delta
delta = date - date
# Boolean
date  # `True`
date == date
date != date
date < date
date <= date
date > date
date >= date
# Method
date = date.replace([year=year][,  # Return new date
                    month=month][,
                    day=day])
struct_time = date.timetuple()  # Return `time.struct_time` object
ordinal = date.toordinal()  # Jan 1 of year 1 has ordinal 1
weekday = date.weekday()  # Mon is 0 and Sun is 6
isoweekday = date.isoweekday()  # Mon is 1 and Sum is 7
isoyear, isoweeknumber, isoweekday = date.isocalendar()
isoformat = str(date); date.isoformat()  # 'YYYY-MM-DD'
ctime = date.ctime()  # `time.ctime()` format
string = date.strftime('format')  # str from time
f'{date:format}'

# datetime
# Construct
dt = datetime.datetime(year=year,
                       month=month,
                       day=day[,
                       hour=hour][,
                       minute=minute][,
                       second=second][,
                       microsecond=microsecond][,
                       tzinfo=tzinfo][,
                       fold=1])  # Disambiguate repeated time: 0: earlier; 1: later
dt = datetime.datetime.today()|now()|now(tz=None)  # Naive now
dt = datetime.datetime.now(tz=tz)  # Aware now
dt = datetime.datetime.now(tz=datetime.timezone.utc)  # Aware UTC now
dt = datetime.datetime.utcnow()  # Naive UTC now
dt = datetime.datetime.fromtimestamp(timestamp=timestamp[,
                                     tz=tz])
dt = datetime.datetime.fromtimestamp(timestamp=timestamp,  # Aware UTC time
                                     tz=datetime.timezone.utc)
dt = datetime.datetime.utcfromtimestamp(timestamp=timestamp)  # Naive UTC time
dt = datetime.datetime.fromordinal(ordinal)  # Jan 1 of year 1 has ordinal 1; Naive
dt = datetime.datetime.combine(date=date,
                               time=time[
                               tzinfo=tzinfo])
dt = datetime.datetime.fromisoformat('YYYY-MM-DD[.HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]]')
dt = datetime.datetime.fromisocalendar(year=isoyear,
                                       week=isoweeknumber,
                                       day=isoweekday)
dt = datetime.datetime.strptime(date_string, 'format')  # `str` parsed datetime
# Class Attribute
datetime.datetime.min  # Earliest datetime
datetime.datetime.max  # Latest datetime
datetime.datetime.resolution  # Smallest value difference
# Read-only Attribute
dt.year
dt.month
dt.day
dt.hour
dt.minute
dt.second
dt.microsecond
dt.tzinfo
dt.fold  # Disambiguate repeated time: 0: earlier; 1: later
# Operation
dt = dt + delta  # Apply delta
dt = dt - delta  # Apply -delta
delta = dt - dt
# Boolean
dt  # `True`
dt == dt
dt != dt  # Naive != Aware
dt < dt
dt <= dt
dt > dt
dt >= dt
# Method
date = dt.date()
time = dt.time()  # `tzinfo` is `None`
time = dt.timetz()  # `tzinfo` is `dt.tzinfo`
dt = dt.replace([year=year][,  # Return replaced datetime
                month=month][,
                day=day][,
                hour=hour][,
                minute=minute][,
                second=second][,
                microsecond=microsecond][,
                tzinfo=tzinfo|None][,  # No time conversion performed, only `tzinfo` is replaced
                fold=fold])  # Disambiguate repeated time: 0: earlier; 1: later
dt = dt.astimezone([tz=tz])  # Return the same time in `tz` (default to system time zone); Naive datetime are presumed to be in system time zone
dt = dt.astimezone(tz=datetime.timezone.utc)  # Return the same time in utc
delta = dt.utcoffset()  # Return `self.tzinfo.utcoffset(self)` or `None` representing utc offset; `dt == (dt.astimezone(tz=datetime.timezone.utc) + dt.utcoffset()).replace(tzinfo=dt.tzinfo)`
delta = dt.dst()  # Return `self.tzinfo.dst(self)` or `None` representing dst offset
tzname = dt.tzname()  # Return `self.tzinfo.tzname(self)`, a `str` or `None` representing time zone name
struct_time = dt.timetuple()  # Return `time.struct_time` object
struct_time = dt.utctimetuple()  # Return `time.struct_time` object in utc (if aware), or in naive (if naive)
ordinal = dt.toordinal()  # Jan 1 of year 1 has ordinal 1
timestamp = dt.timestamp()
weekday = dt.weekday()  # Mon is 0 and Sun is 6
isoweekday = dt.isoweekday()  # Mon is 1 and Sun is 7
isoyear, isoweeknumber, isoweekday = dt.isocalendar()
isoformat = dt.isoformat([sep='SEP'][,  # YYYY-MM-DD[.HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]]
                         timespec='hours|minutes|seconds|milliseconds|microseconds'])
isoformat = str(dt)  # YYYY-MM-DD[ HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]]
ctime = dt.ctime()  # `time.ctime()` format
string = dt.strftime('format')  # `str` from time
f'{dt:format}'

# time
# Construct
time = datetime.time([hour=hour][,
                     minute=minute][,
                     second=second][,
                     microsecond=microsecond][,
                     tzinfo=tzinfo][,  # `None` or default is naive
                     fold=fold])  # Disambiguate repeated time: 0: earlier; 1: later
time = datetime.time.fromisoformat('HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]')
# Class Attribute
datetime.time.min  # Earliest time
datetime.time.max  # Latest time
datetime.time.resolution  # Smallest value difference
# Read-only Attribute
time.hour
time.minute
time.second
time.microsecond
time.tzinfo
time.fold  # Disambiguate repeated time: 0: earlier; 1: later
# Boolean
time  # `True`
time == time
time != time  # Naive != Aware
time < time
time <= time
time > time
time >= time
# Method
time = time.replace([hour=hour][,  # Return replaced time
                    minute=minute][,
                    second=second][,
                    microsecond=microsecond][,
                    tzinfo=tzinfo|None][,  # No time conversion performed, only `tzinfo` is replaced
                    fold=fold])  # Disambiguate repeated time: 0: earlier; 1: later
delta = time.utcoffset()  # Return `self.tzinfo.utcoffset(None)` or `None` representing utc offset
delta = time.dst()  # Return `self.tzinfo.dst(None)` or `None` representing dst offset
tzname = time.tzname()  # Return `self.tzinfo.tzname(None)`, a `str` or `None` representing time zone name
isoformat = time.isoformat([timespec='hours|minutes|seconds|milliseconds|microseconds'])  # HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]
isoformat = str(time)  # HH[:MM[:SS[.fff[fff]]]][[+-]HH:MM[:SS[.ffffff]]]
string = time.strftime('format')  # `str` from time
f'{time:format}'
```
