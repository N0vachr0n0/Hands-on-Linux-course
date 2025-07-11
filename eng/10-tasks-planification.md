# Introduction

In this chapter, we will explore **task scheduling** in Linux, an essential concept for automating repetitive processes and optimizing system management. Whether for backups, temporary file cleanups, or periodic checks, task scheduling is a key skill for any system administrator or Linux user. We will focus on **cron**, the most widely used and powerful scheduling tool in Linux. Get ready to become a master of automation, dear padawan!

---

## Prerequisites

Same old story. 😉

---

# What is Task Scheduling?

**Task scheduling** allows you to automatically execute commands or scripts at specific times or regular intervals. Imagine an alarm clock that, instead of ringing, executes a task on your system: backing up files, sending reports, or cleaning folders. This frees you from repetitive tasks and ensures your system stays up-to-date without manual intervention.

In Linux, several tools exist for scheduling tasks:
- **cron**: The standard, flexible, and robust tool, perfect for periodic tasks.
- **at**: For one-time tasks at a specific time.
- **anacron**: Ideal for tasks that need to run even if the system was off at the scheduled time.

In this course, we will focus on **cron**, the preferred choice for recurring automation.

---

## Cron

### What is cron?

**cron** is a **daemon** (a program that runs in the background) that reads configuration files called **crontabs** to execute scheduled tasks. Each user can have their own **crontab**, and there is also a **system crontab** for global tasks. Think of **cron** as an orchestra conductor who ensures each task is played at the right time.

### Crontab Structure

The **crontab** file is composed of lines, each representing a scheduled task. Each line follows this structure with **six fields**:

```

 * * * * * command 2\>&1 \>/dev/null
 ^ ^ ^ ^ ^
 | | | | |
 | | | | +­­ day of the week 0­6
 | | | +­­­­ month 1­12
 | | +­­­­­­ day of the month 1­31
 | +­­­­­­­­ hour 0­23
 \+­­­­­­­­­­ minute 0­59

````

**Note**: **/dev/null** is a file that points to nothing, so command output is sent nowhere. This allows you to discard the output so it's not logged - especially since, without redirection, standard output is sent by email to root. That said, you can redirect the output to a log file to keep informed of errors that may occur during a scheduled task or use the `chronic` command.

Each field can contain:
- A specific value (e.g., `15` for 15 minutes).
- An asterisk (`*`) for "all possible values".
- A list (e.g., `1,3,5`).
- A range (e.g., `1-5`).
- An interval (e.g., `*/5` for every 5 minutes).

### Cron Task Examples

Here are concrete examples for better understanding:

- **Every day at 2:30 PM**:
  ```bash
  30 14 * * * /usr/bin/backup.sh
  ```

Executes the `backup.sh` script at 2:30 PM every day.

  - **Every Monday at 10:00 AM**:

    ```bash
    0 10 * * 1 /usr/bin/clean_logs.sh
    ```

    Executes `clean_logs.sh` every Monday at 10:00 AM.

  - The 1st of each month at 12:00 PM:

    ```bash
    0 12 1 * * /usr/bin/monthly_report.sh
    ```

    Executes `monthly_report.sh` on the 1st day of each month at noon.

  - Every hour:

    ```bash
    0 * * * * /usr/bin/check_status.sh
    ```

    Executes `check_status.sh` at the beginning of every hour (00:00, 01:00, etc.).

  - Every day at midnight:

    ```bash
    0 0 * * * /usr/bin/daily_backup.sh
    ```

    Executes `daily_backup.sh` every day at 00:00.

<br\>

**Educational tip**: Use the online tool [crontab.guru](https://crontab.guru/) to test and visualize cron expressions in real time.

### Editing the Crontab File

To manage a user's cron tasks, use the `crontab` command:

`crontab -e`: Opens the user's crontab file in the default editor (e.g., nano or vi).
`crontab -l`: Displays the scheduled tasks in the user's crontab.
`crontab -r`: Deletes all scheduled tasks for the user (be careful, irreversible\!).

<br>
<br>

**To test 👨🏾‍💻👩🏾‍💻:**

  - Open your terminal
  - Execute:
    ```bash
    $ crontab -e ## Opens the crontab
    ```
  - Add a line: `*/10 * * * * /home/user/script.sh`.
  - Save and exit. The cron daemon automatically detects changes.

<br>

(( I hope you created the script file 😝))

**Note**: The system crontab (for root or global tasks) is located in `/etc/crontab` or `/etc/cron.d/`. It includes an additional field to specify the user executing the command.

### Time Specifiers

To simplify certain schedules, cron offers special specifiers:

  * `@reboot`: Executes the task once at system startup.
  * `@yearly`: Executes once a year (equivalent to `0 0 1 1 *`).
  * `@monthly`: Executes once a month (equivalent to `0 0 1 * *`).
  * `@weekly`: Executes once a week (equivalent to `0 0 * * 0`).
  * `@daily`: Executes once a day (equivalent to `0 0 * * *`).
  * `@hourly`: Executes once an hour (equivalent to `0 * * * *`).

**Example:**

To initialize a service at startup:

```bash
@reboot /usr/bin/start_service.sh
```

## Tips and Best Practices

To avoid common errors and optimize your cron tasks:

  - **Use absolute paths**: Environment variables in cron are limited. Always specify full paths (e.g., `/usr/bin/echo` instead of `echo`).

  - **Redirect output**: Cron tasks send their output (stdout/stderr) by email to the user. Redirect them to a file to avoid this:

    ```bash
    0 0 * * * /usr/bin/script.sh >> /var/log/script.log 2>&1
    ```

  - **Test your scripts first**: Manually run your script to ensure it works correctly before adding it to the crontab.

  - **Check permissions**: Make sure scripts are executable (`chmod +x script.sh`) and that the cron user has the necessary permissions.

  - **Use logs**: Record the results of your tasks in log files for easier debugging.

  - **Monitor cron**: Check system logs (`/var/log/cron` or `/var/log/syslog`) to see if your tasks are running correctly.

  - **Avoid overlaps**: If a task can take time, use a lockfile to prevent multiple simultaneous executions.

<br>

**Example script with logging**:

```bash

