# Init script for Apache Tomcat #

Written by Adam Sharp  
Based (heavily) on script found at [http://tldp.org/HOWTO/MMBase-Inst-HOWTO/x321.html](http://tldp.org/HOWTO/MMBase-Inst-HOWTO/x321.html).

## Description ##

This is an init script for Apache Tomcat, tested for use on Amazon Linux AMI, and should work on CentOS and other RPM based systems such as Red Hat.

Besides configuration changes, the main change I've made from the source script was to use `catalina.sh start` and `catalina.sh stop` instead of `startup.sh` and `shutdown.sh`, respectively. I also set the `CATALINA_PID` variable so that `catalina.sh` would correctly create the PID file, and `service tomcat status` would correctly report whether tomcat is running. (Previously would get errors such as "tomcat dead but subsys locked").

## Installation ##

To install the script as-is, assuming you have Tomcat v6.0.33 installed at `/usr/local/apache-tomcat-6.0.33` and the `java` executable located at `/usr`:

1. Copy the `tomcat` script to `/etc/init.d`
2. Run `sudo chkconfig --add /etc/init.d/tomcat`
3. Run `sudo service tomcat start`

Configuration you may need to perform first:

* Alter the `chkconfig:` line (line 12) to reflect your desired runlevels and start/stop priority:

                runlevels   stop priority  # lower numbers stop sooner
                      |       |
        # chkconfig: 2345 80 20
                          |
                       start priority   # higher numbers start sooner

    You must do this before you run step 2. If you have already completed step to, just run the command again.

    See `man chkconfig` for more info.

* Edit `JAVA_HOME`, `CATALINA_HOME` and any other relevant environment variables for the Tomcat service (lines 35--39). Available environment variables are listed at the top of `catalina.sh`, with helpful descriptions.

* **If you know what you're doing:** Edit the `SERVICENAME`, `LOCKFILE` and `CATALINA_PID` variables (lines 26--28) as necessary for other Linux systems.

## Usage ##

Once installed, the following commands are available:

    $ sudo service tomcat start  # startup
    Starting Tomcat:
    Using CATALINA_BASE:   /usr/local/apache-tomcat-6.0.33
    Using CATALINA_HOME:   /usr/local/apache-tomcat-6.0.33
    Using CATALINA_TMPDIR: /usr/local/apache-tomcat-6.0.33/temp
    Using JRE_HOME:        /usr
    Using CLASSPATH:       /usr/local/apache-tomcat-6.0.33/bin/bootstrap.jar
    Using CATALINA_PID:    /var/run/tomcat6.pid
    
    $ service tomcat status
    tomcat6 (pid  1234) is running...

    $ sudo service tomcat stop   # shutdown
    Shutting down Tomcat:
    Using CATALINA_BASE:   /usr/local/apache-tomcat-6.0.33
    Using CATALINA_HOME:   /usr/local/apache-tomcat-6.0.33
    Using CATALINA_TMPDIR: /usr/local/apache-tomcat-6.0.33/temp
    Using JRE_HOME:        /usr
    Using CLASSPATH:       /usr/local/apache-tomcat-6.0.33/bin/bootstrap.jar
    Using CATALINA_PID:    /var/run/tomcat6.pid
    
    $ service tomcat status
    tomcat6 is stopped
    
    $ sudo service tomcat restart   # shortcut for stopping
                                    # then starting
