CREATION OF PDB, ALTER AND DELETION
------------------------------------
this README provides an overview of the creation of a pluggable database, creation of a user and deletion
System user
-----------
C:\Users\user>sqlplus sys as sysdba

SQL*Plus: Release 21.0.0.0.0 - Production on Fri Oct 4 12:05:52 2024 Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.

Enter password:

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production Version 21.3.0.0.0

SQL> show user
USER is "SYS"
SQL> select instance_name from v$instance; ##INSTANCE_NAME

INSTANCE_NAME
----------------
xe

INSTANCE_NAME
----------------

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------

      2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO
          4 PLSQL_CLASS2024DB              READ WRITE NO
         5 PLSQL_CLASS20024DB             READ WRITE NO
         6 PLSQL_CLASS22024DB             READ WRITE NO

 Create pluggabe database
SQL>  create pluggable database plsql_class20244db 2  admin user pdbadmin identified by admin 3 file_name_convert=
('C:\APP\USER\PRODUCT\21C\ORADATA\XE\pdbseed\','C:\APP\USER\PRODUCT\21C\ORADATA\XE\plsql_class20244db\');

Pluggable database created.

 SQL>  select con_id,tablespace_name,file_name 2 from cdb_data_files 3 where con_id=3;
    CON_ID TABLESPACE_NAME
---------- ------------------------------
FILE_NAME
--------------------------------------------------------------------------------
         3 SYSTEM
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\SYSTEM01.DBF
       
       3 SYSAUX
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\SYSAUX01.DBF

         3 UNDOTBS1
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\UNDOTBS01.DBF

    CON_ID TABLESPACE_NAME
---------- ------------------------------

FILE_NAME
--------------------------------------------------------------------------------
         3 USERS
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\USERS01.DBF

SQL> alter session set container=plsql_class20244db;

Session altered.

Opening pdb
-------------
SQL> alter pluggable database plsql_class20244db open;

Pluggable database altered.

SQL> alter pluggable database plsql_class20244db save state;

Pluggable database altered.

creation of a user au_plsqlauca
------------------------------------
SQL> create user au_plsqlauca;

User created.

SQL> SQL> grant all privileges to au_plsqlauca;
SP2-0734: unknown command beginning "SQL> grant..." - rest of line ignored.
SQL> grant all privileges to au_plsqlauca;

Grant succeeded.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         7 PLSQL_CLASS20244DB             READ WRITE NO
SQL> alter session set container=cdb$root;

Session altered.

Creating user au_to_delete_pdb
---------------------------------
SQL>  create pluggable database au_to_delete_pdb
  2  from plsql_class20244db
  3  file_name_convert=('C:\app\user\product\21c\oradata\XE\plsql_class20244db\','C:\app\user\product\21c\oradata\XE\au_to_delete_pdb\');

Pluggable database created.

SQL> show pdbs;

  CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO
         4 PLSQL_CLASS2024DB              READ WRITE NO
         5 PLSQL_CLASS20024DB             READ WRITE NO
         6 PLSQL_CLASS22024DB             READ WRITE NO
         7 PLSQL_CLASS20244DB             READ WRITE NO
         8 AU_TO_DELETE_PDB               MOUNTED

SQL>  alter session set container=au_to_delete_pdb;

Session altered.

Opening au_to_delete_pdb
--------------------------
ORA-65019: pluggable database AU_TO_DELETE_PDB already open

SQL>  alter pluggable database au_to_delete_pdb save state;

Pluggable database altered.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         8 AU_TO_DELETE_PDB               READ WRITE NO
SQL>  alter pluggable database au_to_delete_pdb close immediate;

Pluggable database altered.

SQL> alter session set container=cdb$root;

Session altered.

SQL> alter pluggable database au_to_delete_pdb unplug into 'C:\app\user\product\21c\admin\XE\dpdump\au_to_delete_pdb.xml';

Pluggable database altered.

deleting au_to_delete_pdb
----------------------------

SQL>  drop pluggable database au_to_delete_pdb;

Pluggable database dropped.

SQL>

conclusion
------------
This Pluggable database is free of use and modify. 