#!/bin/bash
echo "Task started at $(date)" >> /var/log/task.log
# Task here
echo "Task completed at $(date)" >> /var/log/task.log
```

### Quick Mini Explanation\!

**"Avoid overlaps"**: What exactly is that?

When you schedule a task at regular intervals (e.g., via cron), it can happen that the same task is relaunched before the previous one finishes.
This can cause problems: duplicate processing, system overload, unexpected errors...

Imagine you have a cron job that runs every 5 minutes, but sometimes it takes more than 5 minutes to finish.
Result: two instances of the same script running at the same time → possible conflict\!

**Question:** How to avoid this?

**Answer:** Use a **lockfile**.

**Principle of a script using a lockfile:**

1.  At the beginning of the script:

    It checks if a lock file (`/var/lock/myscript.lock`, for example) exists.

2.  If the file exists:

    The script stops → another instance is already running.

3.  If the file does not exist:

    It creates the lock file.
    It executes the task.

4.  At the end:

    It deletes the lock file.

**Simple example of a Bash script:**

```bash
#!/bin/bash

LOCKFILE="/var/lock/myscript.lock"

# Check if the lock file exists
if [ -f "$LOCKFILE" ]; then
  echo "Another instance is already running. Exiting."
  exit 1
fi

# Create the lock file
touch $LOCKFILE

# Main task code
echo "Task in progress..."
sleep 10

# Delete the lock file at the end
rm -f $LOCKFILE
```

# Training ⚔️

To put your cron knowledge into practice, here are three progressive exercises to master task scheduling.

## Exercise 1: Simple Cron Task

Create a cron task that runs a script every 5 minutes to record the date and time.

## Exercise 2: Daily Task with Error Handling

Schedule a daily task to clean temporary files and manage errors in a log file.

## Exercise 3: Advanced Task with Dependencies

Configure two dependent cron tasks for a backup and a verification.

---
---

## Feedback

> ENG: Please give us your feedback about this chapter.

> FR: Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 https://forms.gle/Br22WxcwgJSeLGkW9
