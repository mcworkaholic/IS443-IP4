--IS443/543 Fall 2022
--Weston Evans

--Create a procedure similar to DROP IF EXISTS

CREATE OR REPLACE PROCEDURE DelObject(ObjName varchar2,ObjType varchar2)
IS
 v_counter number := 0;   
begin    
  if ObjType = 'TABLE' then
    select count(*) into v_counter from user_tables where table_name = upper(ObjName);
    if v_counter > 0 then          
      execute immediate 'drop table ' || ObjName || ' cascade constraints';        
    end if;   
  end if;
  if ObjType = 'PROCEDURE' then
    select count(*) into v_counter from User_Objects where object_type = 'PROCEDURE' and OBJECT_NAME = upper(ObjName);
      if v_counter > 0 then          
        execute immediate 'DROP PROCEDURE ' || ObjName;        
      end if; 
  end if;
  if ObjType = 'FUNCTION' then
    select count(*) into v_counter from User_Objects where object_type = 'FUNCTION' and OBJECT_NAME = upper(ObjName);
      if v_counter > 0 then          
        execute immediate 'DROP FUNCTION ' || ObjName;        
      end if; 
  end if;
  if ObjType = 'TRIGGER' then
    select count(*) into v_counter from User_Triggers where TRIGGER_NAME = upper(ObjName);
      if v_counter > 0 then          
        execute immediate 'DROP TRIGGER ' || ObjName;
      end if; 
  end if;
  if ObjType = 'VIEW' then
    select count(*) into v_counter from User_Views where VIEW_NAME = upper(ObjName);
      if v_counter > 0 then          
        execute immediate 'DROP VIEW ' || ObjName;        
      end if; 
  end if;
  if ObjType = 'SEQUENCE' then
    select count(*) into v_counter from user_sequences where sequence_name = upper(ObjName);
      if v_counter > 0 then          
        execute immediate 'DROP SEQUENCE ' || ObjName;        
      end if; 
  end if;
end;
/

-- DROP statements / Execute procedure DelObject for each obj in database
EXECUTE DelObject ('STUDENT','TABLE'); 
EXECUTE DelObject ('COURSE','TABLE'); 
EXECUTE DelObject ('FACULTY','TABLE'); 
EXECUTE DelObject ('OFFERING','TABLE'); 
EXECUTE DelObject ('ENROLLMENT','TABLE'); 



--Create Student table
CREATE TABLE STUDENT(

StdID Number(3) CONSTRAINT Student_ID_PK PRIMARY KEY,

StdFN VARCHAR2(15),

StdLN VARCHAR2(15),

StdCity VARCHAR2(15),

StdState CHAR(2),

StdZip NUMBER(5),

StdMajor VARCHAR2(5),

StdClass CHAR(2),

StdGPA Number(3,2) CONSTRAINT Student_GPA_cc CHECK (StdGPA< = 4.0

AND StdGPA >= 0) );

-- Insert records into STUDENT
--Row 1
INSERT INTO STUDENT (STDID, STDFN, STDLN, STDCITY, STDSTATE, STDZIP, STDMAJOR, STDCLASS, STDGPA) VALUES (101,'Joe','Smith','Eau Claire','WI',18121,'IS','FR',3.0);
--Row 2
INSERT INTO STUDENT (STDID, STDFN, STDLN, STDCITY, STDSTATE, STDZIP, STDMAJOR, STDCLASS, STDGPA) VALUES (102,'Jenny','Sneider','Eau Claire','WI',98011,'IS','JR',3.2);
--Row 3
INSERT INTO STUDENT (STDID, STDFN, STDLN, STDCITY, STDSTATE, STDZIP, STDMAJOR, STDCLASS, STDGPA) VALUES (103,'Dan','Robinson','Sartell','MN',98042,'IS','JR',3.5);
--Row 4
INSERT INTO STUDENT (STDID, STDFN, STDLN, STDCITY, STDSTATE, STDZIP, STDMAJOR, STDCLASS, STDGPA) VALUES (104,'Sue','Williams','St. Cloud','MN',56301,'ACCT','SR',3.2);

