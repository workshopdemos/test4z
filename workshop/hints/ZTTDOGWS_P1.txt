       PROCESS PGMN(LM),NODYNAM
       IDENTIFICATION DIVISION.
       PROGRAM-ID. 'ZTTDOGWS' RECURSIVE.
       DATA DIVISION.
       WORKING-STORAGE SECTION.

      * Copy in API control blocks from Test4z.
       COPY ZTESTWS.

      * Copy return value control block for ZWS_LoadData API.
      * It loads data from previous "live" recording.
       1 LOAD_Data.
         COPY ZDATA.

      * Copy return value control block for ZWS_MockQSAM API.
      * Mocks ADOPTS reads from DD with previous "live" recording.
       1 MOCK_ADOPTS.
         COPY ZQSAM.

      * Copy return value control block for ZWS_MockQSAM API.
      * Mocks OUTREP writes to DD.
       1 MOCK_OUTREP.
         COPY ZQSAM.

      * Copy return value control block for ZWS_SpyQSAM API.
      * Spies on OUTREP file modifications (which is also mocked).
       1 OUTREP_SPY.
         COPY ZSPQSAM.

      * Count of WRITE commands from ZWS_SpyQSAM callback for OUTREP.
       1 OUTREP_SPY_WRITE_COUNT PIC 9(3) VALUE ZERO.

       LINKAGE SECTION.

      * Copy in required control blocks from Test4z.
       COPY ZTESTLS.

      * Incoming parameter from ZWS_SpyQSAM callback.
       1 SPY_CALLBACK_OUTREP.
         COPY ZSPQSAM.

      * Reference to ZTPDOGOS accumulator retrived via ZWS_GetVariable.
       1 ZTPDOGOS-ACCUMULATOR.
           5 BREED-ADOPTIONS PIC 9(3) OCCURS 9 TIMES.

      * Reference to ZTPDOGOS record from ZWS_SpyQSAM callback.
      * (ADOPTED-REPORT-REC)
           COPY ZTPDGARR.

       PROCEDURE DIVISION.

      ******************************************************************
      * Register test to be run (only one in this simple example).
      ******************************************************************
           move low-values to I_Test
           set testFunction in ZWS_Test to entry 'outrepTotalsUnitTest'
           move 'ZTTDOGWS simple totals test' to testName in ZWS_Test
           call ZTESTUT using ZWS_Test

      * Note: Test4z will call 'outrepTotalsUnitTest' after the SUT
      * is prepared (see runZTPDOGOS for more details).

           goback.

      ******************************************************************
      * Unit test called by Test4z test suite runner.
      ******************************************************************
           entry 'outrepTotalsUnitTest'

           perform mockADOPTSFile
           perform mockOUTREPFile
           perform registerOUTREPFileSpy
           perform runZTPDOGOS

           goback.

      ******************************************************************
      * QSAM spy callback for OUTREP file.
      ******************************************************************
           entry 'spyCallbackOUTREP' using SPY_CALLBACK_OUTREP.

           display 'See workshop step 3.1 (#SPYCALLBACK)'

      * Map the linkage section to the address of the last record.
      *
      * (NB: The SPY_CALLBACK_OUTREP field 'calls' has all middleware
      * calls recorded so far, but we're only interested in the
      * last one; that's recorded in the field 'lastCall').

           set address of ZLS_QSAM_Record to
                lastCall in SPY_CALLBACK_OUTREP

      * For a simple validation, we're only interested in valid WRITEs.

           if command in ZLS_QSAM_Record = 'WRITE' and
                     statusCode in ZLS_QSAM_Record = '00'

                display 'See workshop step 3.2 (#SPYWRITE)'

      * Write the output record to SYSOUT for unit test debugging.
                set address of ADOPTED-REPORT-REC
                   to ptr in record_ in ZLS_QSAM_Record
                display ADOPTED-REPORT-REC

           end-if

      * When the file is closed, verify ZTPDOGOS' internal acculator.

           if command in ZLS_QSAM_RECORD = 'CLOSE'
                display 'See workshop step 3.3 (#SPYCLOSE)'

                perform validateResults
           end-if

           goback.

      ******************************************************************
      * Mock the OUTREP QSAM output file (no need to load data for it).
      ******************************************************************
       mockOUTREPFile.

           display 'See workshop step 1.1 and step 1.2 (#MOCKOUTREP)'

           exit.

      ******************************************************************
      * Load data for ADOPTS file from previous recording and mock it.
      ******************************************************************
       mockADOPTSFile.

      * Load data from a previous "live" recording.

           display 'See workshop step 1.3 (#LOADRECORDED)'

      * Initialize QSAM file access mock object for the ADOPTS DD
      * with the load object (data) created above.

           display 'See workshop step 1.4 (#MOCKADOPTS)'

           exit.

      ******************************************************************
      * Register a QSAM spy callback for changes in the OUTREP file.
      ******************************************************************
       registerOUTREPFileSpy.

           display 'See workshop step 2.1 and step 2.2 (#REGISTERSPY)'

           exit.

      ******************************************************************
      * Run the ZTPDOGOS program.
      ******************************************************************
       runZTPDOGOS.

           move low-values to I_RunFunction
           move 'ZTPDOGOS' to moduleName in ZWS_RunFunction
           call ZTESTUT using ZWS_RunFunction.

           exit.

      ******************************************************************
      * Validate OUTREP writes and ZTPDOGOS internal acculator values.
      ******************************************************************
       validateResults.

      * Black box test - confirm the correct number of OUTREP records.

           display 'See workshop step 3.3 (#VALIDATEOUTREP)'

           if OUTREP_SPY_WRITE_COUNT not = 9 then
             perform failOutrepWriteCount
           end-if

      * Gray box test - get access to ZTPDOGOS' internal accumulator.

           display 'See workshop step 3.4 (#VALIDATEACCUMULATOR)'

           move low-values to I_GetVariable
           move 'ACCUMULATOR' to variableName in ZWS_GetVariable
           call ZTESTUT using ZWS_GetVariable,
                address of ZTPDOGOS-ACCUMULATOR

      * Check accumulator values, assert fail if an incorrect total.

           if BREED-ADOPTIONS(1) not = 8 or
                     BREED-ADOPTIONS(2) not = 0 or
                     BREED-ADOPTIONS(3) not = 7 or
                     BREED-ADOPTIONS(4) not = 1 or
                     BREED-ADOPTIONS(5) not = 0 or
                     BREED-ADOPTIONS(6) not = 0 or
                     BREED-ADOPTIONS(7) not = 0 or
                     BREED-ADOPTIONS(8) not = 6 or
                     BREED-ADOPTIONS(9) not = 0 then
                perform failInternalAccumulator
           end-if

           exit.

      ******************************************************************
      * Signal failure checking the internal ZTPDOGOS accumulator.
      ******************************************************************
       failInternalAccumulator.

           display 'Invalid accumulator value(s) ' ZTPDOGOS-ACCUMULATOR
           move low-values to I_Assert in ZWS_Assert
           move 'Invalid accumulator value(s) from ZTPDOGOS'
                to failMessage in ZWS_Assert
           call ZTESTUT using ZWS_Assert.

           exit.

      ******************************************************************
      * Signal failure of expected count of OUTREP records.
      ******************************************************************
       failOutrepWriteCount.

           display 'OUTREP record count is ' OUTREP_SPY_WRITE_COUNT.
           move low-values to I_Assert in ZWS_Assert
           move 'Invalid OUTREP count from ZTTDOGWS'
                to failMessage in ZWS_Assert
           call ZTESTUT using ZWS_Assert.

           exit.

       END PROGRAM 'ZTTDOGWS'.