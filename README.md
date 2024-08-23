# lern-logwatch

`logwatch` is a powerful tool that can help you monitor and summarize the log files on your Linux server. It provides detailed reports that can be sent via email, making it easier to stay informed about what's happening on your system, including SSH login attempts.

### Steps to Install and Configure `logwatch`

#### 1. **Install Logwatch**
   - On Debian/Ubuntu:
     ```bash
     sudo apt-get install logwatch
     ```
   - On CentOS/RHEL:
     ```bash
     sudo yum install logwatch
     ```

#### 2. **Basic Configuration**
   - The default configuration is usually sufficient for most cases, but you can customize it if needed.
   - The main configuration file is located at:
     ```bash
     /usr/share/logwatch/default.conf/logwatch.conf
     ```
   - To customize, create a copy of this file in `/etc/logwatch/conf/`:
     ```bash
     sudo cp /usr/share/logwatch/default.conf/logwatch.conf /etc/logwatch/conf/logwatch.conf
     ```
   - Open the configuration file to edit:
     ```bash
     sudo nano /etc/logwatch/conf/logwatch.conf
     ```
   - Key settings you might want to adjust:
     - **Output:** Where the report is sent (`stdout`, email, or file).
       ```bash
       Output = mail
       ```
     - **MailTo:** The email address to send the report to.
       ```bash
       MailTo = your-email@example.com
       ```
     - **Detail:** The level of detail in the report (`Low`, `Med`, `High`).
       ```bash
       Detail = Low
       ```
     - **Range:** The period covered by the report (`yesterday`, `today`, `all`).
       ```bash
       Range = yesterday
       ```

#### 3. **Run Logwatch Manually**
   - You can run `logwatch` manually to generate a report:
     ```bash
     sudo logwatch --detail Low --mailto your-email@example.com --service sshd --range today
     ```
   - This command will send a summary of today's SSH logins to the specified email address.

#### 4. **Automate Logwatch with Cron**
   - To automate daily reports, you can set up a cron job:
     - Open the crontab for editing:
       ```bash
       sudo crontab -e
       ```
     - Add the following line to run `logwatch` daily at 2 AM:
       ```bash
       0 2 * * * /usr/sbin/logwatch --output mail --mailto your-email@example.com --detail Low
       ```
     - Save and exit the editor.

### Viewing Logwatch Reports
   - The report you receive via email (or file) will contain detailed information about various services, including SSH, such as:
     - Number of failed login attempts.
     - Users that attempted to log in.
     - IP addresses used in the login attempts.
     - Any unusual activity.

### Customizing Logwatch Reports for SSH
   - If you only want to monitor SSH-related activities, you can specify the `sshd` service in the logwatch command or in the configuration:
     ```bash
     sudo logwatch --service sshd --range yesterday --detail Low
     ```
   - This will limit the report to just SSH-related log entries.

By following these steps, `logwatch` will provide you with a convenient way to monitor your SSH logs and keep an eye on potential security threats.
