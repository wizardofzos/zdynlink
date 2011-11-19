## zdynlink

More and more applications on z/OS are doing stuff inside of Unix System Services.
WebSphere being one of them.

Loads of these applications are writing files there too.

Suppose you have a job like the following:

    //SOMEJOB    JOB  (ACCTCODE),
    //                CLASS=J,
    //                MSGCLASS=X
    //*
    //*   My Example Job
    //STEP01     EXEC PGM=IEBGENER
    //SYSPRINT   DD   SYSOUT=*
    //SYSUT1     DD   DSN=SOMEFILE,DISP=SHR
    //SYSUT2     DD   PATH='/tmp/somefile'
    //SYSIN      DD   DUMMY


