# utl-computing-unique-counts-for-combinations-of-dimension-variables
Computing unique counts for combinations of dimension variables

    %let pgm=utl-computing-unique-counts-for-combinations-of-dimension-variables;

    Computing unique counts for combinations of dimension variables

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data class;
     informat NAME SEX AGE $8.;
     input SEX AGE NAME ;
    cards4;
    M 14 Alfred
    F 14 Alfred
    X 21 Alfred
    F 33 Alfred
    F 13 Alice
    F 14 Alice
    X 21 Alice
    F 33 Alice
    F 13 Barbara
    F 14 Barbara
    F 13 Barbara
    ;;;;
    run;quit;

    Up to 40 obs WORK.CLASS total obs=11 06MAY2022:16:19:28


                                  |  RULES
    Obs     NAME      SEX    AGE  |
                                  |
      1    Alfred      M     14   |  Alfred has 3 sex and 3 ages
      2    Alfred      F     14   |
      3    Alfred      X     21   |
      4    Alfred      F     33   |

      5    Alice       F     13   |  Alice has 2 sex and 4 ages
      6    Alice       F     14   |
      7    Alice       X     21   |
      8    Alice       F     33   |

      9    Barbara     F     13   |  Barbara has 1 sex and 2 ages
     10    Barbara     F     14   |
     11    Barbara     F     13   |

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    NAME       cntname    unqSex    unqAge
    --------------------------------------
    Alfred           4         3         3
    Alice            4         2         4
    Barbara          3         1         2

    /*         _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| `_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    */

    proc sql;
       select
        distinct
         l.name
        ,r.cntName
        ,count(distinct sex) as unqSex
        ,count(distinct age) as unqAge
      from
        (select distinct name, sex, age from class) as l
      ,
        (select name as nam, count(name) as cntname from class group by name) as r
      where
        l.name = r.nam
      group
        by name
    ;quit;


