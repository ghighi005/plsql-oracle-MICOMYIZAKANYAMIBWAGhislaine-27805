# plsql-oracle-MICOMYIZAKANYAMIBWAGhislaine-27805

I used oraclce 23 free ai as oracle 21 c was not working and i was able to do the following

I was required to create and later delete a Pluggable Database (PDB) following the given format. I executed the CREATE PLUGGABLE DATABASE command with the name MI_TO_DELETE_PDB_27805, specifying an admin user Micomyiza_plsqlauca_27805 with a password, assigning the DBA role, and using the FILE_NAME_CONVERT clause to ensure the datafiles were placed in a proper directory. The query executed successfully in approximately 3.257 seconds, and the confirmation message in the script output stated: “Pluggable database MI_TO_DELETE_PDB_27805 created.” This verified that the PDB had been properly instantiated and registered under the container database. The process was smooth because I ensured that the directory mapping was correctly defined, and no conflicts with existing services occurred.

After verifying the creation, I proceeded to perform the deletion step. Using the DROP PLUGGABLE DATABASE MI_TO_DELETE_PDB_27805 INCLUDING DATAFILES; command, I was able to remove both the PDB and its associated datafiles completely. This operation was completed in about 3.468 seconds, as shown in the script output message: “Pluggable database MI_TO_DELETE_PDB_27805 dropped.” One key step I observed during this process was ensuring the PDB was not open when issuing the drop command, otherwise Oracle would have raised an error. The inclusion of the INCLUDING DATAFILES clause ensured there were no orphaned files left behind in the system. Overall, the task was successfully accomplished  the PDB was created, verified, and deleted without any major issues, demonstrating proper lifecycle management of Oracle Pluggable Databases.

for Task 3
i shall take time to configure it as it's still challenging

