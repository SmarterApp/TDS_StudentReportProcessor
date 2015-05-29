# Welcome to the Student Report Processor Project!

The Student Report Processor is a JAR which generates student XML reports and submit to TIS (Test Integration System). It should be set up to run as a background process. It processes records in its queue one by one. The XML reports associated with the test opportunities for those records are sent to TIS and then deleted.

## License ##
This project is licensed under the [AIR Open Source License v1.0](http://www.smarterapp.org/documents/American_Institutes_for_Research_Open_Source_Software_License.pdf).

## Getting Involved ##
We would be happy to receive feedback on its capabilities, problems, or future enhancements:

* For general questions or discussions, please use the [Forum](http://forum.opentestsystem.org/viewforum.php?f=9).
* Use the **Issues** link to file bugs or enhancement requests.
* Feel free to **Fork** this project and develop your changes!

## Module Overview

### Java

Essentially, this is a Java standalone application.

For each record in qareportqueue table that has a datesent value of null, send the XML report associated with the test opportunity for that record and then delete that record.

## Setup
In general, build the code and deploy the JAR file. Run the jar file as background process. Create a start up script to run the application on machine reboot and another script to monitor the application and starts it up if the program is not running.

```
/usr/bin/java \
    -DreportingDLL.tisUrl="<TIS URL>" \
    -DreportingDLL.tisStatusCallbackUrl="<TIS Status Callback URL>" \
    -Doauth.tis.client.id="<Oauth Client ID>" \
    -Doauth.tis.client.secret="<Oauth Client Secret>" \
    -Doauth.tis.username="<Oauth Username>" \
    -Doauth.tis.password="<Oauth Password>" \
    -Doauth.access.url="<Oauth Access URL>" \
    -Dlogfilename="/path/to/log/file/maintenance" \
    -Djdbc.driver="com.mysql.jdbc.Driver" \
    -Djdbc.url="jdbc:mysql://name.of.mysql.server:3306/session" \
    -Djdbc.userName="<mysql_username>" \
    -Djdbc.password="<mysql_password>" \
    -DDBDialect="MYSQL" \
    -classpath ".:/jar/file/path/to/tdsMaintenance.jar:/mysql/connector/path/mysql-connector-java-5.1.22-bin.jar" org.opentestsystem.delivery.studentreportprocessor.StudentReportProcessor

```
## Database Configuration
For populating academicYear inside the XML Report, configure schoolyear column acoording to current year inside `itembank`.`tbltestadmin` table.
This column needs to be updated every year.

```
Example:
UPDATE `itembank`.`tbltestadmin` SET `schoolyear`='2015' WHERE `_key`='SBAC';
UPDATE `itembank`.`tbltestadmin` SET `schoolyear`='2015' WHERE `_key`='SBAC_PT';
```


### Build Order

If building all components from scratch the following build order is needed:

* shared-db
* shared-tr-api
* tdsdlldev

## Dependencies
StudentReportProcessor has a number of direct dependencies that are necessary for it to function.  These dependencies are already built into the Maven POM files.

### Compile Time Dependencies
* shared-db
* shared-tr-api
* tds-dll-api
* tds-dll-mysql
* c3p0
* mysql-connector-java
* log4j
* slf4j-log4j12
* servlet-api