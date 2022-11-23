brief note for creating datestrings with timezones

---------------

**python**

move from implicit UTC to Eastern:

```python
import datetime
from zoneinfo import ZoneInfo 
# python 3.9+
# also see https://stackoverflow.com/a/63575962 
assert datetime.datetime.now().tzinfo == None
now = datetime.datetime.now().replace(tzinfo=datetime.timezone.utc).astimezone(ZoneInfo("US/Eastern"))

now.strftime('%m_%d_%H_%M_%S_%f'), now.strftime('%I:%M %p'), now.strftime('%Y_%m_%d__%I-%M_%p_%S_%f')
```

`('11_23_15_08_13_875273', '03:08 PM', '2022_11_23__03-08_PM_13_875273')`

remove some precision

```python
now.strftime('%Y_%m_%d__%I-%M_%p_%S_%f')[:-3]
```

`2022_11_23__03-08_PM_13_875`

---------------

**logfile redirection with `date` via `tee`**

for year-moth-day-hour, e.g., `2022-11-23-15`, prepend the format specifier with `+`, e.g., `date '+%Y-%m-%d-%H'`

with `tee`,

```bash
python whatever 2>&1 |tee logname_$(date '+%Y_%m_%d__%I-%M_%p').log
```

corresponds to `2022_11_23__03-23_PM`

to specify a timezone:

```bash
python whatever 2>&1 |tee logname_$(env TZ=America/New_York date '+%Y_%m_%d__%I-%M_%p').log
```

(using `env` is a bit more portable)

see https://unix.stackexchange.com/a/48104 for more details
