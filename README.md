
Author: Adam Sheeler
This is a databse system that supports the operations of `ArborDB`, an IoT system monitoring forests, by following the development cycle of a database application.

# How to start:
1. `Download the JDBC Driver`: Begin by downloading the JDBC driver compatible with your database. Place all the JDBC driver files into a single folder for easy access.
2. `Database Preparation`: Execute all necessary SQL files to set up your database schema before connecting through JDBC. Utilize the following connection URL in your Java application: `URL = "jdbc:postgresql://localhost:5432/postgres?currentSchema=schema"`

# Workflow:
Once you've completed the above steps, the JDBC application will guide you through each operation. Upon the completion of an operation, you will be automatically returned to the main menu. The result of the operation will be displayed just before the menu options reappear.

# Exception Handling:

1. `SQLException`: This exception may occur if there are issues with the database connection. Please verify your connection details and ensure the database server is running.

2. `NumberFormatException`: This exception indicates that the input provided does not match the expected data type. Ensure that you enter the correct type of value when prompted.

# System Behavior:

Any other exceptions will cause the system to stop executing. While all outcomes up to the point of failure will be preserved, you will need to restart the Java application and reconnect to the JDBC to continue.

# Functions: 
1. `connect`
This operation should connect to your PostgreSQL database by prompting for a username and password to establish the connection. Recall that the default user is postgres.
2. `addForest`
This operation prompts for a name, area, acid level, MBR XMin, MBR XMax, MBR YMin, and MBR YMax, then adds the new forest to the system by inserting a new entry into the forest relation. forest nos should be auto-generated.
3. `addTreeSpecies`
This operation prompts for a genus, epithet, ideal temperature, largest height, and raunki- aer life form, then adds the new tree species to the system by inserting a new entry into the tree species relation.
4. `addSpeciesToForest`
This operation prompts for a forest no, genus, and epithet, then adds an entry for the tree species being found in the forest.
5. `newWorker`
This operation prompts for a SSN, First name, Last name, Middle initial, rank, and a state abbreviation where the worker will initially be employed, then adds a new worker to the system by inserting a new entry into the worker relation. In addition, the worker’s employment should be inserted.
6. `employWorkerToState`
This operation prompts for a state abbreviation and worker SSN, then adds an entry for the worker being employed by the state.
7. `placeSensor`
This operation prompts for the energy of the sensor, location of deployment (X, Y), and the maintainer id, then adds a new sensor to the system by inserting a new entry into the sensor relation. Note that for the last charged and last read timestamps, you should use the current synthetic time from the CLOCK relation.
8. `generateReport`
This operation should first display a formatted, numbered list (ordered in ascending order by the sensor id) of all the sensors in the ArborDB system. Then it allows the user to either generate a report by entering the appropriate sensor id or to exit browsing and return to the main menu by entering -1 as a sensor id. If the user selects a valid sensor id (i.e., one that exists within ArborDB and is not -1), the user should be prompted for the report time and temperature recorded by the sensor that is generating the report, one at a time. The operation adds a new report to the system by inserting a new entry into the report relation.
In the event that there are no sensors in ArborDB, a message “No Sensors are currently deployed” should be displayed to the user.
2
9. `removeSpeciesFromForest`
This operation prompts for a genus, epithet, and a forest no, then removes the tree species from the forest.
10. `deleteWorker`
This operation prompts for a SSN, then removes the worker by deleting the worker from the worker relation. In addition, the worker should no longer be employed by any states and all sensors maintained by the worker should be removed.
11. `moveSensor`
This operation prompts the user for the sensor id they would like to move and the new X and Y location of the sensor, one at a time. If the user provides a sensor id of -1, the operation should return the user to the main menu of the application. The operation moves the deployed sensor by updating the sensor in the sensor relation.
In the event that there are no sensors in ArborDB, a message “No Sensors to Redeploy” should be displayed to the user.
12. `removeWorkerFromState`
This operation prompts for a SSN and state abbreviation, then removes the worker’s employ- ment for the given state. In addition, if the worker is maintaining a sensor that is located within the state where they were removed from, those sensor(s) should be reassigned to an- other worker within that same state who has the lowest SSN. However, if no other workers are employed to that state, the sensor should be removed.
13. `removeSensor`
This task should first ask the user if they would like to remove all or selected sensors from ArborDB.
If the user’s response is all, then the user is prompted for a confirmation. If the confirmation is Yes, then the operation proceeds with deleting all the sensors and any reports generated by the sensors. If the confirmation is No, the operation produces a message “No Sensors were Removed” and the user should be returned to the main menu of the application.
If the user’s response is selected, it displays one sensor at a time and asks the user to enter the displayed sensor id as a confirmation that they would like to delete the sensor. If the user enters a displayed sensor id, the selected sensor is removed along with all of its reports. If -1 is entered, the user is returned to the main menu of the application without removing the sensor. If an invalid sensor id (other than -1) is entered, the operation should ask the user to enter one of the displayed sensor ids or -1.
14. `listSensors`
This operation prompts for a forest id, then displays all sensors within the specified forest in a nicely formatted way.
15. `listMaintainedSensors`
This operation prompts for a worker’s SSN, then displays all sensors that the worker is currently maintaining in a nicely formatted way.
16. `locateTreeSpecies`
This operation prompts for two patterns α and β, then finds all forests that contain any tree species whose genus matches the pattern α or epithet matches the pattern β. These forests should be displayed in a nicely formatted way.
3

17. `rankForestSensors`
This task should display a ranked list of all forests based on the number of sensors found within them.
In the event that there are no forests in the system, a message “No Forests to Rank” should be displayed to the user.
18. `habitableEnvironment`
This operation prompts for a genus, epithet, and integer value k, then finds all forests that would be habitable environments for the tree species. Note: A forest is considered a habitable environment for the tree species if the ideal temperature of the tree species, denoted δ, is within five degrees of a forest’s average reported temperature for the last k years. 1 year is defined as 365 days counting back starting from the current synthetic date in the CLOCK relation. That is, a tree species is habitable if
δ − 5 ≤ average reported forest temperature (last k years) ≤ δ + 5.
Any forests that are habitable environments should be displayed in a nicely formatted way.
In the event that no forests are habitable for the tree species, a message “No habitable envi- ronments were found” should be displayed to the user.
19. `topSensors`
This operation prompts for integers k and x, then displays the top k sensors with respect to the number of reports generated by the sensor in the past x months. x and k are input parameters to this function. 1 month is defined as 30 days counting back starting from the current synthetic date of the Clock table. The top k sensors should be displayed in a nicely formatted way.
20. `threeDegrees`
This operation prompts for two forest nos, f1 and f2, then finds a path, if one exists, between f1 and f2 with at most 3 hops between them. A hop is defined as two forests having the same tree species (genus, epithet) found within them. This operation should display a string representing the path that connects f1 to f2 with all intermediate hops. E.g., ‘f1 → f3 → f4 → f2’ where f3 and f4 are forests that connect f1 to f2.
In the event that no path is found between f1 and f2 in three hops, a message “No path was found” should be displayed to the user.
21. `exit`
This operation should cleanly shut down and exit the program.
