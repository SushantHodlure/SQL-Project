# SQL-Project 
**Hospital End to End Project

drop table if exists hospitaL_data;
Create table hospitaL_data(Hospital_Name varchar(100),
                           Location varchar(100),
						   Department varchar(100),
						   Doctors_Count int,
						   Patients_Count int,
						   Admission_Date date,
						   Discharge_Date date,
						   Medical_Expenses numeric (10,0)
);

select * from hospital_data;

-- Import Data into hospital_data Table
COPY Hospital_Data(hospital_name,location,department,doctors_count,patients_count,admission_date,discharge_date,medical_expenses) 
FROM 'C:\Users\Dell\Downloads\Hospital_Data.csv'
DELIMITER ','
CSV HEADER;

-- 1. Total Number of Patients.
-- Write an SQL query to find the total number of patients across all hospitals.

SELECT
	SUM(PATIENTS_COUNT) AS TOTAL_PATIENTS
FROM
	HOSPITAL_DATA;
	
-- 2. Average Number of Doctors per Hospital
--  Retrieve the average count of doctors available in each hospital.
SELECT
	HOSPITAL_NAME,
	AVG(DOCTORS_COUNT) AS AVERAGE_DOCTORS
FROM
	HOSPITAL_DATA
GROUP BY
	HOSPITAL_NAME;

-- 3. Top 3 Departments with the Highest Number of Patients
-- Find the top 3 hospital departments that have the highest number of patients.

SELECT
	HOSPITAL_NAME,
	DEPARTMENT,
	PATIENTS_COUNT
FROM
	HOSPITAL_DATA	
ORDER BY
	PATIENTS_COUNT DESC limit  3 ;
	
-- 4. Hospital with the Maximum Medical Expenses
-- Identify the hospital that recorded the highest medical expenses.
SELECT
	HOSPITAL_NAME,
	MEDICAL_EXPENSES
FROM
	HOSPITAL_DATA
ORDER BY
	MEDICAL_EXPENSES DESC
LIMIT 1;

-- 5. Daily Average Medical Expenses
-- Calculate the average medical expenses per day for each hospital.

SELECT
	HOSPITAL_NAME, 
	AVG(MEDICAL_EXPENSES/(Discharge_Date::DATE - Admission_Date::DATE)+1) as avg_medical_expenses_per_day
FROM
	HOSPITAL_DATA
GROUP BY
	HOSPITAL_NAME;



	
	
-- 6.Longest Hospital Stay
-- Find the patient with the longest stay by calculating the difference between Discharge Date and Admission Date.


SELECT 
    Hospital_name,
	PATIENTS_COUNT,
    Admission_Date, 
    Discharge_Date, 
    (Discharge_Date::DATE - Admission_Date::DATE) AS Length_of_Stay
FROM hospital_data
ORDER BY Length_of_Stay DESC
LIMIT 1 ;


-- 7. Total Patients Treated Per City
-- Count the total number of patients treated in each city.
SELECT
	LOCATION AS CITY,
	SUM(PATIENTS_COUNT) AS TOTAL_PATIENTS_TREATED
FROM
	HOSPITAL_DATA
GROUP BY
	CITY
ORDER BY
	TOTAL_PATIENTS_TREATED DESC;

--8. Average Length of Stay Per Department
-- Calculate the average number of days patients spend in each department.	

SELECT
	DEPARTMENT, 
	AVG(patients_Count/(DISCHARGE_DATE::DATE - ADMISSION_DATE::DATE)) AS AVG_LENGTH_OF_STAY
FROM
	HOSPITAL_DATA
GROUP BY
	DEPARTMENT
ORDER BY
	AVG_LENGTH_OF_STAY DESC;
	
-- 9. Identify the Department with the Lowest Number of Patients
-- Find the department with the lowest number of patients.	


SELECT
	DEPARTMENT,
	SUM(PATIENTS_COUNT) AS TOTAL_PATIENTS
FROM
	HOSPITAL_DATA
GROUP BY
	DEPARTMENT
ORDER BY
	TOTAL_PATIENTS ASC
LIMIT
	1;

select * from hospital_data; 

-- 10. Monthly Medical Expenses Report
-- Group the data by month and calculate the total medical expenses for each month.
SELECT
	TO_CHAR(ADMISSION_DATE::DATE, 'YYYY-MM') AS MONTH,
	SUM(MEDICAL_EXPENSES) AS MONTHLY_MEDICAL_EXPENSES
FROM
	HOSPITAL_DATA
GROUP BY
	MONTH
ORDER BY
	MONTH DESC;