--Create Faculty table
CREATE TABLE FACULTY(
FacID Number(4) CONSTRAINT Faculty_ID_PK PRIMARY KEY,

FacFN VARCHAR2(15),

FacLN VARCHAR2(15),

FacDept VARCHAR2(5),

FacRank VARCHAR2(4),

FacHireDate DATE,

FacSalary VARCHAR2(6),

FacSupervisor NUMBER(4));

--Insert Records into FACULTY
--Row 1
INSERT INTO FACULTY (FACID, FACFN, FACLN, FACDEPT, FACRANK, FACHIREDATE, FACSALARY, FACSUPERVISOR) VALUES (9001,'Leonard','Vince','IS','ASST',to_date('12-Apr-2007', 'DD-MON-RR'),'117000',9003);
--Row 2
INSERT INTO FACULTY (FACID, FACFN, FACLN, FACDEPT, FACRANK, FACHIREDATE, FACSALARY, FACSUPERVISOR) VALUES (9002,'Victor','Fibon','IS','ASSO',to_date('8-Aug-2009', 'DD-MON-RR'),'130000',9003);
--Row 3
INSERT INTO FACULTY (FACID, FACFN, FACLN, FACDEPT, FACRANK, FACHIREDATE, FACSALARY, FACSUPERVISOR) VALUES (9003,'Nicki','Colan','IS','PROF',to_date('20-Aug-1991', 'DD-MON-RR'),'145000',9010);
--Row 4
INSERT INTO FACULTY (FACID, FACFN, FACLN, FACDEPT, FACRANK, FACHIREDATE, FACSALARY, FACSUPERVISOR) VALUES (9004,'Fred','Wells','ACCT','ASST',to_date('28-Aug-2006', 'DD-MON-RR'),'106000',9010);
--Row 5
INSERT INTO FACULTY (FACID, FACFN, FACLN, FACDEPT, FACRANK, FACHIREDATE, FACSALARY, FACSUPERVISOR) VALUES (9010,'Chris','Macon','ACCT','ASST',to_date('4-Aug-1990', 'DD-MON-RR'),'127900',NULL);

--Create Course table
CREATE TABLE COURSE(

CourseNo Varchar2(8) CONSTRAINT Course_No_PK PRIMARY KEY,

CrsDesc Varchar2(30),

CrsCredits NUMBER(1));

--Insert Records into COURSE
--Row 1
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 250','Application Program Dev. I',3);
--Row 2
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 345','Application Program Dev. II',3);
--Row 3
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 445','Application Program Dev. III',3);
--Row 4
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 356','System Analysis and Design',3);
--Row 5
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 451','IT Infrastructure',3);
--Row 6
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('ACCT 291','Accounting Principles II',3);
--Row 7
INSERT INTO COURSE (COURSENO, CRSDESC, CRSCREDITS) VALUES ('IS 443','Database Design',3);

--Create Offering table
CREATE TABLE OFFERING(

OfferNo NUMBER(4) CONSTRAINT Offer_No_PK PRIMARY KEY,

CourseNo VARCHAR2(8)CONSTRAINT Course_No_FK REFERENCES COURSE(CourseNo),

OffTerm VARCHAR2(6),

OffYear NUMBER(4),

OffLoca VARCHAR2(5),

OffTime VARCHAR2(7),

OffDay VARCHAR2(6),

FacSSN NUMBER(4) CONSTRAINT Offering_FK REFERENCES FACULTY(FacID));

--Insert Records into OFFERING
--Row 1
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (2201,'IS 250','Spring',2020,'BB260','10:30am','MWF',9002);
--Row 2
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (2202,'IS 250','Spring',2019,'BB118','8:00am','TTH',9002);
--Row 3
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (2203,'IS 356','Fall',2021,'BB260','9:30am','TTH',9001);
--Row 4
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (2204,'IS 451','Fall',2021,'BB315','12:30pm','TTH',9003);
--Row 5
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (1101,'ACCT 291','Fall',2020,'BB320','12:30pm','MWF',9010);
--Row 6
INSERT INTO OFFERING (OFFERNO, COURSENO, OFFTERM, OFFYEAR, OFFLOCA, OFFTIME, OFFDAY, FACSSN) VALUES (2205,'IS 443','Fall',2020,'BB216','12:30pm','MWF',9003);


