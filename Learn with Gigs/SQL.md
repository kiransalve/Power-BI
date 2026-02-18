```
17-Feb-26
# Easy

1.
Show first name of patients that start with the letter 'C'

select first_name 
from patients
where first_name like "C%";

2.
Show first name and last name of patients that weight
within the range of 100 to 120 (inclusive)

select first_name, last_name
from patients
where weight between 100 and 120;

3.
Update the patients table for the allergies column.
If the patient's allergies is null then replace it with 'NKA'

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
Show all the columns from admissions where the patient was admitted and
discharged on the same day.

select *
from admissions
where admission_date = discharge_date;

10.
Show the patient id and the total number of admissions for patient_id 579.

select patient_id, count(*)
from admissions
where patient_id = 579;

11.
Based on the cities that our patients live in, show unique cities
that are in province_id 'NS'.

select distinct city from patients
where province_id = "NS";

12.
Write a query to find the first_name, last name and birth date of patients
who has height greater than 160 and weight greater than 70

select first_name, last_name, birth_date
from patients
where height > 160
and weight > 70;

13.
Write a query to find list of patients first_name, last_name, and allergies
where allergies are not null and are from the city of 'Hamilton'

select first_name, last_name, allergies
from patients
where allergies not null and city = "Hamilton";

# Medium

14.
Show unique birth years from patients and order them by ascending.

select distinct year(birth_date) as yr from patients
order by yr;

15.
Show unique first names from the patients table which only occurs once in the list.
For example, if two or more people are named 'John' in the first_name column
then don't include their name in the output list. If only 1 person is named 'Leo'
then include them in the output.

select first_name
from patients
group by first_name
having count(*) = 1;

16.
Show patient_id and first_name from patients where their first_name
start and ends with 's' and is at least 6 characters long.

SELECT patient_id, first_name
FROM patients
WHERE first_name LIKE 's%'
  AND first_name LIKE '%s'
  AND LEN(first_name) > 5;

17.
Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.

select first_name
from patients
order by len(first_name), first_name;

18.
Show the total amount of male patients and the total amount of female
patients in the patients table.
Display the two results in the same row.

select 
	sum(case when gender = "M" then 1 else 0 end) as male_count,
    sum(case when gender = "F" then 1 else 0 end) as female_count
from patients;

19.
Show first and last name, allergies from patients which have allergies to
either 'Penicillin' or 'Morphine'.
Show results ordered ascending by allergies then by first_name then by last_name.

SELECT first_name, last_name, allergies
FROM patients
WHERE allergies IN ('Penicillin', 'Morphine')
ORDER BY allergies, first_name, last_name;

20.
Show patient_id, diagnosis from admissions. Find patients admitted
multiple times for the same diagnosis.

SELECT patient_id, diagnosis
FROM admissions
GROUP BY patient_id, diagnosis
HAVING COUNT(*) > 1;

21.
Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.

select city, count(*)
from patients
group by city
order by count(*) desc, city asc;

22.
Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"

select first_name, last_name, "Patient" as role
from patients

union all

select first_name, last_name, "Doctor" as role
from doctors;

23.
Show all allergies ordered by popularity. Remove NULL values from query.

select allergies, count(*)
from patients 
where allergies is not null
group by allergies
order by count(*) desc;


24.
Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade.
Sort the list starting from the earliest birth_date.

Sol - 1
select first_name, last_name, birth_date
from patients
where year(birth_date) > 1969 and year(birth_date) < 1980
order by birth_date;

Sol - 2
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE
  YEAR(birth_date) BETWEEN 1970 AND 1979
ORDER BY birth_date ASC;

Sol - 3
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE
  birth_date >= '1970-01-01'
  AND birth_date < '1980-01-01'
ORDER BY birth_date ASC

Sol - 4
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE year(birth_date) LIKE '197%'
ORDER BY birth_date ASC

25.
We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first,
then first_name in all lower case letters. Separate the last_name and first_name with a comma.
Order the list by the first_name in decending order
EX: SMITH,jane

select upper(last_name) || "," || lower(first_name)
from patients
order by first_name desc

SELECT
  CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS new_name_format
FROM patients
ORDER BY first_name DESC;

26.
Show the province_id(s), sum of height; where the total sum of its patient's
height is greater than or equal to 7,000.

select province_id, sum(height)
from patients
group by province_id
having sum(height) >= 7000;

27.
Show the difference between the largest weight and smallest weight for
patients with the last name 'Maroni'

select max(weight) - min(weight)
from patients
where last_name = "Maroni";

28.
Show all of the days of the month (1-31) and how many admission_dates occurred on that day.
Sort by the day with most admissions to least admissions.

select day(admission_date), count(*)
from admissions
group by day(admission_date)
order by count(*) desc;

29.
Show all columns for patient_id 542's most recent admission_date.

select * from admissions
where patient_id = 542
order by admission_date desc
limit 1;

SELECT *
FROM admissions
GROUP BY patient_id
HAVING
  patient_id = 542
  AND max(admission_date)

18-Feb-2026
30.
Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

select patient_id, attending_doctor_id, diagnosis
from admissions
where 
(patient_id % 2 <> 0 
and attending_doctor_id in (1,5,19)
) 
or
(attending_doctor_id like "%2%"
and len(patient_id) = 3
);

31.
For each doctor, display their id, full name, and the first and last admission date they attended.

select d.doctor_id, concat(d.first_name," ", d.last_name),
min(a.admission_date),
max(a.admission_date)
from admissions a
join doctors d on a.attending_doctor_id = d.doctor_id
group by d.doctor_id, d.first_name, d.last_name;

32.
Display the total amount of patients for each province. Order by descending.

select pr.province_name, count(p.patient_id)
from patients p
join province_names pr 
on p.province_id = pr.province_id
group by pr.province_name
order by count(p.patient_id) desc;













```
