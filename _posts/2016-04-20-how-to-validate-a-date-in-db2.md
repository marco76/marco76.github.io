---
id: 626
title: How to validate a Date in DB2
date: 2016-04-20T10:23:48+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=626
permalink: /2016/04/20/how-to-validate-a-date-in-db2/
categories:
  - SQL Server
tags:
  - db2
  - sql
---
This function verify if a date is valid in DB2 according to a custom date format.

<pre class="brush: sql; title: ; notranslate" title="">-- this function return 1 if the varchar date and 0 if the date is not valid
-- the format of the date is passed in the second parameter (ex. mm/dd/yyyy)

CREATE OR REPLACE FUNCTION DB.FUNC_IS_VALID_DATE(dateToValidate VARCHAR(20), dateFormat VARCHAR(20)) RETURNS INTEGER

  BEGIN
     DECLARE functionResult INT;
     DECLARE resultingDate DATE;
     DECLARE SQLCODE INTEGER;

     DECLARE CONTINUE HANDLER FOR SQLEXCEPTION BEGIN
         RETURN 0; -- the data is not valid
     END; -- END DECLARE

     SELECT to_date(dateToValidate, dateFormat) INTO resultingDate FROM SYSIBM.SYSDUMMY1;
     RETURN 1; -- the data is valid
  END;
</pre>