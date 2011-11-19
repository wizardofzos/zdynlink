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
    //*
    //STEP01     EXEC PGM=IEBGENER
    //SYSPRINT   DD   SYSOUT=*
    //SYSUT1     DD   DSN=SOMEFILE,DISP=SHR
    //SYSUT2     DD   PATH='/tmp/somefile'
    //SYSIN      DD   DUMMY

This would then copy your 'SOMEFILE' to /tmp/somefile.
But what if you wanted to have this file within the USS environment be 
unique every time you ran the job?

Come on in zdynlink (thanks to Bas for the name).

If, within your USS environment you would have had the *zdynlink* command your could have
done the following:

    IBMUSER:/u/ibmuser: >zdynlink /tmp/somefile
    Done. Any writes to /tmp/somefile will result in a file /tmp/somefile&JOBNAME.-&LYYMMDD.-&LHHMMSS.

Making your /tmp look like this :

    IBMUSER:/u/ibmuser: >ls -la /tmp
    lrwxrwxrwx   1 OMVSKERN SYS1          12 Jun 25  2010 /tmp -> $SYSNAME/tmp/somefile-&JOBNAME.-&LYYMMDD.-&LHHMMSS.

Now what happens when you write to /tmp/somefile ?

    IBMUSER:/u/ibmuser: >echo "Just a test for github post" > /tmp/somefile
    IBMUSER:/u/ibmuser: >ls -la /tmp/
    total 48
    drwxrwxrwt   2 OMVSKERN SYS1        8192 Nov 19 14:23 .
    drwxr-xr-x   6 OMVSKERN SYS1        8192 Jun 25  2010 ..
    -rw-------   1 100000   SYS1          27 Nov 13 14:14 .sh_history
    lrwxrwxrwx   1 OMVSKERN SYS1          51 Nov 19 14:21 somefile -> $SYSSYMA/tmp/somefile-&JOBNAME.-&LYYMMDD.-&LHHMMSS.
    -rw-r--r--   1 OMVSKERN SYS1          28 Nov 19 14:23 somefile-IBMUSER7-111119-142316

See! You get a nice filename with JOBNAME, DATE and TIMESTAMP in it!

It's very handy on a lot of stuff. You might want to use it on "applyPTF.out" to name just one :)


Future version of zdynlink will enable you to select what 'SYSTEM SYMBOLS' you want to have in your symbolic link.

For now, enjoy version 0.1 and let me know what you think!
