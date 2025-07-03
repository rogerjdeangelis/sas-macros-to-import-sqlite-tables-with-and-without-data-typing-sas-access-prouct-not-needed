# sas-macros-to-import-sqlite-tables-with-and-without-data-typing-sas-access-prouct-not-needed
sas macros to import sqlite tables with and without data typing sas access prouct not needed
    %let pgm=utl-sas-macros-to-import-sqlite-tables-with-and-without-data-typing-sas-access-prouct-not-needed;

    %stop_submission;

    sas macros to import sqlite tables with and without data typing sas access prouct not needed

           CONTENTS

                1 macro sqlitex deferred typing
                2 macro sqlitet sqlite mapped typing
                3 macros for autocall library

    github
    https://tinyurl.com/yc8h92zw
    https://github.com/rogerjdeangelis/sas-macros-to-import-sqlite-tables-with-and-without-data-typing-sas-access-prouct-not-needed

    USAGES

      SQLITE TABLE STUDENTS TO WORK.WANT INFERRED TYPING

       %sqlitex(
          dbpath = d:/sqlite/have.db
         ,inp    = students
         ,out    = want
       )

      SQLITE TABLE STUDENTS TO WORK.WANT WITH MAPPED SQLITE TYPES

       %sqlitet(
          dbpath = d:/sqlite/have.db
         ,inp    = students
         ,out    = want
       )

    SOAPBOX_ON

    For some non-binary cases this may be a better way to export data from r, python, matlab/octave to sas, unlike a csv,
    we can do some data typing?

    Because the parmcards macro sandwich, utl_psbeginx/endx, cannot exist inside a macro.
    We need to use the utl_submit_ps64x macro, which can placed inside a macro or datastep.
    Cards and parmcards are not supported inside a macro.

    Moved these inside the macros after the problem block comment

     /*---- for development & testing -----*/
     proc datasets lib=work
       nolist nodetails;
      delete want _meta_ mapem;
     run;quit;

     %utlfkil(
      %sysfunc(pathname(work))/want.csv/want.csv)
     %utlfkil(
      %sysfunc(pathname(work))/want.csv/mets.csv)

    SOAPBOX OFF

    realed repo (see for data typing)
    github
    https://tinyurl.com/42zyttre
    https://github.com/rogerjdeangelis/utl-import-a-sqlite-table-with-data-types-without-sas-access-product

    PROBLEM


    /**************************************************************************************************************************/
    /* INPUT                                |  PROCESS                                        | OUTPUT                        */
    /* =====                                |  =======                                        | ======                        */
    /* CONTENTS SQLITE STUDENTS TABLE       |  1 SQLITEX NO DATA TYPING                       | SAS WANT                      */
    /*                                      |  ========================                       | NAME   SEX AGE HEIGHT WEIGHT  */
    /* d:/sqlite/have.db                    |                                                 |                               */
    /*                                      |  CREATE TABLE WANT FROM                         | Alfred  M   14     69  112.5  */
    /* +    pragma_table_info('students')") |  SQLITE TABLE STUDENTS                          | Alice   F   13   56.5     84  */
    /*                                      |  1  USAGE                                       | Barbara F   13   65.3     98  */
    /*                    not               |     %sqlitex(                                   | Carol   F   14   62.8  102.5  */
    /*   cid   name type null dflt pk       |        dbpath = d:/sqlite/have.db               | Henry   M   14   63.5  102.5  */
    /* 1   0   NAME TEXT    0  NA  0        |       ,inp    = students                        |                               */
    /* 2   1    SEX TEXT    0  NA  0        |       ,out    = want                            | COONTENTS                     */
    /* 3   2    AGE REAL    0  NA  0        |     )                                           |  Variable Type Len  Informat  */
    /* 4   3 HEIGHT REAL    0  NA  0        |  2 Shell out to powershell and                  |                               */
    /* 5   4 WEIGHT REAL    0  NA  0        |    use sqlite3 to create csv                    |  NAME     Char   7  $7.       */
    /*                                      |  3  Use sas proc import to                      |  SEX      Char   1  $1.       */
    /*                                      |     create sas dataset from csv                 |  AGE      Num    8  BEST32.   */
    /* SQLITE TABLE STUDENTS                |                                                 |  HEIGHT   Num    8  BEST32.   */
    /*                                      |  /*---- for development & testing -----*/       |  WEIGHT   Num    8  BEST32.   */
    /* +      students")                    |                                                 |                               */
    /*      NAME SEX AGE HEIGHT WEIGHT      |  proc datasets lib=work                         |                               */
    /* 1  Alfred   M  14   69.0  112.5      |    nolist nodetails;                            |                               */
    /* 2   Alice   F  13   56.5   84.0      |   delete want;                                  |                               */
    /* 3 Barbara   F  13   65.3   98.0      |  run;quit;                                      |                               */
    /* 4   Carol   F  14   62.8  102.5      |                                                 |                               */
    /* 5   Henry   M  14   63.5  102.5      |  %utlfkil(                                      |                               */
    /*                                      |  %sysfunc(pathname(work))/want.csv/want.csv);   |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %symdel dbpath inp out /nowarn;                |                               */
    /* options validvarname=upcase;         |                                                 |                               */
    /* libname sd1 "d:/sd1";                |  /*------------------------------------*/       |                               */
    /* data sd1.students;                   |                                                 |                               */
    /*    informat name $8. sex $2. ;       |                                                 |                               */
    /*    input name sex age height weight; |  %macro sqlitex(                                |                               */
    /* cards4;                              |     dbpath   = d:/sqlite/have.db                |                               */
    /* Alfred  M 14 69.0 112.5              |    ,inp      = students                         |                               */
    /* Alice   F 13 56.5 84.0               |    ,out      = want                             |                               */
    /* Barbara F 13 65.3 98.0               |  ) / des="import sqlite table sas dataset";     |                               */
    /* Carol   F 14 62.8 102.5              |                                                 |                               */
    /* Henry   M 14 63.5 102.5              |  %utlfkil(%sysfunc(pathname(work))/want.csv);   |                               */
    /* ;;;;                                 |                                                 |                               */
    /* run;quit;                            |  /*---- powershell ----*/                       |                               */
    /*                                      |  %utl_submit_ps64("                             |                               */
    /* * Using R to create sqlite table;    |  sqlite3 -csv -header '&dbpath'                 |                               */
    /*                                      |    'select * from &inp;'                        |                               */
    /* %utlfkil(d:/sqlite/have.db);         |     > '%sysfunc(pathname(work))/want.csv';      |                               */
    /*                                      |  ");                                            |                               */
    /* %utl_rbeginx;                        |                                                 |                               */
    /* parmcards4;                          |  proc import out=want                           |                               */
    /* library(haven)                       |      datafile="d:/csv/want.csv"                 |                               */
    /* library(DBI)                         |      dbms=csv                                   |                               */
    /* library(RSQLite)                     |      replace;                                   |                               */
    /* students<-read_sas(                  |      getnames=YES;                              |                               */
    /*  "d:/sd1/students.sas7bdat")         |      guessingrows=MAX;                          |                               */
    /* con <- dbConnect(                    |  run;quit;                                      |                               */
    /*     RSQLite::SQLite()                |                                                 |                               */
    /*    ,"d:/sqlite/have.db")             |  %mend sqlitex;                                 |                               */
    /* dbWriteTable(                        |                                                 |                               */
    /*     con                              |  %sqlitex;                                      |                               */
    /*   ,"students"                        |                                                 |                               */
    /*   ,students)                         |  proc print data=want;                          |                               */
    /* dbListTables(con)                    |  run;quit;                                      |                               */
    /* dbGetQuery(                          |                                                 |                               */
    /*    con                               |  -----------------------------------------------|-------------------------------*/
    /*  ,"select                            |  2 SQLITET DATA TYPING                          | SAS                           */
    /*      *                               |  =====================                          |                               */
    /*    from                              |  USAGE IS THE SAME AS ABOVE                     |  NAME  SEX AGE HEIGHT WEIGHT  */
    /*      students")                      |                                                 |                               */
    /* dbGetQuery(                          |  /*---- for development & testing -----*/       | Alfred  M   14  69.0   112.5  */
    /*    con                               |  proc datasets lib=work                         | Alice   F   13  56.5    84.0  */
    /*  ,"select                            |    nolist nodetails;                            | Barbara F   13  65.3    98.0  */
    /*      *                               |   delete want _meta_ mapem;                     | Carol   F   14  62.8   102.5  */
    /*   from                               |  run;quit;                                      | Henry   M   14  63.5   102.5  */
    /*    pragma_table_info('students')")   |                                                 |                               */
    /* dbDisconnect(con)                    |  %utlfkil(                                      |                               */
    /* ;;;;                                 |   %sysfunc(pathname(work))/want.csv/want.csv)   | Variable    Type    Len       */
    /* %utl_rendx;                          |  %utlfkil(                                      |                               */
    /*                                      |   %sysfunc(pathname(work))/want.csv/mets.csv)   | NAME        Char      7       */
    /*                                      |                                                 | SEX         Char      1       */
    /*                                      |  %symdel dbpath inp out /nowarn;                | AGE         Num       3 **    */
    /*                                      |                                                 | HEIGHT      Num       8       */
    /*                                      |  /*------------------------------------*/       | WEIGHT      Num       3 **    */
    /*                                      |                                                 |                               */
    /*                                      |  %macro sqlitet(                                | Optimizwed numerics           */
    /*                                      |     dbpath   = d:/sqlite/have.db                |                               */
    /*                                      |    ,inp      = students                         |                               */
    /*                                      |    ,out      = want                             |                               */
    /*                                      |  ) / des="import sqlite table to sas dataset";  |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %utlfkil(%sysfunc(pathname(work))/want.csv);   |                               */
    /*                                      |                                                 |                               */
    /*                                      |  /*---- powershell ----*/                       |                               */
    /*                                      |  %utl_submit_ps64("                             |                               */
    /*                                      |  sqlite3 -csv -header '&dbpath'                 |                               */
    /*                                      |    'select * from &inp;'                        |                               */
    /*                                      |     > '%sysfunc(pathname(work))/want.csv';      |                               */
    /*                                      |  sqlite3 -header -csv 'd:/sqlite/have.db'       |                               */
    /*                                      |    'PRAGMA table_info (&inp);'                  |                               */
    /*                                      |     > '%sysfunc(pathname(work))/meta.csv';      |                               */
    /*                                      |  ");                                            |                               */
    /*                                      |                                                 |                               */
    /*                                      |  proc import out=_meta_                         |                               */
    /*                                      |   datafile="%sysfunc(pathname(work))/meta.csv"  |                               */
    /*                                      |   dbms=csv                                      |                               */
    /*                                      |   replace;                                      |                               */
    /*                                      |   getnames=YES;                                 |                               */
    /*                                      |   guessingrows=MAX;                             |                               */
    /*                                      |  run;quit;                                      |                               */
    /*                                      |                                                 |                               */
    /*                                      |  proc format;                                   |                               */
    /*                                      |                                                 |                               */
    /*                                      |   value $maptyp                                 |                               */
    /*                                      |    'REAL'    = '32.'                            |                               */
    /*                                      |    'INTEGER' = '32.'                            |                               */
    /*                                      |    'TEXT'    = '$255.';                         |                               */
    /*                                      |                                                 |                               */
    /*                                      |  run;quit;                                      |                               */
    /*                                      |                                                 |                               */
    /*                                      |  data _mapem_;                                  |                               */
    /*                                      |    set _meta_(keep=name type);                  |                               */
    /*                                      |    typ=put(type,$maptyp.);                      |                               */
    /*                                      |    drop type;                                   |                               */
    /*                                      |  run;quit;                                      |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %array(_typ,data=_mapem_,var=typ);             |                               */
    /*                                      |  %array(_nam,data=_mapem_,var=name);            |                               */
    /*                                      |                                                 |                               */
    /*                                      |  data &out;                                     |                               */
    /*                                      |    informat                                     |                               */
    /*                                      |      %do_over(_nam _typ,phrase=?_nam ?_typ);;   |                               */
    /*                                      |    infile "d:/csv/data.csv" delimiter=',';      |                               */
    /*                                      |    input                                        |                               */
    /*                                      |      %do_over(_nam,phrase=?);;                  |                               */
    /*                                      |  run;quit;                                      |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %arraydelete(_typ)                             |                               */
    /*                                      |  %arraydelete(_nam)                             |                               */
    /*                                      |                                                 |                               */
    /*                                      |  /*---- optimize variable lengths ----*/        |                               */
    /*                                      |  %utl_optlenpos(&out,&out);                     |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %mend sqlitett;                                |                               */
    /*                                      |                                                 |                               */
    /*                                      |  %sqlitet;                                      |                               */
    /*                                      |                                                 |                               */
    /*                                      |  proc print data=want;                          |                               */
    /*                                      |  run;quit;                                      |                               */
    /**************************************************************************************************************************/

     /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.students;
       informat name $8. sex $2. ;
       input name sex age height weight;
    cards4;
    Alfred  M 14 69.0 112.5
    Alice   F 13 56.5 84.0
    Barbara F 13 65.3 98.0
    Carol   F 14 62.8 102.5
    Henry   M 14 63.5 102.5
    ;;;;
    run;quit;

    * Using R to create sqlite table;

    %utlfkil(d:/sqlite/have.db);

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(DBI)
    library(RSQLite)
    students<-read_sas(
     "d:/sd1/students.sas7bdat")
    con <- dbConnect(
        RSQLite::SQLite()
       ,"d:/sqlite/have.db")
    dbWriteTable(
        con
      ,"students"
      ,students)
    dbListTables(con)
    dbGetQuery(
       con
     ,"select
         *
       from
         students")
    dbGetQuery(
       con
     ,"select
         *
      from
       pragma_table_info('students')")
    dbDisconnect(con)
    ;;;;
    %utl_rendx;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  CONTENTS STUDENTS TABLE                                                                                               */
    /*                                                                                                                        */
    /*  d:/sqlite/have.db                                                                                                     */
    /*                                                                                                                        */
    /*  +    pragma_table_info('students')")                                                                                  */
    /*                                                                                                                        */
    /*                     not                                                                                                */
    /*    cid   name type null dflt pk                                                                                        */
    /*  1   0   NAME TEXT    0  NA  0                                                                                         */
    /*  2   1    SEX TEXT    0  NA  0                                                                                         */
    /*  3   2    AGE REAL    0  NA  0                                                                                         */
    /*  4   3 HEIGHT REAL    0  NA  0                                                                                         */
    /*  5   4 WEIGHT REAL    0  NA  0                                                                                         */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  SQLITE TABLE STUDENTS                                                                                                 */
    /*                                                                                                                        */
    /*  +      students")                                                                                                     */
    /*       NAME SEX AGE HEIGHT WEIGHT                                                                                       */
    /*  1  Alfred   M  14   69.0  112.5                                                                                       */
    /*  2   Alice   F  13   56.5   84.0                                                                                       */
    /*  3 Barbara   F  13   65.3   98.0                                                                                       */
    /*  4   Carol   F  14   62.8  102.5                                                                                       */
    /*  5   Henry   M  14   63.5  102.5                                                                                       */
    /**************************************************************************************************************************/

    /*             _ _ _              _        __                        _  _
    / |  ___  __ _| (_) |_ _____  __ (_)_ __  / _| ___ _ __ _ __ ___  __| || |_ _   _ _ __   ___  ___
    | | / __|/ _` | | | __/ _ \ \/ / | | `_ \| |_ / _ \ `__| `__/ _ \/ _` || __| | | | `_ \ / _ \/ __|
    | | \__ \ (_| | | | ||  __/>  <  | | | | |  _|  __/ |  | | |  __/ (_| || |_| |_| | |_) |  __/\__ \
    |_| |___/\__, |_|_|\__\___/_/\_\ |_|_| |_|_|  \___|_|  |_|  \___|\__,_| \__|\__, | .__/ \___||___/
                |_|                                                             |___/|_|
    */

    /*---- for development & testing -----*/
    %symdel dbpath inp out /nowarn;
    /*------------------------------------*/

    %macro sqlitex(
       dbpath   = d:/sqlite/have.db
      ,inp      = students
      ,out      = want
    ) / des="import sqlite table sas dataset";

    proc datasets lib=work
      nolist nodetails;
     delete want;
    run;quit;

    %utlfkil(%sysfunc(pathname(work))/want.csv);

    /*---- powershell ----*/
    %utl_submit_ps64("
    sqlite3 -csv -header '&dbpath'
      'select * from &inp;'
       > '%sysfunc(pathname(work))/want.csv';
    ");

    proc import out=want
        datafile="d:/csv/want.csv"
        dbms=csv
        replace;
        getnames=YES;
        guessingrows=MAX;
    run;quit;

    %mend sqlitex;

    %sqlitex;

    proc print data=want;
    run;quit;

    /**************************************************************************************************************************/
    /* OUTPUT                                                                                                                 */
    /* ======                                                                                                                 */
    /* SAS WANT                                                                                                               */
    /* NAME    SEX AGE HEIGHT WEIGHT                                                                                          */
    /*                                                                                                                        */
    /* Alfred   M   14     69  112.5                                                                                          */
    /* Alice    F   13   56.5     84                                                                                          */
    /* Barbara  F   13   65.3     98                                                                                          */
    /* Carol    F   14   62.8  102.5                                                                                          */
    /* Henry    M   14   63.5  102.5                                                                                          */
    /*                                                                                                                        */
    /* COONTENTS                                                                                                              */
    /*  Variable Type Len  Informat                                                                                           */
    /*                                                                                                                        */
    /*  NAME     Char   7  $7.                                                                                                */
    /*  SEX      Char   1  $1.                                                                                                */
    /*  AGE      Num    8  BEST32.                                                                                            */
    /*  HEIGHT   Num    8  BEST32.                                                                                            */
    /*  WEIGHT   Num    8  BEST32.                                                                                            */
    /**************************************************************************************************************************/

    /*___              _ _ _       _                                          _  _
    |___ \   ___  __ _| (_) |_ ___| |_   _ __ ___   __ _ _ __  _ __   ___  __| || |_ _   _ _ __   ___  ___
      __) | / __|/ _` | | | __/ _ \ __| | `_ ` _ \ / _` | `_ \| `_ \ / _ \/ _` || __| | | | `_ \ / _ \/ __|
     / __/  \__ \ (_| | | | ||  __/ |_  | | | | | | (_| | |_) | |_) |  __/ (_| || |_| |_| | |_) |  __/\__ \
    |_____| |___/\__, |_|_|\__\___|\__| |_| |_| |_|\__,_| .__/| .__/ \___|\__,_| \__|\__, | .__/ \___||___/
                    |_|                                 |_|   |_|                    |___/|_|
    */

    /*---- for development & testing -----*/
    %symdel dbpath inp out /nowarn;
    /*------------------------------------*/

    %macro sqlitet(
       dbpath   = d:/sqlite/have.db
      ,inp      = students
      ,out      = want
      ) / des="export sqlite table to sas dataset";

    proc datasets lib=work
      nolist nodetails;
     delete want _meta_ mapem;
    run;quit;

    %utlfkil(
     %sysfunc(pathname(work))/want.csv/want.csv)
    %utlfkil(
     %sysfunc(pathname(work))/want.csv/mets.csv)

    /*---- powershell ----*/
    %utl_submit_ps64("
    sqlite3 -csv -header '&dbpath'
      'select * from &inp;'
       > '%sysfunc(pathname(work))/want.csv';
    sqlite3 -header -csv 'd:/sqlite/have.db'
      'PRAGMA table_info (&inp);'
       > '%sysfunc(pathname(work))/meta.csv';
    ");

    proc import out=_meta_
        datafile="%sysfunc(pathname(work))/meta.csv"
        dbms=csv
        replace;
        getnames=YES;
        guessingrows=MAX;
    run;quit;

    proc format;

     value $maptyp
      'REAL'    = '32.'
      'INTEGER' = '32.'
      'TEXT'    = '$255.';

    run;quit;

    data _mapem_;
      set _meta_(keep=name type);
      typ=put(type,$maptyp.);
      drop type;
    run;quit;

    %array(_typ,data=_mapem_,var=typ);
    %array(_nam,data=_mapem_,var=name);

    data &out;
      informat
        %do_over(_nam _typ,phrase=?_nam ?_typ);;
      infile "d:/csv/data.csv" delimiter=',';
      input
        %do_over(_nam,phrase=?);;
    run;quit;

    %arraydelete(_typ)
    %arraydelete(_nam)

    /*---- optimize variable lengths ----*/
    %utl_optlenpos(&out,&out);

    %mend sqlitet;

    %sqlitet;

    proc print data=want;
    run;quit;


    /**************************************************************************************************************************/
    /*  SAS                                                                                                                   */
    /*                                                                                                                        */
    /*   NAME  SEX AGE HEIGHT WEIGHT                                                                                          */
    /*                                                                                                                        */
    /*  Alfred  M   14  69.0   112.5                                                                                          */
    /*  Alice   F   13  56.5    84.0                                                                                          */
    /*  Barbara F   13  65.3    98.0                                                                                          */
    /*  Carol   F   14  62.8   102.5                                                                                          */
    /*  Henry   M   14  63.5   102.5                                                                                          */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  Variable    Type    Len                                                                                               */
    /*                                                                                                                        */
    /*  NAME        Char      7                                                                                               */
    /*  SEX         Char      1                                                                                               */
    /*  AGE         Num       3 **                                                                                            */
    /*  HEIGHT      Num       8                                                                                               */
    /*  WEIGHT      Num       3 **                                                                                            */
    /*                                                                                                                        */
    /*  Optimizwed numerics                                                                                                   */
    /**************************************************************************************************************************/

    /*____                                        _                     _                  _ _
    |___ /   _ __ ___   __ _  ___ _ __ ___  ___  | |_ ___    __ _ _   _| |_ ___   ___ __ _| | |
      |_ \  | `_ ` _ \ / _` |/ __| `__/ _ \/ __| | __/ _ \  / _` | | | | __/ _ \ / __/ _` | | |
     ___) | | | | | | | (_| | (__| | | (_) \__ \ | || (_) || (_| | |_| | || (_) | (_| (_| | | |
    |____/  |_| |_| |_|\__,_|\___|_|  \___/|___/  \__\___/  \__,_|\__,_|\__\___/ \___\__,_|_|_|


    */

    filename ft15f001 "c:/oto/sqlitex.sas";
    parmcards4;
    %macro sqlitex(
       dbpath   = d:/sqlite/have.db
      ,inp      = students
      ,out      = want
    ) / des="import sqlite table sas dataset";

    proc datasets lib=work
      nolist nodetails;
     delete want;
    run;quit;

    %utlfkil(%sysfunc(pathname(work))/want.csv);

    /*---- powershell ----*/
    %utl_submit_ps64("
    sqlite3 -csv -header '&dbpath'
      'select * from &inp;'
       > '%sysfunc(pathname(work))/want.csv';
    ");

    proc import out=want
        datafile="d:/csv/want.csv"
        dbms=csv
        replace;
        getnames=YES;
        guessingrows=MAX;
    run;quit;

    %mend sqlitex;
    ;;;;
    %mend sqlitex;


    filename ft15f001 "c:/oto/sqlitet.sas";
    parmcards4;
    %macro sqlitet(
       dbpath   = d:/sqlite/have.db
      ,inp      = students
      ,out      = want
    ) / des="import sqlite table to sas dataset";

    %utlfkil(%sysfunc(pathname(work))/want.csv);

    /*---- powershell ----*/
    %utl_submit_ps64("
    sqlite3 -csv -header '&dbpath'
      'select * from &inp;'
       > '%sysfunc(pathname(work))/want.csv';
    sqlite3 -header -csv 'd:/sqlite/have.db'
      'PRAGMA table_info (&inp);'
       > '%sysfunc(pathname(work))/meta.csv';
    ");

    proc import out=_meta_
     datafile="%sysfunc(pathname(work))/meta.csv"
     dbms=csv
     replace;
     getnames=YES;
     guessingrows=MAX;
    run;quit;

    proc format;

     value $maptyp
      'REAL'    = '32.'
      'INTEGER' = '32.'
      'TEXT'    = '$255.';

    run;quit;

    data _mapem_;
      set _meta_(keep=name type);
      typ=put(type,$maptyp.);
      drop type;
    run;quit;

    %array(_typ,data=_mapem_,var=typ);
    %array(_nam,data=_mapem_,var=name);

    data &out;
      informat
        %do_over(_nam _typ,phrase=?_nam ?_typ);;
      infile "d:/csv/data.csv" delimiter=',';
      input
        %do_over(_nam,phrase=?);;
    run;quit;

    %arraydelete(_typ)
    %arraydelete(_nam)

    /*---- optimize variable lengths ----*/
    %utl_optlenpos(&out,&out);

    %mend sqlitett;
    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
