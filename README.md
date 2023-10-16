# utl-select-students-that-have-not-transfered-schools-over-the-last-three-years-wps-r-python-sql
Select students that have not transfered schools over the last three years wps r python sql
    %let pgm=utl-select-students-that-have-not-transfered-schools-over-the-last-three-years-wps-r-python-sql;

    Select students that have not transfered schools over the last three years wps r python sql

      SOLUTIONS

          1 wps sql
          2 wps r sql
          3 wps python sql
          4 wps r base

    stackoverflow
    https://tinyurl.com/468ne49a
    https://stackoverflow.com/questions/77304459/remove-groups-in-r-if-any-row-matches-a-condition

    github
    https://tinyurl.com/49p5bexe
    https://github.com/rogerjdeangelis/utl-select-students-that-have-not-transfered-schools-over-the-last-three-years-wps-r-python-sql

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input years  student  school $3.;
    cards4;
    2000 1 CSU
    2001 1 CSU
    2002 1 CSU
    2003 1 CSU
    2000 2 PSU
    2001 2 PSU
    2002 2 UVM
    2003 2 PSU
    ;;;;
    run;quit;

    /********************************************************|******************************************************************/
    /*                            |                          |                                                                 */
    /*                            |                          |                                                                 */
    /*                            |                          |                                                                 */
    /*             INPUT          |      PROCESS             |       OUTPUT                                                    */
    /*                            |                          |                                                                 */
    /*    YEARS  STUDENT  SCHOOL  |                          |  YEARS  STUDENT  SCHOOL                                         */
    /*                            |                          |                                                                 */
    /*    2000      1       CSU   |   Keep these records     |  2000      1       CSU                                          */
    /*    2001      1       CSU   |   student did not        |  2001      1       CSU                                          */
    /*    2002      1       CSU   |   transfer               |  2002      1       CSU                                          */
    /*    2003      1       CSU   |                          |  2003      1       CSU                                          */
    /*                            |                          |                                                                 */
    /*    2000      2       PSU   |   Student transfered     |                                                                 */
    /*    2001      2       PSU   |   so delete this student |                                                                 */
    /*    2002      2       UVM   |                          |                                                                 */
    /*    2003      2       PSU   |                          |                                                                 */
    /*                            |                          |                                                                 */
    /************************************************************************************************************************* */

    /*                                  _
    / | __      ___ __  ___   ___  __ _| |
    | | \ \ /\ / / `_ \/ __| / __|/ _` | |
    | |  \ V  V /| |_) \__ \ \__ \ (_| | |
    |_|   \_/\_/ | .__/|___/ |___/\__, |_|
                 |_|                 |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    libname sd1 "d:/sd1";
    proc sql;
      create
         table sd1.want as
      select
         *
      from
         sd1.have
      group
         by student
      having
         count(distinct school) = 1
    ;quit;

    proc print;
    run;quit;

    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    YEARS    STUDENT    SCHOOL                                                                                      */
    /*                                                                                                                        */
    /*  1      2000       1        CSU                                                                                        */
    /*  2      2001       1        CSU                                                                                        */
    /*  3      2002       1        CSU                                                                                        */
    /*  4      2003       1        CSU                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                          _
    |___ \  __      ___ __  ___   _ __   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | |    \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                     |_|                        |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    want <- sqldf("
      select
         r.*
      from
         (select student from have group by student having count(distinct school) = 1) as l
      inner
         join have as r
      on
         l.student = r.student
      ");
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    ');

    proc print data=sd1.want width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS PROC R                                                                                                        */
    /*                                                                                                                        */
    /*    YEARS STUDENT SCHOOL                                                                                                */
    /*  1  2000       1    CSU                                                                                                */
    /*  2  2001       1    CSU                                                                                                */
    /*  3  2002       1    CSU                                                                                                */
    /*  4  2003       1    CSU                                                                                                */
    /*                                                                                                                        */
    /*  WPS                                                                                                                   */
    /*                                                                                                                        */
    /*  Obs    YEARS    STUDENT    SCHOOL                                                                                     */
    /*                                                                                                                        */
    /*   1      2000       1        CSU                                                                                       */
    /*   2      2001       1        CSU                                                                                       */
    /*   3      2002       1        CSU                                                                                       */
    /*   4      2003       1        CSU                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                    _   _
    |___ /  __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __
      |_ \  \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \
     ___) |  \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | |
    |____/    \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_|
                     |_|         |_|    |___/
    */

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    from os import path;
    import pandas as pd;
    import numpy as np;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    sqlite3conn = next(pdsql.conn.gen).connection.connection;
    sqlite3conn.enable_load_extension(True);
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll');
    mysql = lambda q: sqldf(q, globals());
    want = pdsql('''
      select
         r.*
      from
         (select student from have group by student having count(distinct school) = 1) as l
      inner
         join have as r
      on
         l.student = r.student
    ''');
    print(want);
    endsubmit;
    import data=sd1.want python=want;
    run;quit;
    "));

    proc print data=sd1.want width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS PYTHON                                                                                                         */
    /*                                                                                                                        */
    /*      YEARS  STUDENT SCHOOL                                                                                             */
    /*  0  2000.0      1.0    CSU                                                                                             */
    /*  1  2001.0      1.0    CSU                                                                                             */
    /*  2  2002.0      1.0    CSU                                                                                             */
    /*  3  2003.0      1.0    CSU                                                                                             */
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs    YEARS    STUDENT    SCHOOL                                                                                      */
    /*                                                                                                                        */
    /*  1      2000       1        CSU                                                                                        */
    /*  2      2001       1        CSU                                                                                        */
    /*  3      2002       1        CSU                                                                                        */
    /*  4      2003       1        CSU                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                 _
    | || |   __      ___ __  ___   _ __  | |__   __ _ ___  ___
    | || |_  \ \ /\ / / `_ \/ __| | `__| | `_ \ / _` / __|/ _ \
    |__   _|  \ V  V /| |_) \__ \ | |    | |_) | (_| \__ \  __/
       |_|     \_/\_/ | .__/|___/ |_|    |_.__/ \__,_|___/\___|
                      |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    library(magrittr);
    want <- have |> filter(n_distinct(SCHOOL) == 1, .by = STUDENT);
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    ');

    proc print data=sd1.want width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS R                                                                                                             */
    /*                                                                                                                        */
    /*    YEARS STUDENT SCHOOL                                                                                                */
    /*                                                                                                                        */
    /*  1  2000       1    CSU                                                                                                */
    /*  2  2001       1    CSU                                                                                                */
    /*  3  2002       1    CSU                                                                                                */
    /*  4  2003       1    CSU                                                                                                */
    /*                                                                                                                        */
    /*  WPS                                                                                                                   */
    /*                                                                                                                        */
    /*  Obs    YEARS    STUDENT    SCHOOL                                                                                     */
    /*                                                                                                                        */
    /*   1      2000       1        CSU                                                                                       */
    /*   2      2001       1        CSU                                                                                       */
    /*   3      2002       1        CSU                                                                                       */
    /*   4      2003       1        CSU                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
