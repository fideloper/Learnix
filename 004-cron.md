# CRON
Cron daemon runs in background from startup to shutdown.
It is run by `sh` by default. This is configurable.

Use the crontab command rather than editing cron files (often found in /var/spool/crons) directly. Crontab notifies cron when crontabs change. Editing files directly circumvents this.

Most versions allow logging. Some log if the log file is present. Some log based on config values. Some log to syslog. Check: /var/cron/log, /var/adm/cron/log. Logging should generally be turned off, as they can grow quite large quickly. Ubuntu uses the syslog.

Each line in a crontab contains 6 fields. The first five tell when to run the command. The command is the 6th field. White space separates them all. The command, however, CAN have white space.
*minute hour dom month weekday command*

| Field     | Description        | Range               |
| :---------| :----------------- | :------------------ |
| *minute*  | Minute of the hour | 0 to 59             |
| *hour*    | Hour of the day    | 0 to 23             |
| *dom*     | Day of the month   | 1 to 31             |
| *month*   | Month of the year  | 1 to 12             |
| *weekday* | Day of the week    | 0 to 6 (0 = Sunday) |


**Notes:**

1. A star matches *everything*
2. A single integer matches that exactly
3. Ranges are possible, i.e. 1-5
4. Range and step value possible, i.e. 1-10/2 (Linux only)
5. A comma separated list of integers, ranges possible

**Example:**

	45 10 * * 1-5  #10:45am, Monday - Friday

**Ambiguity!:**

	0,30 * 13 * 5  #Every half hour on Friday AND every half hour on the 13th of the month!
	
The command can be any valid shell command and should *not* be quoted. Note that this command is unaware of environmental variables. It does not load in ~/.profile or ~/.bash_profile files.



