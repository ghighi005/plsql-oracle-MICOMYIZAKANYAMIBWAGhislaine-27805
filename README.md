# plsql-oracle-MICOMYIZAKANYAMIBWAGhislaine-27805

I used oraclce 23 free ai as oracle 21 c was not working and i was able to do the following
## TASK 1 

When I began Task 1 (creating the PDB using the first-two-letters + suffix naming scheme and then creating the student user inside it), I first connected as SYS (or an account with CREATE PLUGGABLE DATABASE privilege) to the root container (CDB$ROOT). I issued a CREATE PLUGGABLE DATABASE XX_pdb_StudentID ADMIN USER FirstName_plsqlauca_StudentID IDENTIFIED BY simplepwd … FILE_NAME_CONVERT = (…) statement. At first, I got errors about file paths or “file already exists” because I hadn’t properly set or converted the seed datafile path to the target PDB path. I had to use the FILE_NAME_CONVERT clause (mapping seed paths to new PDB paths) so that the new PDB’s datafiles go into a clean directory. I also needed to ensure that db_create_file_dest (or an appropriate parameter) was set so Oracle knew where to place the files. Once the PDB was created in “MOUNTED” mode, I then ran ALTER PLUGGABLE DATABASE … OPEN; and ALTER PLUGGABLE DATABASE … SAVE STATE; to leave it open by default on startup. Then inside that new PDB container, I switched container context (via ALTER SESSION SET CONTAINER = XX_pdb) and created the user with the username format and granted appropriate privileges. The final verification was SHOW PDBS (to see the PDB list) and CONNECT FirstName_plsqlauca_ID/simplepwd to confirm I could log into the PDB.

However, during that flow I also encountered a subtle issue: listener service name collisions. Because Oracle automatically registers a service name for each PDB (often matching the PDB name), I saw an error complaining of a name conflict when I used a PDB name that was identical to or clashed with an existing service. In one case a service starting with the PDB name plus a dot caused strange behavior (could not stop or start the service) this is a known bug in some Oracle versions related to service naming. To work around it, I ensured the PDB name avoided dots and did not match existing service names. I also checked in CDB_SERVICES and V$SERVICES to confirm there was no service name conflict. Once I chose a safe PDB name and used appropriate SERVICE_NAME_CONVERT clause if needed, the PDB opened cleanly.

## Task 2

For Task 2 (create another PDB just to delete it), I repeated a similar create flow, naming it like XX_to_delete_pdb_StudentID. The creation succeeded using the same clauses and opening steps. Then to delete it, I first closed the PDB (e.g. ALTER PLUGGABLE DATABASE XX_to_delete_pdb_StudentID CLOSE IMMEDIATE;) to make sure it was not open. In some Oracle versions, if you drop a PDB without closing it first (or without IMMEDIATE), the drop can hang forever (especially if there are active sessions). I encountered exactly that hang in an earlier run when I omitted IMMEDIATE. After correcting that, I used:
ALTER SESSION SET CONTAINER = CDB$ROOT;
DROP PLUGGABLE DATABASE XX_to_delete_pdb_StudentID INCLUDING DATAFILES;

The INCLUDING DATAFILES clause ensured that the PDB’s data files were cleaned up (so no orphan files remained). That drop succeeded. Afterward, I ran SHOW PDBS to confirm it was gone, and I also checked the filesystem to ensure its directory/data files had been removed. During the drop, I also monitored for errors like “file not found” or “unable to drop because still open,” which I avoided by closing first. In sum, both tasks ultimately worked, after I addressed file path conversion, service name conflicts, and the need to close before dropping with IMMEDIATE.

## Task 3
WE shall take time to configure it as it's still challenging

