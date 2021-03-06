# JCL job to customize SMP/e global zone environment

**`CUSTGLB` - This JCL job is used to customize SMPe global zone environment**

[**Link to clone/download from the repository**](https://github.com/IBA-mainframe-dev/Global-Repository-for-Mainframe-Developers/blob/master/zOS%20System%20operating/SMPe/JCL%20job%20for%20customizing%20SMPe%20global%20zone%20environment/CUSTGLB)

```
//CUSTGLB  JOB <JOB PARAMETERS>
//******************************************************//
//*  THIS JOB IS USED TO CUSTOMIZE                     *//
//*  SMP/E GLOBAL ZONE ENVIRONMENT                     *//
//*                                                    *//
//*  INSTRUCTIONS:                                     *//
//*  1. CHANGE SMPEHLQ TO THE HLQ YOU NEED             *//
//*  2. REPLACE USERHLQ WITH SMPEHLQ VALUE             *//
//*  3. REPLACE XXXXXX IN ZONEDESCRIPTION(XXXXXX)      *//
//*     WITH YOUR DESCRIPTION                          *//
//*  4. REPLACE XXX IN OPTIONS(XXX) WITH YOUR OPTIONS  *//
//*     NAME                                           *//
//******************************************************//
//       SET SMPEHLQ='#SMPE HLQ#'
//******************************************************//
//UPDGLBL EXEC PGM=GIMSMP,
// PARM='DATE=U',REGION=8M
//SMPLOG DD DISP=MOD,DSN=&SMPEHLQ..SMPLOG
//SMPLOGA DD DISP=MOD,DSN=&SMPEHLQ..SMPLOGA
//SMPRPT DD SYSOUT=*
//SMPPTS DD DISP=SHR,DSN=&SMPEHLQ..SMPPTS
//SMPCSI DD DSN=&SMPEHLQ..GB.CSI,DISP=SHR
//******************************************************//
//SMPCNTL DD *
  SET BOUNDARY(GLOBAL).
  UCLIN.
   ADD GLOBALZONE
   SREL(Z038)
    OPTIONS(XXX)
   ZONEINDEX(
   (TZONE,USERHLQ.GB.CSI,TARGET)
   (DZONE,USERHLQ.GB.CSI,DLIB)
   )
   ZONEDESCRIPTION(XXXXXX)
   .
  ADD OPTIONS(XXX)
   ASM(ASMA90)              /* SMP/E NAME FOR ASM H */
   COMP(IEBCOPY)            /* SMP/E NAME FOR COMPRESS */
   COPY(IEBCOPY)            /* SMP/E NAME FOR COPY */
   DSSPACE(200,100,500)     /* (PRIM,SEC,DIR) FOR TLIBS */
   DSPREFIX(USERHLQ)        /* PREFIX FOR TLIBS */
   RETRY(IEBCOPY)           /* SMP/E NAME FOR RETRY */
   RETRYDDN(ALL)            /* ALL ARE ELIGIBLE FOR X37 RETRY */
   UPDATE(IEBUPDTE)         /* SMP/E NAME FOR UPDATE */
   NOPURGE                  /* NO PTF DELETION AFTER ACCEPT */
   NOREJECT                 /* NO PTF DELETION AFTER RESTORE */
  .
  ADD UTILITY(ASMA90) NAME(ASMA90)
   PARM(XREF(SHORT),NOLOAD,NOLIST,DECK)
   RC(4) PRINT(SYSPRINT)
  .
  ADD UTILITY(IEBCOPY) NAME(IEBCOPY)
   RC(0) PRINT(SYSPRINT)
  .
  ADD UTILITY(IEBUPDTE) NAME(IEBUPDTE)
   RC(0) PRINT(SYSPRINT)
  .
  ADD UTILITY(IEWL) NAME(IEWL)
   PARM(COMPAT=LKED)
   RC(8) PRINT(SYSPRINT)
  .
  ADD DDDEF(SMPLOG) MOD DA(USERHLQ.SMPLOG)
  .
  ADD DDDEF(SMPLOGA) MOD DA(USERHLQ.SMPLOGA)
  .
  ADD DDDEF(SMPPTS) SHR DA(USERHLQ.SMPPTS)
  .
  ADD DDDEF(SMPSTS) SHR DA(USERHLQ.SMPSTS)
  .
  ADD DDDEF(SMPMTS) SHR DA(USERHLQ.SMPMTS)
  .
  ADD DDDEF(SMPSCDS) SHR DA(USERHLQ.SMPSCDS)
  .
  ADD DDDEF(SMPTLIB)
   UNIT(SYSDA)
  .
  ADD DDDEF(SMPOUT)
   SYSOUT(*)
  .
  ADD DDDEF(SYSPRINT)
   SYSOUT(*)
  .
  ADD DDDEF(SMPLIST)
   SYSOUT(*)
  .
  ADD DDDEF(SMPRPT)
   SYSOUT(*)
  .
  ADD DDDEF(SMPSNAP)
   SYSOUT(*)
  .
  ADD DDDEF(SMPDEBUG)
   SYSOUT(*)
  .
  ADD DDDEF(SYSUT1) BLK(6160) SPACE(200,100)
   UNIT(SYSDA)
  .
  ADD DDDEF(SYSUT2) BLK(6160) SPACE(200,100)
   UNIT(SYSDA)
  .
  ADD DDDEF(SYSUT3) BLK(6160) SPACE(200,100)
   UNIT(SYSDA)
  .
  ADD DDDEF(SYSUT4) BLK(6160) SPACE(200,100)
   UNIT(SYSDA)
  .
  ADD DDDEF(SYSPUNCH) BLK(6160) SPACE(200,100) DIR(91)
   UNIT(SYSDA)
  .
  ENDUCL.
//*
```