# utl-using-dosubl-to-avoid-klingon-obsucated-macro-coding
    Using dosubl to avoid klingon obsucated macro coding

      Two Solutions

          1.  "dir ""c:\windows\&soft.dist"" /b"
          Bartosz Jablonski
          yabwon@gmail.com
          
      2.  Although it’s harder to read, this won’t fail
          if the filename (&soft) contains a double quote:
          %let soft=a;
          %let dist=b;
          %put %sysfunc(quote(dir %sysfunc(quote(c:\windows\&soft.&dist)) /b));
          "dir ""c:\windows\ab"" /b"
          Tom Robinson
          barefootguru@gmail.com

       3. Another solution below          


    github
    https://tinyurl.com/re2pfoj
    https://github.com/rogerjdeangelis/utl-using-dosubl-to-avoid-klingon-obsucated-macro-coding

    SAS Forum
    https://tinyurl.com/tdhrosk
    https://communities.sas.com/t5/SAS-Programming/Must-I-macro-quote-or-not-quote-if-quote-then-what-quoting/m-p/631950

    This solution resolves the macro code ata macro execution time not
    datastep compile or execution time.

    *                _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | '_ \| '__/ _ \| '_ \| |/ _ \ '_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    ;

    I need to resolve the following at macro execution time

       'dir "c:\windows\&soft.dist" /b'

    This works

        %unquote(%str(%'dir "c:\windows\&soft.dist" /b%'))

    But this looks like  klingon obsucated macro coding

        %unquote(%str(%' ... %')

    Do not waste your time learning all the idiosycracies
    of macro coding, just use your
    knowledge of datastep programmimg,

        Two Solutions

             a. dosubl
             b. %dosubl (macro wrapper on dosubl - generic resolver?)

    dosubl mmacro on the end

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    dir ""c:\windows\softwaredistribution"" /b"
       AuthCabs
       DataStore
       Download
       PostRebootEventCache
       ReportingEvents.log
       SelfUpdate
       WuRedir
    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/
                  _                 _     _
      __ _      __| | ___  ___ _   _| |__ | |
     / _` |    / _` |/ _ \/ __| | | | '_ \| |
    | (_| |_  | (_| | (_) \__ \ |_| | |_) | |
     \__,_(_)  \__,_|\___/|___/\__,_|_.__/|_|
    ;
    %symdel soft dist cmd / nowarn;
    %let soft=software;
    %let dist=distribution;
    %let rc=%qsysfunc(dosubl(%nrstr(
        data _null_;
            cmd=resolve('dir "c:\windows\&soft.&dist" /b');
            call symputx("cmd",quote(trim(cmd)));
        run;quit;
    )));

    filename filelist pipe &cmd;
    data _null_;
      infile filelist;
      input;
      put _infile_;
    run;quit;
    *_        _  __     _                 _     _
    | |__    (_)/ /  __| | ___  ___ _   _| |__ | |
    | '_ \     / /  / _` |/ _ \/ __| | | | '_ \| |
    | |_) |   / /_ | (_| | (_) \__ \ |_| | |_) | |
    |_.__(_) /_/(_) \__,_|\___/|___/\__,_|_.__/|_|
    ;
    %symdel soft dist cmd / nowarn;
    %let soft=software;
    %let dist=distribution;
    %dosubl(%nrstr(
        data _null_;
            cmd=resolve('dir "c:\windows\&soft.&dist" /b');
            call symputx("cmd",quote(trim(cmd)));
        run;quit;
    ));
    filename filelist pipe &cmd;
    data _null_;
      infile filelist;
      input;
      put _infile_;
    run;quit;
    *    _                 _     _
      __| | ___  ___ _   _| |__ | |  _ __ ___   __ _  ___ _ __ ___
     / _` |/ _ \/ __| | | | '_ \| | | '_ ` _ \ / _` |/ __| '__/ _ \
    | (_| | (_) \__ \ |_| | |_) | | | | | | | | (_| | (__| | | (_) |
     \__,_|\___/|___/\__,_|_.__/|_| |_| |_| |_|\__,_|\___|_|  \___/
    ;
    %macro dosubl(arg);
      %let rc=%qsysfunc(dosubl(&arg));
    %mend dosubl;