--Create Enrollment table
CREATE TABLE ENROLLMENT(

StdID Number(3)CONSTRAINT Enroll_StdID_FK References STUDENT(StdID),

OfferNo NUMBER(4)CONSTRAINT Enroll_OfferNo_FK References OFFERING(OfferNo),

EnrGrade CHAR(1) CONSTRAINT Enroll_grade_cc CHECK (EnrGrade IN('A','B','C','D','F')));

ALTER TABLE ENROLLMENT

ADD CONSTRAINT Enroll_PK PRIMARY KEY (StdID, OfferNo);

--Insert Records into ENROLLMENT
--Row 1
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (101,2201,'A');
--Row 2
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (101,2203,'B');
--Row 3
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (102,2203,'C');
--Row 4
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (103,2203,'B');
--Row 5
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (103,2201,'C');
--Row 6
INSERT INTO ENROLLMENT (STDID, OFFERNO, ENRGRADE) VALUES (103,1101,'B');

---Q1
SELECT FacFN, FacLN, FacSalary, FacHireDate FROM FACULTY 
WHERE FacHireDate > to_date('31-Dec-1995');

---Q2 
SELECT FacFN, FacLN, FacHireDate 
FROM FACULTY 
WHERE FacHireDate BETWEEN to_date('1-Jan-2006') AND to_date('31-Dec-2007');

---Q3
SELECT CourseNo FROM COURSE
WHERE CourseNo LIKE ('IS 3%%');

--Q4
SELECT OfferNO, CourseNo, FacFN, FacLN
FROM OFFERING
INNER JOIN FACULTY
ON FacSSN = FacID WHERE FacRank = 'ASST' AND CourseNo LIKE('IS%%%%') AND OffTerm = 'Fall' AND OffYear = '2021';

--Q5
SELECT StdMajor, 
AVG(StdGPA) AS AVG_GPA
FROM STUDENT
WHERE StdClass IN ('JR', 'SR') 
GROUP BY StdMajor;

--Q6
SELECT StdMajor, 
AVG(StdGPA) AS AVG_GPA
FROM STUDENT
WHERE StdClass IN ('JR', 'SR') 
GROUP BY StdMajor
HAVING AVG(StdGPA) > 3.25;

--Q7
SELECT StdMajor, StdClass, MIN(StdGPA) AS MIN_GPA, MAX(StdGPA) AS MAX_GPA
FROM STUDENT 
GROUP BY StdMajor, StdClass
ORDER BY StdMajor;

--Q8
SELECT COURSE.CrsDesc, COUNT(OFFERING.CourseNo) AS COURSE_OFFERINGS
FROM OFFERING
INNER JOIN COURSE
ON OFFERING.CourseNo = COURSE.CourseNo WHERE OFFERING.CourseNo LIKE('IS%%%%') AND COURSE.CourseNo LIKE('IS%%%%') 
GROUP BY offering.courseno, course.crsdesc
HAVING COUNT(OFFERING.CourseNo) > 0;

--Q9
SELECT OFFERING.CourseNo, COURSE.CrsDesc
FROM ENROllMENT
INNER JOIN OFFERING
ON ENROLLMENT.OfferNo = OFFERING.OfferNo INNER JOIN COURSE
ON OFFERING.CourseNo = Course.CourseNo
WHERE StdID = 103; 

--Q10
SELECT ENROLLMENT.StdID, StdFN, StdLN, COUNT(ENROLLMENT.StdID) AS NUM_COURSES
FROM STUDENT
INNER JOIN ENROLLMENT
ON STUDENT.StdID = ENROLLMENT.StdID GROUP BY ENROLLMENT.StdID, StdFN, StdLN 
UNION
SELECT STUDENT.StdID, STUDENT.StdFN, STUDENT.StdLN, COUNT(ENROLLMENT.StdID) AS NUM_COURSES
FROM STUDENT
LEFT OUTER JOIN ENROLLMENT
ON STUDENT.StdID = ENROLLMENT.StdID
WHERE ENROLLMENT.StdID IS NULL GROUP BY STUDENT.StdID, STUDENT.StdFN, STUDENT.StdLN;
