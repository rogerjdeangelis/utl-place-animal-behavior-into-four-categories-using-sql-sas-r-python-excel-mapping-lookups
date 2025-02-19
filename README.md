# utl-place-animal-behavior-into-four-categories-using-sql-sas-r-python-excel-mapping-lookups
Place animal behavior into four categories using sql sas r python excel mapping lookups 
    %let pgm=utl-place-animal-behavior-into-four-categories-using-sql-sas-r-python-excel-mapping-lookups;

    Place animal behavior into four categories using sql sas r python excel mapping lookups

    %stop_submission;

                 SOLUTIONS

                     1 sas sql prxmatch
                     2 sas sql in operator
                     3 sas sql find function (not sutible for multi-word keywords lookups)
                     4 r sql in operator
                     5 python sql in operator
                     6 excel sql in operator
                     7 weakly related repos

    github
    https://tinyurl.com/ane2uadd
    https://github.com/rogerjdeangelis/utl-place-animal-behavior-into-four-categories-using-sql-sas-r-python-excel-mapping-lookups

    stackoverflow r
    https://tinyurl.com/y7t3jxnh
    https://stackoverflow.com/questions/79448692/how-to-populate-column-based-on-multiple-types-of-strings-in-other-column


    /****************************************************************************************************************************/
    /*                      |                                                                      |                            */
    /*        INPUT         |             PROCESS                                                  |           OUTPUT           */
    /*                      | (use mapping to categorize behavior)                                 |           =======          */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /*   SD1.MAPPINGS       | PSEUDO CODE MAPPING KEYWORDS TO BEHAVIOR CATEGORIES                  |       SD1.EANT             */
    /*                      |                                                                      |                            */
    /* BEHAVIOR     CAT     | if behavior in (fin raise or ram or bite) then fight                 |     CAT     BEHAVIOR       */
    /*                      | else if behavior in (avoid or dive) then flee                        |                            */
    /* fin raise   fight    | else if behavior in (bump or splash) then join                       |    fight    bite           */
    /* ram         fight    | else unk                                                             |    fight    fin raise      */
    /* bite        fight    |                                                                      |    fight    ram            */
    /*                      |                                                                      |    flee     avoid          */
    /* avoid       flee     | 1 SAS SQL PRXMATCH                                                   |    flee     dive           */
    /* dive        flee     | ==================                                                   |    join     bump           */
    /*                      |                                                                      |    join     splash         */
    /* bump        join     | proc sql;                                                            |    unk      eat            */
    /* splash      join     |  create                                                              |                            */
    /*                      |     table wantprx as                                                 |                            */
    /*                      |  select                                                              |                            */
    /*  SD1.LOOKUPS         |   case                                                               |                            */
    /*                      |    when prxmatch("/\b(fin raise|ram|bite)\b/",behavior) then "fight" |                            */
    /* BEHAVIOR             |    when prxmatch("/\b(avoid|dive)\b/", behavior) then "flee"         |                            */
    /*                      |    when prxmatch("/\b(bump|splash)\b/", behavior) then "join"        |                            */
    /* fin raise            |    else "unk"                                                        |                            */
    /* ram                  |   end as cat                                                         |                            */
    /* bite                 |   ,behavior                                                          |                            */
    /* avoid                |  from                                                                |                            */
    /* dive                 |    sd1.lookups                                                       |                            */
    /* bump                 |  order                                                               |                            */
    /* splash               |    by cat, behavior                                                  |                            */
    /* eat                  | ;quit;                                                               |                            */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /* ptions               | 2 SAS SQL IN OPERATOR                                                |                            */
    /* validvarname=upcase; | =====================                                                |                            */
    /* ibname sd1 "d:/sd1"; |                                                                      |                            */
    /* ata sd1.mappings;    | proc sql;                                                            |                            */
    /* input                |  create                                                              |                            */
    /*  cat$                |     table wantin as                                                  |                            */
    /*  Behavior & $10. ;   |  select                                                              |                            */
    /* ards4;               |    case                                                              |                            */
    /* ight fin raise       |     when behavior in ("fin raise","ram","bite") then "fight"         |                            */
    /* ight ram             |     when behavior in ("avoid","dive") then "flee"                    |                            */
    /* ight bite            |     when behavior in ("bump","splash") then "join"                   |                            */
    /* lee avoid            |     else 'unk'                                                       |                            */
    /* lee dive             |    end as cat                                                        |                            */
    /* oin bump             |   ,behavior                                                          |                            */
    /* oin splash           |  from                                                                |                            */
    /* ;;;                  |    sd1.lookups                                                       |                            */
    /* un;quit;             |  order                                                               |                            */
    /*                      |    by cat, behavior                                                  |                            */
    /* ata sd1.lookups;     | ;quit;                                                               |                            */
    /* nformat              |                                                                      |                            */
    /*  Behavior $24.;      |                                                                      |                            */
    /* nput Behavior &;     | 3 SAS SQL FIND FUNCTION                                              |                            */
    /* ards4;               | =======================                                              |                            */
    /* in raise             |                                                                      |                            */
    /* am                   | proc sql;                                                            |                            */
    /* ite                  |  create                                                              |                            */
    /* void                 |     table wantin as                                                  |                            */
    /* ive                  |  select                                                              |                            */
    /* ump                  |    case                                                              |                            */
    /* plash                |     when find("fin raise ram  bite",behavior,"t") then "fight"       |                            */
    /* at                   |     when find("avoid dive",behavior,"t") then "flee"                 |                            */
    /* ;;;                  |     when find("bump splash",behavior,"t") then "join"                |                            */
    /* un;quit;             |     else 'unk'                                                       |                            */
    /*                      |    end as cat                                                        |                            */
    /*                      |   ,behavior                                                          |                            */
    /*                      |  from                                                                |                            */
    /*                      |    sd1.lookups                                                       |                            */
    /*                      |  order                                                               |                            */
    /*                      |    by cat, behavior                                                  |                            */
    /*                      | ;quit;                                                               |                            */
    /*                      |                                                                      |                            */
    /*                      |----------------------------------------------------------------------|                            */
    /*                      |                                                                      |                            */
    /*                      | 4 R SQL IN OPERATOR                                                  |                            */
    /*                      | ===================                                                  |                            */
    /*                      |                                                                      |                            */
    /*                      | proc datasets lib=sd1 nolist nodetails;                              |                            */
    /*                      |  delete want;                                                        |                            */
    /*                      | run;quit;                                                            |                            */
    /*                      |                                                                      |                            */
    /*                      | %utl_rbeginx;                                                        |                            */
    /*                      | parmcards4;                                                          |                            */
    /*                      | library(haven)                                                       |                            */
    /*                      | library(sqldf)                                                       |                            */
    /*                      | source("c:/oto/fn_tosas9x.R")                                        |                            */
    /*                      | lookups<-read_sas("d:/sd1/lookups.sas7bdat")                         |                            */
    /*                      | want<-sqldf('                                                        |                            */
    /*                      |  select                                                              |                            */
    /*                      |    case                                                              |                            */
    /*                      |     when behavior in ("fin raise","ram","bite") then "fight"         |                            */
    /*                      |     when behavior in ("avoid","dive") then "flee"                    |                            */
    /*                      |     when behavior in ("bump","splash") then "join"                   |                            */
    /*                      |     else "unk"                                                       |                            */
    /*                      |    end as cat                                                        |                            */
    /*                      |   ,behavior                                                          |                            */
    /*                      |  from                                                                |                            */
    /*                      |    lookups                                                           |                            */
    /*                      |  order                                                               |                            */
    /*                      |    by cat, behavior                                                  |                            */
    /*                      |  ')                                                                  |                            */
    /*                      | want                                                                 |                            */
    /*                      | fn_tosas9x(                                                          |                            */
    /*                      |       inp    = want                                                  |                            */
    /*                      |      ,outlib ="d:/sd1/"                                              |                            */
    /*                      |      ,outdsn ="want"                                                 |                            */
    /*                      |      )                                                               |                            */
    /*                      | ;;;;                                                                 |                            */
    /*                      | %utl_rendx;                                                          |                            */
    /*                      |                                                                      |                            */
    /*                      | proc print data=sd1.want;                                            |                            */
    /*                      | run;quit;                                                            |                            */
    /*                      |                                                                      |                            */
    /*                      |----------------------------------------------------------------------|                            */
    /*                      |                                                                      |                            */
    /*                      | 5 PYTHON SQL IN OPERATOR                                             |                            */
    /*                      | ========================                                             |                            */
    /*                      |                                                                      |                            */
    /*                      | proc datasets lib=sd1 nolist nodetails;                              |                            */
    /*                      |  delete pywant;                                                      |                            */
    /*                      | run;quit;                                                            |                            */
    /*                      |                                                                      |                            */
    /*                      | %utl_pybeginx;                                                       |                            */
    /*                      | parmcards4;                                                          |                            */
    /*                      | exec(open('c:/oto/fn_python.py').read());                            |                            */
    /*                      | lookups,meta = ps.read_sas7bdat('d:/sd1/lookups.sas7bdat');          |                            */
    /*                      | want=pdsql('''                                                       |                            */
    /*                      |  select                                                      \       |                            */
    /*                      |    case                                                      \       |                            */
    /*                      |     when behavior in ("fin raise","ram","bite") then "fight" \       |                            */
    /*                      |     when behavior in ("avoid","dive") then "flee"            \       |                            */
    /*                      |     when behavior in ("bump","splash") then "join"           \       |                            */
    /*                      |     else "unk"                                               \       |                            */
    /*                      |    end as cat                                                \       |                            */
    /*                      |   ,behavior                                                  \       |                            */
    /*                      |  from                                                        \       |                            */
    /*                      |    lookups                                                   \       |                            */
    /*                      |  order                                                       \       |                            */
    /*                      |    by cat, behavior                                          \       |                            */
    /*                      |  ''')                                                                |                            */
    /*                      | print(want);                                                         |                            */
    /*                      | fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);         |                            */
    /*                      | ;;;;                                                                 |                            */
    /*                      | %utl_pyendx;                                                         |                            */
    /*                      |                                                                      |                            */
    /*                      | proc print data=sd1.pywant;                                          |                            */
    /*                      | run;quit;                                                            |                            */
    /*                      |                                                                      |                            */
    /*                      |---------------------------------------------------------------------------------------------------*/
    /*                      |                                                                      |                            */
    /*                      | 6 EXCEL SQL IN OPERATOR                                              |                            */
    /*                      | =======================                                              |                            */
    /*                      |                                                                      |                            */
    /*                      | proc datasets lib=sd1 nolist nodetails;                              | INPUT                      */
    /*                      |  delete want;                                                        |                            */
    /*                      | run;quit;                                                            | d:/xls/wantxl.lsx          */
    /*                      |                                                                      | Sheet have                 */
    /*                      | %utlfkil(d:/xls/wantxl.xlsx);                                        |                            */
    /*                      |                                                                      | --------------+            */
    /*                      | %utl_rbeginx;                                                        | | A1| fx |CAT |            */
    /*                      | parmcards4;                                                          | --------------+            */
    /*                      | library(openxlsx)                                                    | [_] |    B    |            */
    /*                      | library(sqldf)                                                       | --------------|            */
    /*                      | library(haven)                                                       |  1  |BEHAVIOR |            */
    /*                      | have<-read_sas("d:/sd1/lookups.sas7bdat")                            |  -- |---------|            */
    /*                      | wb <- createWorkbook()                                               |  2  |bite     |            */
    /*                      | addWorksheet(wb, "have")                                             |  -- |---------|            */
    /*                      | writeData(wb, sheet = "have", x = have)                              |  3  |fin raise|            */
    /*                      | saveWorkbook(                                                        |  -- |---------|            */
    /*                      |     wb                                                               |  4  |ram      |            */
    /*                      |    ,"d:/xls/wantxl.xlsx"                                             |  -- |---------|            */
    /*                      |    ,overwrite=TRUE)                                                  |  5  |avoid    |            */
    /*                      | ;;;;                                                                 |  -- |---------|            */
    /*                      | %utl_rendx;                                                          |  6  |dive     |            */
    /*                      |                                                                      |  -- |---------|            */
    /*                      | %utl_rbeginx;                                                        |  7  |bump     |            */
    /*                      | parmcards4;                                                          |  -- |---------|            */
    /*                      | library(openxlsx)                                                    |  8  |splash   |            */
    /*                      | library(sqldf)                                                       |  -- |---------|            */
    /*                      | source("c:/oto/fn_tosas9x.R")                                        |  9  |eat      |            */
    /*                      |  wb<-loadWorkbook("d:/xls/wantxl.xlsx")                              |  -- |---------|            */
    /*                      |  have<-read.xlsx(wb,"have")                                          | [HAVE]                     */
    /*                      |  addWorksheet(wb, "want")                                            |                            */
    /*                      |  want<-sqldf('                                                       |  OUTPUT                    */
    /*                      |  select                                                              |                            */
    /*                      |    case                                                              | d:/xls/wantxl.lsx          */
    /*                      |     when behavior in ("fin raise","ram","bite") then "fight"         | Sheet WANT                 */
    /*                      |     when behavior in ("avoid","dive") then "flee"                    | -------------------------+ */
    /*                      |     when behavior in ("bump","splash") then "join"                   | | A1| fx       |CAT      | */
    /*                      |     else "unk"                                                       | -------------------------+ */
    /*                      |    end as cat                                                        | [_] |    A     |    B    | */
    /*                      |   ,behavior                                                          | -------------------------| */
    /*                      |  from                                                                |  1  | CAT      |BEHAVIOR | */
    /*                      |    have                                                              |  -- |----------+---------| */
    /*                      |  order                                                               |  2  |fight     |bite     | */
    /*                      |    by cat, behavior                                                  |  -- |----------+---------| */
    /*                      |   ')                                                                 |  3  |fight     |fin raise| */
    /*                      |  print(want)                                                         |  -- |----------+---------| */
    /*                      |  writeData(wb,sheet="want",x=want)                                   |  4  |fight     |ram      | */
    /*                      |  saveWorkbook(                                                       |  -- |----------+---------| */
    /*                      |      wb                                                              |  5  |flee      |avoid    | */
    /*                      |     ,"d:/xls/wantxl.xlsx"                                            |  -- |----------+---------| */
    /*                      |     ,overwrite=TRUE)                                                 |  6  |flee      |dive     | */
    /*                      | fn_tosas9x(                                                          |  -- |----------+---------| */
    /*                      |       inp    = want                                                  |  7  |join      |bump     | */
    /*                      |      ,outlib ="d:/sd1/"                                              |  -- |----------+---------| */
    /*                      |      ,outdsn ="want"                                                 |  8  |join      |splash   | */
    /*                      |      )                                                               |  -- |----------+---------| */
    /*                      | ;;;;                                                                 |  9  |unk       |eat      | */
    /*                      | %utl_rendx;                                                          |  -- |----------+---------| */
    /*                      |                                                                      | [WANT]                     */
    /*                      | proc print data=sd1.want;                                            |                            */
    /*                      | run;quit;                                                            |                            */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /*                      |                                                                      |                            */
    /*--------------------------------------------------------------------------------------------------------------------------*/
    /*                                                                                                                          */
    /* 7 WEAKLY RELATED REPOS                                                                                                   */
    /* ======================                                                                                                   */
    /*                                                                                                                          */
    /* https://github.com/rogerjdeangelis/utl-an-additional-algorithm-to-map-codes-to-decodes                                   */
    /* https://github.com/rogerjdeangelis/utl-ingenious-technique-to-map-F-D-A-to-1-2-3                                         */
    /* https://github.com/rogerjdeangelis/utl-mapping-clinical-terms-to-descriptions-for-a-large-number-of-vocabularies         */
    /* https://github.com/rogerjdeangelis/utl-reduce-your-sdtm-and-adam-mapping-effort-using-a-common-meta-data-model           */
    /* https://github.com/rogerjdeangelis/utl_binning_36_character_and_numeric_variables_with_separate_mappings                 */
    /* https://github.com/rogerjdeangelis/utl-enhanced-common-meta-data-model-to-reduce-CDSIC-mapping-efforts                   */
    /* https://github.com/rogerjdeangelis/utl-cdisc-check-adam-table-mappings                                                   */
    /* https://github.com/rogerjdeangelis/utl-efficient-lookup-of-a_47-million-subset-of_50-million                             */
    /* https://github.com/rogerjdeangelis/utl-faster-index-lookups-when-array-elements-are-not-contiguous-in-the-pdv            */
    /* https://github.com/rogerjdeangelis/utl-simple-lookup-code-decode-using-macro-array-format                                */
    /* https://github.com/rogerjdeangelis/utl-simplest-case-of-a-hash-or-sql-lookup                                             */
    /* https://github.com/rogerjdeangelis/utl_hash_lookup_with_multiple_keys_nice_simple_example                                */
    /*                                                                                                                          */
    /****************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
