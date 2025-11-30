Tags:
- [[Linux]]
---
## what
- **crontab**: text file storing schedules, indicating what command to run and at what interval
- **cron**: daemon that runs each minute, checking the crontabs to see which commands to run

## format
- 5 space separated fields, followed by the command to run
    - minute, hour, day, month, day-of-week
    - e.g. `0 13 * * 1-5 echo "hi"` runs the echo at 1pm from monday to friday
- `*`: wildcard, matches any value for that field
- `-`: range: e.g. `1-5` matches any number from 1 to 5 inclusive
- `,`: list: e.g. `3,5` matches 3 and 5
- `/`: step interval: e.g. `*/5` => every 5; `1-10/2` => every 2 between 1-10

## how to configure
- check: `crontab -l`
- edit: `crontab -e`
- remove: `crontab -r`
---
## References
- 