# utl_dosubl_subroutine_interfaces
Interfacing DOSUBL with parent address spaces

    ```  SAS Forum: Building the ISPERFECTSQUARE and ISFIBONNACINUMBER Functions with Proc FCMP/DOSUBL  ```
    ```    ```
    ```       There is big issue with DOSUBL, it looks like SAS saves and restores  ```
    ```       whole environments instead of integrating the DOSUBL into parent address space.  ```
    ```    ```
    ```       Call execute throws out the parent enviroment and just stacks code.  ```
    ```    ```
    ```       WORKING CODE (four solutions)  ```
    ```    ```
    ```       1.  DOSUBL with common storage -- VERY SLOW - compiler needs work;  ```
    ```    ```
    ```           MAINLINE  ```
    ```            %commonn(i);    * common i variable (not a macro variable)  ```
    ```            do i= 1 to 4;  ```
    ```    ```
    ```           DOSUBL (commonn macro on end)  ```
    ```    ```
    ```            rc=dosubl('  ```
    ```                 %commonn(i,action=GET); * matching common (like an argument to dosubl _;  ```
    ```                 sqrt=int(sqrt(i));  ```
    ```                 if sqrt*sqrt=i then call symputx("Answer"," isPerfectSquare","G");  ```
    ```                 else call symputx("Answer","NotPerfectSquare","G");  ```
    ```            ');  ```
    ```    ```
    ```           MAINLINE  ```
    ```           put i= answer=;  ```
    ```    ```
    ```       2.  DOSUBL with macro variable  ```
    ```    ```
    ```           do i= 1 to 4;  ```
    ```             call symputx('i',put(i,3.));  ```
    ```             rc=dosubl('  ```
    ```               data _null_;  ```
    ```                  sqrt=int(sqrt(i));  ```
    ```                  if sqrt*sqrt=i then call symputx("Answer"," isPerfectSquare","G");  ```
    ```                  else call symputx("Answer","NotPerfectSquare","G");  ```
    ```             ');  ```
    ```             put i= answer=;  ```
    ```    ```
    ```       3.  DOSUBL calling a macro  ```
    ```    ```
    ```           %macro isperfectsquare(num) ;  ```
    ```               data _null_;  ```
    ```                 sqrt=int(sqrt(&num));  ```
    ```                 if sqrt*sqrt=&num then call symputx("YN","Y","G");  ```
    ```                 else call symputx("YN","N","G");  ```
    ```               run;quit;  ```
    ```           %mend isperfectsquare;  ```
    ```    ```
    ```    ```
    ```           %symdel num / nowarn;  ```
    ```           data _null_;  ```
    ```                 do i=1 to 16;  ```
    ```                    call symputx('num',put(i,3.));  ```
    ```                    rc=dosubl('%isperfectsquare(&num);');  ```
    ```                    if symget('YN')='Y' then put i= 'is a perfect square';  ```
    ```                 end;  ```
    ```           run;  ```
    ```    ```
    ```        4 see for FCMP solution  ```
    ```    ```
    ```          https://goo.gl/vGTu7m  ```
    ```          https://communities.sas.com/t5/SAS-Communities-Library/Building-the-ISPERFECTSQUARE-and-ISFIBONNACINUMBER-Functions/ta-p/394593  ```
    ```    ```
    ```          ChrisBrooks  ```
    ```          https://communities.sas.com/t5/user/viewprofilepage/user-id/32246  ```
    ```    ```
    ```  HAVE  ```
    ```  ====  ```
    ```    ```
    ```    The follwing mainline code  ```
    ```    ```
    ```        do i= 1 to 4;   * rules identify perfect number 1=1*1 and 4 = 2*2 (square of same number);  ```
    ```        end;  ```
    ```    ```
    ```  WANT  ```
    ```    ```
    ```      I=1 ANSWER=isPerfectSquare  ```
    ```    ```
    ```      I=2 ANSWER=NotPerfectSquare  ```
    ```      I=3 ANSWER=NotPerfectSquare  ```
    ```    ```
    ```      I=4 ANSWER=isPerfectSquare  ```
    ```    ```
    ```  *                _               _       _  ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|  ```
    ```  ;  ```
    ```    ```
    ```    do i= 1 to 4;   * rules identify perfect number 1=1*1 and 4 = 2*2 (square of same number);  ```
    ```    end;  ```
    ```    ```
    ```  *          _       _   _                _  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __    / |  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \   | |  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  | |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  |_|  ```
    ```    ```
    ```  ;  ```
    ```  *commonn;  ```
    ```    ```
    ```  data _null_;  ```
    ```    %commonn(i);  ```
    ```    do i= 1 to 4;  ```
    ```      rc=dosubl('  ```
    ```        data _null_;  ```
    ```           %commonn(i,action=GET);  ```
    ```           sqrt=int(sqrt(i));  ```
    ```           if sqrt*sqrt=i then call symputx("Answer"," isPerfectSquare","G");  ```
    ```           else call symputx("Answer","NotPerfectSquare","G");  ```
    ```        run;quit;  ```
    ```      ');  ```
    ```      answer=symget('answer');  ```
    ```      put i= answer=;  ```
    ```    end;  ```
    ```  run;quit;  ```
    ```    ```
    ```  *          _       _   _                ____  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __    |___ \  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \     __) |  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |   / __/  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  |_____|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  * macro variables;  ```
    ```    ```
    ```  data _null_;  ```
    ```    do i= 1 to 4;  ```
    ```      call symputx('i',put(i,3.));  ```
    ```      rc=dosubl('  ```
    ```        data _null_;  ```
    ```           i=input(symget("i"),3.);  ```
    ```           sqrt=int(sqrt(i));  ```
    ```           if sqrt*sqrt=i then call symputx("Answer"," isPerfectSquare","G");  ```
    ```           else call symputx("Answer","NotPerfectSquare","G");  ```
    ```        run;quit;  ```
    ```      ');  ```
    ```      answer=symget('answer');  ```
    ```      put i= answer=;  ```
    ```    end;  ```
    ```  run;quit;  ```
    ```    ```
    ```  *          _       _   _               _____  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __   |___ /  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \    |_ \  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  ___) |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_| |____/  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  %macro isperfectsquare(num) ;  ```
    ```      data _null_;  ```
    ```        sqrt=int(sqrt(&num));  ```
    ```        if sqrt*sqrt=&num then call symputx("YN","Y","G");  ```
    ```        else call symputx("YN","N","G");  ```
    ```      run;quit;  ```
    ```  %mend isperfectsquare;  ```
    ```    ```
    ```    ```
    ```  %symdel num / nowarn;  ```
    ```  data _null_;  ```
    ```        do i=1 to 16;  ```
    ```           call symputx('num',put(i,3.));  ```
    ```           rc=dosubl('%isperfectsquare(&num);');  ```
    ```           if symget('YN')='Y' then put i= 'is a perfect square';  ```
    ```        end;  ```
    ```  run;  ```
    ```    ```
    ```  *  ```
    ```    ___ ___  _ __ ___  _ __ ___   ___  _ __ ___  _ __  ```
    ```   / __/ _ \| '_ ` _ \| '_ ` _ \ / _ \| '_ ` _ \| '_ \  ```
    ```  | (_| (_) | | | | | | | | | | | (_) | | | | | | | | |  ```
    ```   \___\___/|_| |_| |_|_| |_| |_|\___/|_| |_| |_|_| |_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  %macro commonn(var,action=init);  ```
    ```     %if %upcase(&action) = INIT %then %do;  ```
    ```        retain &var 0;  ```
    ```        call symputx("varadr",put(addrlong(&var.),hex16.),"G");  ```
    ```     %end;  ```
    ```     %else %if "%upcase(&action)" = "GET" %then %do;  ```
    ```        &var = input(peekclong("&varadr."x,8),rb8.);  ```
    ```     %end;  ```
    ```  %mend commonn;  ```
    ```    ```
    ```    ```
    ```    ```
