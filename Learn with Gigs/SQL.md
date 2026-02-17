```
1.
Show first name of patients that start with the letter 'C'

select first_name 
from patients
where first_name like "C%";

2.
Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

select first_name, last_name
from patients
where weight between 100 and 120;

3.
Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

update patients
set allergies = "NKA"
where allergies is null;

4.
Show first name and last name concatinated into one column to show their full name.

select first_name || " " || last_name from patients

5.
Show how many patients have a birth_date with 2010 as the birth year.

select count(*) 
from patients
where year(birth_date) = 2010

6.
Show the first_name, last_name, and height of the patient with the greatest height.

Option - 1
select first_name, last_name, height
from patients
order by height desc
limit 1;

Option - 2
select first_name, last_name, height
from patients
where height = (select max(height) from patients);

7.
Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000

select * from patients
where patient_id in (1,45,534,879,1000);

8.
Show the total number of admissions

select count(*)
from admissions

9.
Show all the columns from admissions where the patient was admitted and discharged on the same day.

select *
from admissions
where admission_date = discharge_date;

10.
Show the patient id and the total number of admissions for patient_id 579.

select patient_id, count(*)
from admissions
where patient_id = 579;

11.
Based on the cities that our patients live in, show unique cities that are in province_id 'NS'.

select distinct city from patients
where province_id = "NS";

12.
Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70

select first_name, last_name, birth_date
from patients
where height > 160
and weight > 70;











```
