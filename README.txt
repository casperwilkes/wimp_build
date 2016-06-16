// Ready host machine //
1. We want to edit the hosts file to map the static IP to a dns so we can call 
   local server by name instead of by IP address
	- On host (your) machine go to text editor of choice (np++ or Sublime, or whatever you use), right click->run as administrator
	- Then browse to and open: C:/Windows/System32/drivers/etc/hosts 
	- Drop in this line: 
		192.168.100.10	wimp_build.xcom 
			(using xcom to remind us it's a testing environment and not a legit site) 
	- under: 
		127.0.0.1       localhost
	- (If you can't access the Hosts file, you will have to use 192.168.100.10 as the url instead of the dns entry)

// Boot vm up //
2. Open cmd and type: vagrant up

// Check example site //
3. After VM boots up, and vagrant script outputs "Done installing .NET 4.5!"
	- type into host browser address bar:
		 http://wimp_build.xcom
	- Confirm sample site came up

// Guest additions //
4. Go to Virtualbox's Gui toolbar
	- Click on Devices -> Insert Guest Additions CD Image
	- Install / update if necessary

// Make sure server is ready //
4. On server, click first icon in task bar (Server Manager). 
	- Confirm all modules are operational (green status)

// Begin installations //
5. In upper right hand corner, click tools:
	- Navigate to Internet Information Services (IIS) Manager
	- A new IIS window will open up

6. Click on WIMP-DEV in the right hand connections panel
	- A prompt window will appear asking you to install Web Platform Installer. 
	- Click yes
	- A new browser window will open up in IE
	- Click the green download button
	- Click run in the prompt that appears at the bottom of the screen

// .NET install //
7. Search for ".net 3.5"
	- Click add on ".Net Framework 3.5 SP 1"
	- Click install

// Update VM //
8. Go to Control Panel
	- System and Security
	- Under Windows Update -> check for updates
	- Install updates

9. Restart machine
	- Might have to do a few more updates and this could take a little bit of time

// PHP & MYSQL install //
10. Search for "php 5.4.45" and "mysql 5.5"
	- Click Add for both installations
	- Click Install

11. A new prompt comes up for Root access for MySQL
	- Add password ("password" will work for this, but you can use whatever you wish)
	- Click continue 
	- Click Accept (This will install all components)
		(After the install, its going to say Microsoft SQL Drivers failed. This is ok because we're not using MSSQL.)
	- Close IIS panel if it's open

12. Go Back to IIS panel
	(A new icon for PHP Manager should have appeared)
	- Click on PHP Manager module
	- Click on View Recommendations for PHP Configuration
	- Check boxes for both recommendations
	- Click Ok to fix
	- Go back to PHP manager
	- Click on Manage all settings under PHP Settings
	- Go to display_errors, double click and change value from "Off" to "On"
	- Go to error_reporting, double click and change value from "E_ALL & ~E_DEPRECATED & ~E_STRICT" to "E_ALL"
		(This will allow us to see what errors are kicked up while developing)
	- Go to date.timezone, double click and change value from "America/Los_Angeles" to "America/New_York"
	- Install xdebug
		- Copy php_xdebug.dll from the after_build folder
		- Paste in C:\Program Files (x86)\PHP\v5.4\ext
		- Go back to PHP Manager
		- Click on Manage all settings
		- Click the Add... button in the upper right hand corner of the manager
			Name: zend_extension
			Value: "C:\Program Files (x86)\PHP\v5.4\ext\php_xdebug.dll" <= Make sure to add quotes
			Section: XDebug
		- You should see a new entry added with the values we just put in
		- We'll confirm during FTP install
	
// FTP Install //
13. Click on Server Manager
	- Go to (2) Add roles and features
	- Next, Next, Next
	- Go down to Web Server (IIS)
		- Click on FTP Server
			- and FTP Server
			- and FTP Extensibility
	- Install

14. restart vm

15. Open filezilla
	- Put credentials in from /after_build/configs/filezilla.txt
	- Connect
	- In host panel
		- Navigate to web_build
			- Upload Test directory to wwwroot
	- In Host web browser
		- Navigate to:
			- http://wimp_build.xcom/test/phpinfo.php
		- Make sure phpinfo page appears
		- Search page for: xdebug 
		- Confirm installation of extension

// PHPMyadmin install // 
16. There are 2 options to getting PhpMyAdmin uploaded. 
	- Either
		- Use Filezilla to upload the directory like we did the test directory
	- Or
		- On VM navigate to:
		- C/vagrant/upload/pma
		- Copy pma directory
		- Paste directory to:
		- C/inetpub/wwwroot so that location will be:
			- C/inetpub/wwwroot/pma
	- Give administrator full privledges to pma directory
		- Right click -> Properties -> Security tab -> Edit button
		- Give Users and IIS_IUSRS full control
		- Apply
	- In host browser bar, go to:
		- http://wimp_build.xcom/pma
		- Enter credentials
			Username: root
			Password: password (or whatever you chose when you created mysql)
		- then click "SQL" button and enter the PMA scripts from below
	- Create controluser by clicking on SQL tab on home page
		- Copy SQL query found in: 
			after_build/configs/sql/create_controluser.sql
		- Paste query in window and run
	- Grant controluser access by clicking on the SQL tab again
		- Copy SQL query found in: 
			after_build/configs/sql/grant_control.sql
		- Paste query in window and run
	- Log out and back in
	- Create PMA tables
		- Click on Import tab on main page
		- Click choose file button
		- Navigate to:
			after_build/configs/sql/create_tables.sql
		- Click Go
		- A query output window should appear saying 20 or so rows were affected and a few new databases should appear
	- Log back out and back in
	- Create a throwaway test db
		- Click Databases tab
			- Create database: testing_data
		- Click on newly created database from db list panel
		- Click Import tab
		- browse to:
			after_build/configs/sql/testing_data.sql
		- Click Go
		- Confirm DB and data were imported
	- Drop test db
		- In database list, click on testing_data
		- Click on Operations tab
		- Click on: Delete the database(DROP)
		- Confirm test db was destroyed
		
// Cleanup // 
16. At this point, you should probably run another update just to be completely updated.
    There are 2 service packs for the server. the first should have been dowloaded when you 
    did the first update.
