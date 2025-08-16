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
























































































run;quit;













data x;
input x1 x2 x3;
cards4;
11 12 13
21 22 23
31 32 33
;;;;
run;quit;


%deletesasmacn;
%symdel ary / nowarn;

%put %utl_getary(x);

data want;
  array x %utl_getary(x);
  trace=x[1,1]+x[2,2]+x[3,3];
  put trace=;
run;quit;

TRACE=66

data x ;
/*---
since sas does not
have a 3d structure
you need to know the
dimensions ahead
of time, below
is a 2x2x2 array
order has to be
 all rows first
 all columns 2nd and
 all 2rd dim next
 [2,2,2] 8 obs
---*/
 input x;
cards4;
111
112
121
122
211
212
221
222
;;;;
run;quit;

%array(_x,data=x,var=x);

data want;
 array x[2,2,2] ( %do_over(_x,phrase=?,between=comma) );
 trace=x[1,1,1]+x[2,2,2];
 put trace;;
run;quit;



111,112,113,121,122,123,131,132,133,211,212,213,221,222,223,231,232,233,311,312,313,321,322,323,331,
332, 33














111
112
121
122
131
132
141
142
211
212
221
222
231
232
241
242
311
312
321
322
331
332
341
342
;;;;
run;quit;

data dim3;
  array[3,4,2]











%array(_x,values=1 2 3);
%array(_x,data=x,var=x);

%put &=_xn;
data want;
  array[

         ;;;;%end;%mend;/*'*/ *);*};*];*/;/*"*/;run;quit;%end;end;run;endcomp;%utlfix;
Given the following sas dataset and the sas
macro variable with contents 3 4 2

NUMS

col1

111
112
121
122
131
132
141
142
211
212
221
222
231
232
241
242
311
312
321
322
331
332
341
342

create a sas program  constructs this sas array statement

array x[3,4,2] (
111,
112,
121,
122,
131,
132,
141,
142,
211,
212,
221,
222,
231,
232,
241,
242,
311,
312,
321,
322,
331,
332,
341,
342 )
















111,112,121,122,211,212,221,222
























data dim3;
  /* Define array dimensions */
  array dims[3] _temporary_ (2, 2, 2);  /* Dimensions: 3x3x3 */

  /* Initialize with nested loops */
  do dim1 = 1 to dims[1];
    do dim2 = 1 to dims[2];
      do dim3 = 1 to dims[3];
        /* Calculate unique value for each cell (example pattern) */
        value = 100*dim1 + 10*dim2 + dim3;
        output;
      end;
    end;
  end;

  /* Define variable types */
  retain dim1-dim3 value;
  label dim1 = 'Layer' dim2 = 'Row' dim3 = 'Column';
  format value 8.;
run;


data _null_;
  declare hash h(dataset:'dim3');
  h.definekey('dim1','dim2','dim3');
  h.definedata('value');
  h.definedone();

  /* Get element (2,3,1) */
  dim1=2; dim2=3; dim3=1;
  rc = h.find();
  if rc=0 then put value=;
run;


































































%macro zero_array(ary);
  /*---- to hard to remember ----*/
  call stdize(
       'replace'
       ,'mult='
       ,0
       , of &ary[*]
       , _N_);
%mend zero_array;





 %macro mdarray(name,dims,mwork=2048,debug=0)/des="mutidimensional arrays"
 ;
%*********************************************************************;
%*                                                                   *;
%*  MACRO: MDARRAY                                                   *;
%*                                                                   *;
%*  USAGE: 1) MDARRAY array-name(dim1,dim2,...)                      *;
%*            %array-name(sub1,sub2,...)=value                       *;
%*            value=%array-name(sub1,sub2,...)                       *;
%*                                                                   *;
%*  DESCRIPTION:                                                     *;
%*    This macro is used to implement multi-dimensional arrays       *;
%*    using the native explicit array subscript and the macro        *;
%*    facility. It maps multiple subscripts onto the single          *;
%*    subscript which is allowed and generates the array reference   *;
%*    that is appropriate for the subscripts given. Basically,       *;
%*    this macro generates a macro with the same name as the array   *;
%*    name that you use. The generated macro has a parameter list    *;
%*    with the same number of parameters as subscripts specified.    *;
%*    It can be used in any place that a normal array reference      *;
%*    can be used. Some attempt has been made to stream-line the     *;
%*    data step execution by having the macro facility do all the    *;
%*    calculations that it can.                                      *;
%*                                                                   *;
%*  NOTES:                                                           *;
%*    This macro makes no attempt at verifying subscript ranges.     *;
%*                                                                   *;
%*********************************************************************;

%local qchar i l_name f l_dims dim_cnt c_pos;
%local dim plist p a macro offset;

%let p=%nrstr(%%);
%let a=%nrstr(&);
%let mwork=%eval(&mwork-400);    %* fudge it *;
%let name=%quote(&name);
%let dims=%quote(&dims);

