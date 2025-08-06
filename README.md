# Group5Automated-package-system-update
Creating Automated Package system update to check the updating and upgrading is working
https://github.com/Godfred901/Group5Automated-package-system-update.git
 Creating the Update Script: update_system.sh
This script will update the system, log the results, and handle errors. You can include the following in your script:

bash
Copy
Edit
#!/bin/bash

# Update Log File
LOG_FILE="/var/log/apt/update.log"
ERROR_LOG="/var/log/apt/error.log"

# Time of update
echo "Update started at $(date)" >> $LOG_FILE

# Update and upgrade system
{
    echo "Running apt update..."
    apt update
    echo "Running apt upgrade..."
    apt upgrade -y
} >> $LOG_FILE 2>> $ERROR_LOG

# Check if updates were successful
if [ $? -eq 0 ]; then
    echo "Update successful at $(date)" >> $LOG_FILE
else
    echo "Update failed at $(date)" >> $ERROR_LOG
fi
This script updates the system with apt update and apt upgrade -y.

The results (both output and errors) are logged in /var/log/apt/update.log and /var/log/apt/error.log.

If the update fails, an error message is logged.

2. Making the Script Executable
After creating the script, you need to make it executable:

bash
Copy
Edit
chmod +x ~/myproject/scripts/update_system.sh
3. Scheduling the Script with Cron
You can schedule the script to run weekly using cron.

Edit the cron jobs:

bash
Copy
Edit
crontab -e
Add a line for the weekly execution of your script:

bash
Copy
Edit
0 3 * * 0 /bin/bash /home/user/myproject/scripts/update_system.sh
This cron job runs every Sunday at 3:00 AM.

Verify cron jobs with:

bash
Copy
Edit
crontab -l
4. Audit Logs in /var/10g/apt
You mentioned auditing logs in /var/10g/apt. You can configure the script to write logs to that directory:

bash
Copy
Edit
LOG_FILE="/var/10g/apt/update.log"
ERROR_LOG="/var/10g/apt/error.log"
Ensure that the directory exists and has appropriate permissions:

bash
Copy
Edit
sudo mkdir -p /var/10g/apt
sudo chmod 755 /var/10g/apt
5. Optional: Email Alerts with Mailutils
If you'd like to receive email alerts, you can install mailutils and modify the script to send an email when updates succeed or fail.

Install mailutils:

bash
Copy
Edit
sudo apt-get install mailutils
Then, update the script to send an email upon success or failure:

bash
Copy
Edit
if [ $? -eq 0 ]; then
    echo "System update was successful." | mail -s "System Update Success" your-email@example.com
else
    echo "System update failed." | mail -s "System Update Failure" your-email@example.com
fi
6. Documentation: plect_readme.md
Create a markdown file to explain the steps of setting up and running the script.

markdown
Copy
Edit
# Automated Package Update System

## Description
This system automates the process of checking for and applying system updates using the `apt` package manager. It logs the results and runs weekly through cron to ensure that the system remains secure with up-to-date packages.

## Files
- `update_system.sh`: A shell script for updating and upgrading the system packages.
- `plect_readme.md`: Documentation for setting up and running the script.

## Installation
1. Place the `update_system.sh` script in `~/myproject/scripts/`.
2. Make the script executable:
   ```bash
   chmod +x ~/myproject/scripts/update_system.sh
Schedule the script to run weekly using cron.

Running the Script Manually
To run the update script manually, execute the following command:

bash
Copy
Edit
bash ~/myproject/scripts/update_system.sh
Logs
Logs are stored in /var/10g/apt/:

update.log: Records the output of the update process.

error.log: Records any errors that occur during the update process.

Optional: Email Alerts
To enable email alerts, ensure mailutils is installed and configured.

markdown
Copy
Edit

### 7. **Screenshots**

Capture the output of the log and cron to show the system's behavior.

- `tail ~/myproject/logs/update.log`
- `crontab -l`

This should help you get your automated update system set up. If you need any more details, feel free to ask!









