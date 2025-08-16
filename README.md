# utl-useful-macros-to-create-in-memory-sas-array-statements-with-data
    %let pgm=utl-useful-macros-to-create-in-memory-sas-array-statements-with-data;

    %stop_submission;

    Useful macros to create in memory sas array statements with data

    sas communities
    https://tinyurl.com/5n8xwfx2
    https://communities.sas.com/t5/SAS-Programming/Convert-list-to-an-array/m-p/765534#M242488


    CONTENTS
     1 one dimension    creates array x[3,1] (1,2,3)  (rows do first)
     2 two dimension    creates  array x[3,3] ( 11,12,13,21,22,23,31,32,33 );
     3 three dimension  creates  111,112,121,122,211,212,221,222
     4 zero_array       aeros out previus 3d array
     5 utl_getary macro

    github
    https://tinyurl.com/5n8at26m
    https://github.com/rogerjdeangelis/utl-useful-macros-to-create-in-memory-sas-array-statements-with-data

    related repos
    https://tinyurl.com/vfffx6zd
    https://github.com/rogerjdeangelis/utl-generate-in-memory-sas-two-dimensional-numeric-array-statement-from-sas-table

    https://github.com/rogerjdeangelis/utl-datastep-in-memory-matrix-and-submatrix-row-and-column-reductions-summations-fcmp
    https://github.com/rogerjdeangelis/utl-datastep-matrix-column-and-row-reductions
    https://github.com/rogerjdeangelis/utl_datastep_array_row_col_reductions_sumOf

    /**************************************************************************************************************************/
    /* INPUT                   | PROCESS                               | OUTPUT                                               */
    /* =====                   | =======                               | =====                                                */
    /* WORK.X                  | 1 ONE DIMENSION                       | CREATED TEXT                                         */
    /*                         | ===============                       | [3,1] ( 1,2,3 )                                      */
    /*    X                    |                                       |                                                      */
    /*                         | %deletesasmacn;                       | x[1,1]+x[2,1]+x[3,1]                                 */
    /*    1                    | %symdel ary / nowarn;                 |                                                      */
    /*    2                    |                                       | 1+2+3                                                */
    /*    3                    | %put %utl_getary(x);                  | TRACE=6                                              */
    /*                         |                                       |                                                      */
    /* data x ;                | data want;                            |                                                      */
    /*  input x;               |   array x %utl_getary(x);             |                                                      */
    /* cards4;                 |   trace=x[1,1]+x[2,1]+x[3,1];         |                                                      */
    /* 1                       |   put trace=;                         |                                                      */
    /* 2                       | run;quit;                             |                                                      */
    /* 3                       |                                       |                                                      */
    /* ;;;;                    |                                       |                                                      */
    /* run;quit;               |                                       |                                                      */
    /*                         |                                       |                                                      */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /* data x;                 | 2 TWO DIMENSION                       | Created text                                         */
    /* input                   | ===============                       | [3,3] ( 11,12,13,21,22,23,31,32,33 )                 */
    /* x1 x2 x3;               |                                       |                                                      */
    /* cards4;                 | %deletesasmacn;                       | x[1,1]+x[2,2]+x[3,3]                                 */
    /* 11 12 13                | %symdel ary / nowarn;                 |                                                      */
    /* 21 22 23                |                                       | 11+22+33                                             */
    /* 31 32 33                | %put %utl_getary(x);                  | TRACE=66                                             */
    /* ;;;;                    |                                       |                                                      */
    /* run;quit;               | data want;                            |                                                      */
    /*                         |   array x %utl_getary(x);             |                                                      */
    /*                         |   trace=x[1,1]+x[2,2]+x[3,3];         |                                                      */
    /*                         |   put trace=;                         |                                                      */
    /*                         | run;quit;                             |                                                      */
    /*                         |                                       |                                                      */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /* data x ;                | 3 THREE DIMENSION                     | Created text                                         */
    /* /*---                   | =================                     | 111,112,121,122,211,212,221,222                      */
    /* since sas does not      |                                       |                                                      */
    /* have a 3d structure     | %array(_x,data=x,var=x);              | x[1,1,1]+x[2,2,2]                                    */
    /* you need to know the    |                                       |                                                      */
    /* dimensions ahead        | data want;                            | 111+222                                              */
    /* of time, below          |  array x[2,2,2] (                     | TRACE=333                                            */
    /* is a 2x2x2 array        |   %do_over(_x                         |                                                      */
    /* order has to be         |     ,phrase=?                         |                                                      */
    /*  all rows first         |     ,between=comma) );                |                                                      */
    /*  all columns 2nd and    |  trace=x[1,1,1]+x[2,2,2];             |                                                      */
    /*  all 2rd dim next       |  put trace=;                          |                                                      */
    /*  [2,2,2] 8 obs          | run;quit;                             |                                                      */
    /* ---*/                   |                                       |                                                      */
    /*  input x;               | %arraydelete(_x)                      |                                                      */
    /* cards4;                 |                                       |                                                      */
    /* 111                     |                                       |                                                      */
    /* 112                     |----------------------------------------------------------------------------------------------*/
    /* 121                     | 4 ZERO 3D_ARRAY                       |  Zeroed out prevous array                            */
    /* 122                     | ===============                       |  x[1,1,1]+x[2,2,2]                                   */
    /* 211                     |                                       |                                                      */
    /* 212                     | %array(_x,data=x,var=x);              |  0 + 0                                               */
    /* 221                     |                                       |  TRACE=0                                             */
    /* 222                     | data want;                            |                                                      */
    /* ;;;;                    |  array x[2,2,2] (                     |                                                      */
    /* run;quit;               |   %do_over(_x                         |                                                      */
    /*                         |     ,phrase=?                         |                                                      */
    /*                         |     ,between=comma) );                |                                                      */
    /*                         |  %zero_array(x);                      |                                                      */
    /*                         |  trace=x[1,1,1]+x[2,2,2];             |                                                      */
    /*                         |  put trace=;                          |                                                      */
    /*                         | run;quit;                             |                                                      */
    /*                         |                                       |                                                      */
    /*                         | %arraydelete(_x)                      |                                                      */
    /**************************************************************************************************************************/

    /*___          _   _                _
    | ___|   _   _| |_| |     __ _  ___| |_ __ _ _ __ _   _
    |___ \  | | | | __| |    / _` |/ _ \ __/ _` | `__| | | |
     ___) | | |_| | |_| |   | (_| |  __/ || (_| | |  | |_| |
    |____/   \__,_|\__|_|____\__, |\___|\__\__,_|_|   \__, |
                       |_____|___/                    |___/
    */

    filename ft15f001 "c:/oto/utl_getary.sas";
    parmcards4;
    %macro utl_getary(dsn);
    %symdel ary/nowarn;
    %dosubl('
    %symdel ary/nowarn;
    data _null_;
      set &dsn end=eof;
      array vars[*] _numeric_;
      length allvals $32756;
      length array_str $32756;
      length prefix $44;
      retain allvals;
      length temp $32;
      do i = 1 to dim(vars);
        temp = strip(put(vars[i], best32.));
        if missing(allvals) then allvals = temp;
        else allvals = catx(",", allvals, temp);
      end;
      if eof then do;
        r = _n_;
        c = dim(vars);
        prefix = cats("[",r,",",c,"] (");
        suffix = ");";
        array_str = catx(" ",prefix,allvals,suffix);
        put array_str=;
        call symputx("ary",array_str);
      end;
    run;quit;
    ')
    &ary
    %mend utl_getary;
    ;;;;
    run;quit;


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

