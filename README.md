# Greener

'Greener' is a Java-based Desktop application designed to give the users insight into ways of reducing their CO2 emissions and encouraging them to do so by providing proper carbon calculations, the possibility of sharing their struggle for the environment with others and using gamification techniques to make the use of the application enjoyable. 

> This is project is done by 7 TU Delft Computer Science & Engineering students as part of OOP Project.

# Project Setup
For the setup of this project you will need to install the following:
- [GlassFish 5.0 - Full Platform](https://javaee.github.io/glassfish/download) (just download it and remember the file where you put it)
- [MySQL](https://dev.mysql.com/downloads/windows/installer/8.0.html) (MySQL Workbench is preffered for Project Management,)
- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

## Database Creation

- You  need to create a User 'sammy' with Password 'temporary' for your database. 

- Create a new user by running the following scripts on the database or create the user through Mysql Workbench (easier). You can see how that's done [here](https://dev.mysql.com/doc/refman/8.0/en/creating-accounts.html). 


```
CREATE USER 'sammy'@'localhost' IDENTIFIED BY 'temporary';

GRANT ALL PRIVILEGES ON * . * TO 'sammy'@'localhost';

FLUSH PRIVILEGES;
```


- Don't forget to create the user 'sammy', as mentioned above
- Create a new database named 'greener'.
- In this database, create a table named 'person' with the following DDL script:

```
CREATE TABLE `person` (
  `ID` double NOT NULL,
  `Email` varchar(40) NOT NULL,
  `Password` varchar(150) NOT NULL,
  `Name` varchar(40) NOT NULL,
  `Friend_code` varchar(30) DEFAULT NULL,
  `Level` int(11) NOT NULL DEFAULT '1',
  `CO_2_saved` double NOT NULL,
  `Vegan_meal` double NOT NULL,
  `Bike` double NOT NULL,
  `Solar_panels` double NOT NULL,
  `Local_produce` double NOT NULL,
  `Lowering_home_temperature` double NOT NULL,
  `Public_transport` double NOT NULL,
  `Achievements` varchar(24) NOT NULL DEFAULT '000000000000000000000000',
  PRIMARY KEY (`ID`),
  UNIQUE KEY `Email_UNIQUE` (`Email`),
  UNIQUE KEY `ID_UNIQUE` (`ID`),
  UNIQUE KEY `Friend_code_UNIQUE` (`Friend_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```

- In this database, create a table 'friends' with the following DDL script:

```
CREATE TABLE `friends` (
  `ID` int(11) NOT NULL,
  `User_email` varchar(40) NOT NULL,
  `Friend_email` varchar(45) NOT NULL,
  PRIMARY KEY (`ID`,`Friend_email`),
  KEY `User_email_idx` (`User_email`),
  CONSTRAINT `User_email` FOREIGN KEY (`User_email`) REFERENCES `person` (`Email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```

## Server Setup

- **Don't forget** to go to File -> Project Structure.. and select jdk 1.8
- Furthermore you need to have a working version of [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and add it as a project sdk by going to File -> Project Structure -> Project SDK (select New.).
- After installing, you should download and import the project into your Intellij IDE.
- You should add this as a maven project.
- Once you're set up, run 'maven clean' and 'maven install'.
- You will have to set up GlassFish as follows:
- On the left of the 'Run' button of Intellij, there is a dropdown to select the run configuration, click on it then head to Edit Configurations... -> Add New Configuration ( the '+' button in the upper left corner of the window) -> GlassFish Server -> Local, if it is the first time you use Glassfish you will have to click 'Configure.' at the top of this window and navigate to the location where you downloaded the Glassfish Server files.
- Set JRE to 1.8 in this window
- Set Server Domain to domain1 in this window
- Go to Deployment tab of your configuration window, Add Artifact..('+' button on the right)->Artifact and set to Server:war
- You can start the server by clicking run now.
- **Make sure** that your database process is running, and you have created the user 'sammy' and the tables accordingly.
- **Once the server is running**, you can start the application by running the Main class from the Client module (modules/Client/src/main/java/gui/Main.java).

## Getting the test coverage
- Run "mvn site" then check targer/site/cobertura in the Server module for the Server coverage and in the Client module for the Client coverage.

## Getting the checkstyle report
- Run "mvn checkstyle:checkstyle" and check the site file of each module for the checkstyle-result.xml or just use the Intellij Checkstyle plug-in.
