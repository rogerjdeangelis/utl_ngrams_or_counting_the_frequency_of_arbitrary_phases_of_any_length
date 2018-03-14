# utl_ngrams_or_counting_the_frequency_of_arbitrary_phases_of_any_length
Counting the frequency of arbitrary phases of any length (ngrams).  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS/WPS/R Counting the frequency of arbitrary phases of any length (ngrams)

    I want to find the frequency of ALL three word phrases,without defining the three words.
    You do not have to define the phrases you are looking for ahead of time.

    If you have IML you can cut and paste the code below into IML
    Perl and Oython are prbably faster?

    see github
    https://tinyurl.com/ydcbchxm
    https://github.com/rogerjdeangelis/utl_ngrams_or_counting_the_frequency_of_arbitrary_phases_of_any_length

    see
    https://goo.gl/bCbmnc
    http://stackoverflow.com/questions/43496348/text-mining-count-frequencies-of-phrases-more-than-one-word

    Imran Ali profile
    http://stackoverflow.com/users/923194/imran-ali


    HAVE ( a long text string in dataset have)
    ==========================================

    SD1.HAVE

    Middle Observation(1 ) of SD1.HAVE - Total Obs 1

     -- CHARACTER --
    TEXT      C    187     This is my littl..

    TEXT=This is my little R text example and I want
         to count the frequency of some pattern (and -
         is - my - of). This is my little R text example
         and I want to count the frequency of some patter.


    WANT (ngrams of size 3)
    =======================

    Up to 40 obs from ngram_wps total obs=22

    Obs    W1W2                   FREQ    LENGTH

      1    I want to                2        9
      2    R text example           2       14
      3    This is my               2       10
      4    and I want               2       10
      5    and is my                1        9
      6    count the frequency      2       19
      7    example and I            2       13
      8    frequency of some        2       17
      9    is my little             2       12
     10    is my of                 1        8
     11    little R text            2       13
     12    my little R              2       11
     13    my of This               1       10
     14    of This is               1       10
     15    of some patter           1       14
     16    of some pattern          1       15
     17    pattern and is           1       14
     18    some pattern and         1       16
     19    text example and         2       16
     20    the frequency of         2       16
     21    to count the             2       12
     22    want to count            2       13

     *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    text =compbl( "This is my little R text example and I want to count the frequency
     of some pattern (and - is - my - of). This is my little
     R text example and I want to count the frequency of some patter.");
     put text=;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("c:/Program Files/R/R-3.3.2/etc/Rprofile.site",echo=T);
    library(tau);
    library(data.table);
    library(haven);
    text<-read_sas("d:/sd1/have.sas7bdat");
     createNgram <-function(stringVector, ngramSize){
      ngram <- data.table();
      ng <- textcnt(stringVector, method = "string", n=ngramSize, tolower = FALSE);
      if(ngramSize==1){
        ngram <- data.table(w1 = names(ng), freq = unclass(ng), length=nchar(names(ng)));
      }
      else {
        ngram <- data.table(w1w2 = names(ng), freq = unclass(ng), length=nchar(names(ng)));
      };
      return(ngram)
    };
    res <- createNgram(text, 3);
    endsubmit;
    import r=res data=wrk.ngram_wps;
    run;quit;
    ');

    NOTE: Processing of R statements complete

    19        import r=res data=wrk.ngram_wps;
    NOTE: Creating data set 'WRK.ngram_wps' from R data frame 'res'
    NOTE: Column names modified during import of 'res'
    NOTE: Data set "WRK.ngram_wps" has 22 observation(s) and 3 variable(s)

    20        run;
    NOTE: Procedure r step took :
          real time : 0.791
          cpu time  : 0.000


    21        quit;

    NOTE: Submitted statements took :
          real time : 0.838
          cpu time  : 0.046