%****************** parse the statement *****************************;
%if &name = %then %do;
   %put ERROR: Missing operands;
   %goto errout;
%end;

%if &dims= %then %do;
  %let f=%index(&name,%str(%());
  %if &f^=0 %then %do;
     %let dims=%qsubstr(&name,&f);
     %let name=%qsubstr(&name,1,&f-1);
  %end;
%end;

%let l_name=%length(&name);
%let l_dims=%length(&dims);

%****************** validate the name *******************************;
%if l_name=0 %then %do;
  %put ERROR: Missing array name;
  %goto errout;
%end;
%let qchar=%qupcase(%qsubstr(&name,1,1));
%if %index(ABCDEFGHIJKLMNOPQRSTUVWXYZ_,&qchar)=0 %then %do;
    %put ERROR: Missing/invalid array name;
    %goto errout;
%end;
%if %verify(%qupcase(&name),ABCDEFGHIJKLMNOPQRSTUVWXYZ_1234567890)^=0
  %then %do;
  %put %ERROR: Invalid array name;
  %goto errout;
%end;

%****************** validate the dimensions *************************;
%if &l_dims<3 %then %do;
  %put ERROR: Missing/invalid array dimensions;
  %goto errout;
%end;
%if %qsubstr(&dims,1,1) ^= %str(%() |
    %qsubstr(&dims,&l_dims) ^= %str(%)) %then %do;
   %put ERROR: Invalid dimension format - missing parens;
   %goto errout;
%end;
%let dim_cnt=0;
%let f=2;
%let c_pos=%eval(%index(&dims,%str(,))-1);
%do %while(&c_pos>0);
  %let dim_cnt=%eval(&dim_cnt+1);
  %local d&dim_cnt;
  %let d&dim_cnt=%qsubstr(&dims,&f,&c_pos-1);
  %if %datatyp(&&d&dim_cnt)=CHAR %then %do;
    %put ERROR: Invalid/missing dimension value;
    %goto errout;
  %end;
  %let f=%eval(&c_pos+&f);
  %let c_pos=%index(%qsubstr(&dims,&f),%str(,));
%end;
%let dim_cnt=%eval(&dim_cnt+1);
%local d&dim_cnt;
%let d&dim_cnt=%qsubstr(&dims,&f,&l_dims-&f);
%if %datatyp(&&d&dim_cnt)=CHAR %then %do;
  %put ERROR: Invalid/missing dimension value;
  %goto errout;
%end;

%*********************** define array *******************************;

%let dim=1;
%do i=1 %to &dim_cnt;
  %let dim=%eval(&dim*&&d&i);
  %let plist=&plist.X&i%str(,);
%end;
%let plist=(%substr(&plist,1,%length(&plist)-1));

array &name(&dim);

%****************** build the needed macro **************************;
%let macro=
  %str(&p.macro &name&plist; &p.local n_dim c_dim;);
%do i=1 %to &dim_cnt-1;
  %let offset=1;
  %do j=&i+1 %to &dim_cnt;
    %let offset=%eval(&offset*&&d&j);
  %end;
  %let macro=
    &macro%str(&p.if &p.datatyp(&a.X&i)=NUMERIC &p.then &p.let
    n_dim=&p.eval(&a.n_dim+((&a.X&i-1)*&offset)); &p.else &p.do; &p.if
    &p.quote(&a.c_dim)^= &p.then &p.let
    c_dim=&a.c_dim+((&a.X&i-1)*&offset);&p.else &p.let
    c_dim=((&a.X&i-1)*&offset); &p.end;);
  %if %length(&macro)>&mwork %then %do;
    %put NOTE: MDARRAY macro compressing blanks;
    %put NOTE: Length of %nrstr(&)MACRO=%length(&macro);
    %let macro=%qcmpres(&macro);
  %end;
%end;
%let macro=
  &macro%str(&p.if &p.datatyp(&a.X&i)=NUMERIC &p.then &p.let
  n_dim=&p.eval(&a.n_dim+&a.X&i); &p.else &p.do; &p.if
  &p.quote(&a.c_dim)^= &p.then &p.let c_dim=&a.c_dim+&a.X&i; &p.else
  &p.let c_dim=&a.X&i; &p.end; &p.if &p.quote(&a.n_dim)= &p.then
  &name(&a.c_dim); &p.else &p.if &p.quote(&a.c_dim)= &p.then
  &name(&a.n_dim); &p.else &name(&a.c_dim+&a.n_dim); &p.mend &name;);

%unquote(&macro);

%****************** debug the macro generated ***********************;
%if &debug %then %do;
  %local line;
  %let line=*** STARTING ***;
  %let i=1;
  %do %while(%quote(&line)^=);
    %put &line%str(;);
    %let line=%qcmpres(%qscan(&macro,&i,%str(;)));
    %let i=%eval(&i+1);
  %end;
  %put *** END ***;
%end;

%errout:
%mend;
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
