# RailsBricks 3.0.3-athalas with MySQL support
# Getting Started guide

Guide author: David Camarena
Original date: Nov 23, 2014
Revision: 2


RailsBricks is an excellent Rails-based GUI to use for Ruby projects. It has neat interface, it is fast, easy to deploy, and it has an assistant and some command-line tools aimed at letting you develop and deploy rails sites faster. It has been created by Nico Schuele.

I've recently added MySQL support to RailsBricks and wrote that guide aimed at helping those users wanting to configure and use MySQL on RailsBricks.

You will require RailsBricks 3.0.3-athalas or later in order to have MySQL support. Also, this guide will help you to complete all the needed requirements. 


## Requirements

* An OS matching requirements for Ruby & RailsBricks, either OS X, Linux or Windows (latest is unofficial but works ok)
* A standard working Ruby environment (ruby, rake, bundle...), tested with 1.9.3, 2.0.x, 2.1.x
* Required development tools to compile native gems on your system (more on this after)
	* DevKit properly installed on Windows
	* Standard C/C++ development tools (gcc, make, etc.) on Linux
* Git
* RailsBricks 3.0.3-athalas or later (with the new MySQL support)


## Objectives

* Meet the requirements to be able to install the native gem "mysql2" in the system for MySQL support
* Install and deploy RailsBricks 3 with MySQL


### Required Steps

#### 1. Proper working Ruby setup

This tutorial assumes that you already have a proper working Ruby setup on your target system.

Any recent version of Ruby should do ok, but I've only tested with version 1.9.3 and versions 2.0.x and 2.1.x. **Older versions aren't recommended nor supported at all**.

- Linux: go the "rvm" way if possible (http://rvm.io/). Otherwise, use your package manager (yum, apt-get, etc.).

- Windows: use RailsInstaller on Windows.

Make sure before going forward in this tutorial to have properly installed Ruby and its usual toolset (rake, bundle, ...). 


#### 2. "mysql2" native gem compilation

"mysql2" is a native gem. It is not 100% written in pure Ruby code, it also has some native (ie.: C language) code which must be compiled into appropiate binaries for your target system before usage. 

	https://github.com/brianmario/mysql2

Nonetheless, I will attempt to give you some basics to build this.


2.1. Native gem compilation on Linux

On a Linux development machine this is more or less straightforward. 

2.1.1. Make sure you have a proper C/C++ standard development environment with typical tools (gcc, make, etc.)

If you haven't yet: 

	On Ubuntu / Debian: 	$ sudo apt-get install build-essentials ruby-dev
	On CentOS / RedHat:		$ sudo yum groupinstall 'Development Tools'

2.1.2. Install the required MySQL development libraries with your package manager

Attempting to install the "mysql2" gem without the required libraries/tools will fail. You will need one of these groups, depending on your system (run them as root or use sudo).

	Ubuntu / Debian based
	
	* required: libmysqlclient-dev
	
	# apt-get update
	# apt-get install libmysqlclient-dev
	# gem install mysql2

		(successfully tested on Ubuntu 14.04)


	CentOS / RedHat based
	
	* required: 
	
	# yum install mysql-devel

		(successfully tested on CentOS 6.5)
	
2.1.3. Install the mysql2 gem

The last step on Linux is to launch the usual gem installation command, sit down and relax while the gem builds and installs. 

	# gem install mysql2
	
	Building native extensions.  This could take a while...
	Successfully installed mysql2-0.3.17
	Parsing documentation for mysql2-0.3.17
	Installing ri documentation for mysql2-0.3.17
	Done installing documentation for mysql2 after 0 seconds
	1 gem installed 

That's all on Linux.


2.2. Native gem compilation on Windows

On Windows the process is slightly different. 

2.2.1 Install DevKit

DevKit is a MinGW/Cygwin subset of the basic Linux development tool set ported to Windows and adapted to work with Ruby to allow compilation of native gems on Windows using the usual toolset (gcc, make, ...). 

	Download DevKit from here:					http://rubyinstaller.org/add-ons/devkit/
	Install it using the available tutorial: 	https://github.com/oneclick/rubyinstaller/wiki/Development-Kit
	(a shorter tutorial found also here):		http://corlewsolutions.com/articles/article-22-install-ruby-devkit-on-windows-7

Make sure it is properly installed, you can try to compile some simple native gem such as "json" as the tutorial suggests.

2.2.2. Install MySQL Connector for C

Next you will need to download the "MySQL Connector for C" libraries for your platform (32-bit or 64-bit, check which Ruby platform do you have with "ruby -v"). I recommend you to download it as a .zip file from the official website.

	http://dev.mysql.com/downloads/connector/c/

	(file may be like "mysql-connector-c-6.1.2-win32.zip", for 32-bit Ruby installation)
	
Unpack it on a simple, short folder with NO spaces in its path, for example:

	c:\mysql-connector-c-6.1.2-win32\

After unpack you should be able to see:

	c:\mysql-connector-c-6.1.2-win32\
		|- bin
		|- docs
		|- include
		|- lib
		|- COPYING
		|- README

If you have them, you're set. 

2.2.3. Install the mysql2 gem (Windows)

Now you can attempt to compile and install the gem:

	c:\> gem install mysql2 --platform=ruby -- --with-mysql-dir=c:\mysql-connector-c-6.1.2-win32
	                                                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	
	Temporarily enhancing PATH to include DevKit...
	Building native extensions with: '--with-mysql-dir=c:\mysql-connector-c-6.1.2-win32'
	This could take a while...
	Successfully installed mysql2-0.3.17
	Parsing documentation for mysql2-0.3.17
	Done installing documentation for mysql2 after 1 seconds
	1 gem installed


#### 3. Install RailsBricks

	Please Note: 

	a) In order to use the new MySQL support on RailsBricks you will need at least RailsBricks version 3.0.3-athalas or later.
	
	b) Mysql2 gem is a requirement ONLY if you plan to use MySQL option for RailsBricks. You don't require it if you just want 
	   to use SQLite or PostgreSQL.

After all the requirements have been met you can install the RailsBricks gem the usual way.

	gem install railsbricks


#### 4. Deploy your RailsBricks applications with MySQL

Now you can use the usual RailsBricks command-line assistant "rbricks" to deploy your new RailsBricks site(s). The tool has been enhanced to include MySQL support now and it will ask you some more questions if you choose that specific database.

To create a MySQL-based RailsBricks site:

a) Make sure you have a MySQL user with proper permissions to create the database (create it and assign proper permissions if required)

b) Then launch "rbricks" as usual to create a new site at your document root folder (www, htdocs, html, wwwroot, ...)

	rbricks -n

c) The assistant will ask you the usual questions. You can select MySQL now as the database and the assistant will ask a few more questions if you do so.

	MySQL

	d) the usual connection settings for it will be asked with smart defaults

		Host, port, user, pass, db name, etc.

	e) (if you are on Windows AND if MySQL have been selected) you will be prompted for the path where you have extracted 
		the "MySQL Connector for C" files for your platform. On Windows, You will also be expected to have DevKit properly 
		installed beforehand. See above sections for details. 

		The specified path will be validated and used to compile the native mysql2 gem automatically for you if valid.

Once you complete answers to all the questions RailsBricks will build the site for you. 


#### 5. Enjoy your MySQL-based site

As usual, enter the new folder project and run

	rails server

Then point your web browser at the usual location

	http://localhost:3000
	
And there you have! A complete, working RailsBricks site with transparent MySQL support.


Enjoy!


