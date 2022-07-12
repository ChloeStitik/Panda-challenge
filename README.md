# Panda-challenge
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
District Summary
Calculate the total number of schools

Calculate the total number of students

Calculate the total budget

Calculate the average math score

Calculate the average reading score

Calculate the percentage of students with a passing math score (70 or greater)

Calculate the percentage of students with a passing reading score (70 or greater)

Calculate the percentage of students who passed math and reading (% Overall Passing)

Create a dataframe to hold the above results

Optional: give the displayed data cleaner formatting

#calculate the Totals(Schools and Students)
school_count = len(school_data_complete["school_name"].unique())
student_count = school_data_complete["Student ID"].count()

#calculate the Total Budget
total_budget = school_data["budget"].sum()
#calculate the Average Scores
average_math_score = school_data_complete["math_score"].mean()
average_reading_score = school_data_complete["reading_score"].mean()
#calculate the Percentage Pass Rate
#first, math
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]
passing_math_percentage = (passing_math_count / float(student_count)) * 100 #(74.98%)
#second reading
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]
passing_reading_percentage = (passing_reading_count / float(student_count)) * 100 #(85.81%)
#third, pass reading and math
passing_math_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70) &
                                                 (school_data_complete["math_score"] >= 70)].count()["student_name"] #(25,528)
passing_math_reading_percentage = (passing_math_reading_count / float(student_count)) * 100 #(65.17%)

passing_math_reading_percentage
65.17232575950983
#Mirror Data Cleanup
district_summary = pd.DataFrame({
    "Total Schools": [school_count],
    "Total Students": [student_count],
    "Total Budget": [total_budget],
    "Average Math Score": [average_math_score],
    "Average Reading Score": [average_reading_score],
    "% Passing Math": [passing_math_percentage],
    "% Passing Reading": [passing_reading_percentage],
    "% Overall Passing": [passing_math_reading_percentage]
})

#Formatting
district_summary["Total Students"] = district_summary["Total Students"].map("{:,}".format)
district_summary["Total School Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)
district_summary["Average Math Score"] = district_summary["Average Math Score"].map("{:,.2f}".format)
district_summary["Average Reading Score"] = district_summary["Average Reading Score"].map("{:,.2f}%".format)
district_summary["% Passing Math"] = district_summary["% Passing Math"].map("{:,.2f}%".format)
district_summary["% Passing Reading"] = district_summary["% Passing Reading"].map("{:,.2f}%".format)
district_summary["% Overall Passing"] = district_summary["% Overall Passing"].map("{:,.2f}%".format)

#Display the DataFrame
district_summary
Total Schools	Total Students	Total Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Total School Budget
0	15	39,170	24649428	78.99	81.88%	74.98%	85.81%	65.17%	$24,649,428.00
School Summary
Create an overview table that summarizes key metrics about each school, including:

School Name
School Type
Total Students
Total School Budget
Per Student Budget
Average Math Score
Average Reading Score
% Passing Math
% Passing Reading
% Overall Passing (The percentage of students that passed math and reading.)
Create a dataframe to hold the above results

#Determine the School Type
school_types = school_data.set_index(["school_name"])["type"]
#school_types.head(3)

#Calculate the total student count
per_school_counts = school_data_complete["school_name"].value_counts()
#per_school_counts

#Calculate the total school budget and per capita spending
per_school_budget = school_data_complete.groupby(["school_name"]).mean()["budget"]
per_school_capita = per_school_budget / per_school_counts
#per_school_capita
#per_school_budget

#Calculate the averag test scores
per_school_math = school_data_complete.groupby(["school_name"]).mean()["math_score"]
#per_school_math
per_school_reading = school_data_complete.groupby(["school_name"]).mean()["reading_score"]
#per_school_reading
#Get the students who passed math and passed reading by creating separate filtered DataFrames.
school_passing_math = school_data_complete[(school_data_complete["math_score"] >= 70)]
#school_passing_math.head()
school_passing_reading = school_data_complete[(school_data_complete["reading_score"] >= 70)]
school_passing_reading.head()

#Get the students who passed both reading and math in a separate DataFrame.
passing_math_and_reading = school_data_complete[(school_data_complete["math_score"] >= 70) &
                                                (school_data_complete["reading_score"] >= 70)]
passing_math_and_reading.head()
Student ID	student_name	gender	grade	school_name	reading_score	math_score	School ID	type	size	budget
4	4	Bonnie Ray	F	9th	Huang High School	97	84	0	District	2917	1910635
5	5	Bryan Miranda	M	9th	Huang High School	94	94	0	District	2917	1910635
6	6	Sheena Carter	F	11th	Huang High School	82	80	0	District	2917	1910635
8	8	Michael Roth	M	10th	Huang High School	95	87	0	District	2917	1910635
9	9	Matthew Greene	M	10th	Huang High School	96	84	0	District	2917	1910635
# Calculate the Percentage Pass Rates

#calculate the percentage of students who passed math per school
per_school_passing_math = (school_passing_math.groupby(["school_name"]).count()["student_name"] / per_school_counts) * 100
#per_school_passing_math

#calculate the percentage of students who passed reading per school
per_school_passing_reading = (school_passing_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts) * 100
#per_school_passing_reading

#calculate the percentage of students who passed math and reading per school
per_school_passing_math_reading = (passing_math_and_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts) * 100
per_school_passing_math_reading
Bailey High School       54.642283
Cabrera High School      91.334769
Figueroa High School     53.204476
Ford High School         54.289887
Griffin High School      90.599455
Hernandez High School    53.527508
Holden High School       89.227166
Huang High School        53.513884
Johnson High School      53.539172
Pena High School         90.540541
Rodriguez High School    52.988247
Shelton High School      89.892107
Thomas High School       90.948012
Wilson High School       90.582567
Wright High School       90.333333
dtype: float64
# Convert to DataFrame
per_school_summary = pd.DataFrame({
    "School Type": school_types,
    "Total Students": per_school_counts,
    "Total School Budget": per_school_budget,
    "Per Student Budget": per_school_capita,
    "Average Math Score": per_school_math,
    "Average Reading Score": per_school_reading,
    "% Passing Math": per_school_passing_math,
    "% Passing Reading": per_school_passing_reading,
    "% Overall Passing": per_school_passing_math_reading,
})
#Minor data wrangling
per_school_summary["Total Students"] = per_school_summary["Total Students"].map("{:,}".format)
per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,.2f}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.02f}".format)
per_school_summary["Average Math Score"] = per_school_summary["Average Math Score"].map("{:.2f}".format)
per_school_summary["Average Reading Score"] = per_school_summary["Average Reading Score"].map("{:.2f}".format)
per_school_summary["% Passing Math"] = per_school_summary["% Passing Math"].map("{:.2f}%".format)
per_school_summary["% Passing Reading"] = per_school_summary["% Passing Reading"].map("{:.2f}%".format)
per_school_summary["% Overall Passing"] = per_school_summary["% Overall Passing"].map("{:.2f}%".format)

#Display the DataFrame
per_school_summary
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68%	81.93%	54.64%
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%
Figueroa High School	District	2,949	$1,884,411.00	$639.00	76.71	81.16	65.99%	80.74%	53.20%
Ford High School	District	2,739	$1,763,916.00	$644.00	77.10	80.75	68.31%	79.30%	54.29%
Griffin High School	Charter	1,468	$917,500.00	$625.00	83.35	83.82	93.39%	97.14%	90.60%
Hernandez High School	District	4,635	$3,022,020.00	$652.00	77.29	80.93	66.75%	80.86%	53.53%
Holden High School	Charter	427	$248,087.00	$581.00	83.80	83.81	92.51%	96.25%	89.23%
Huang High School	District	2,917	$1,910,635.00	$655.00	76.63	81.18	65.68%	81.32%	53.51%
Johnson High School	District	4,761	$3,094,650.00	$650.00	77.07	80.97	66.06%	81.22%	53.54%
Pena High School	Charter	962	$585,858.00	$609.00	83.84	84.04	94.59%	95.95%	90.54%
Rodriguez High School	District	3,999	$2,547,363.00	$637.00	76.84	80.74	66.37%	80.22%	52.99%
Shelton High School	Charter	1,761	$1,056,600.00	$600.00	83.36	83.73	93.87%	95.85%	89.89%
Thomas High School	Charter	1,635	$1,043,130.00	$638.00	83.42	83.85	93.27%	97.31%	90.95%
Wilson High School	Charter	2,283	$1,319,574.00	$578.00	83.27	83.99	93.87%	96.54%	90.58%
Wright High School	Charter	1,800	$1,049,400.00	$583.00	83.68	83.95	93.33%	96.61%	90.33%
Top Performing Schools (By % Overall Passing)
Sort and display the top five performing schools by % overall passing.
#Sort and show top five schools
topSchools = per_school_summary.sort_values(["% Overall Passing"], ascending=False)
topSchools.head()
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%
Thomas High School	Charter	1,635	$1,043,130.00	$638.00	83.42	83.85	93.27%	97.31%	90.95%
Griffin High School	Charter	1,468	$917,500.00	$625.00	83.35	83.82	93.39%	97.14%	90.60%
Wilson High School	Charter	2,283	$1,319,574.00	$578.00	83.27	83.99	93.87%	96.54%	90.58%
Pena High School	Charter	962	$585,858.00	$609.00	83.84	84.04	94.59%	95.95%	90.54%
Bottom Performing Schools (By % Overall Passing)
Sort and display the five worst-performing schools by % overall passing.
#Sort and show bottom five schools
bottomSchools = per_school_summary.sort_values(["% Overall Passing"], ascending=True)
bottomSchools.head()
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Rodriguez High School	District	3,999	$2,547,363.00	$637.00	76.84	80.74	66.37%	80.22%	52.99%
Figueroa High School	District	2,949	$1,884,411.00	$639.00	76.71	81.16	65.99%	80.74%	53.20%
Huang High School	District	2,917	$1,910,635.00	$655.00	76.63	81.18	65.68%	81.32%	53.51%
Hernandez High School	District	4,635	$3,022,020.00	$652.00	77.29	80.93	66.75%	80.86%	53.53%
Johnson High School	District	4,761	$3,094,650.00	$650.00	77.07	80.97	66.06%	81.22%	53.54%
Math Scores by Grade
Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

Create a pandas series for each grade. Hint: use a conditional statement.

Group each series by school

Combine the series into a dataframe

Optional: give the displayed data cleaner formatting

#Create data series of scores by grade levels using conditionals (9th, 10th, 11th, and 12th)
ninthGraders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenthGraders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventhGraders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfthGraders = school_data_complete[(school_data_complete["grade"] == "12th")]

#Group each by school name
ninthGraderMathScores = ninthGraders.groupby(["school_name"]).mean()["math_score"]
tenthGraderMathScores = tenthGraders.groupby(["school_name"]).mean()["math_score"]
eleventhGraderMathScores = eleventhGraders.groupby(["school_name"]).mean()["math_score"]
twelfthGraderMathScores = twelfthGraders.groupby(["school_name"]).mean()["math_score"]

#Combine series into single DataFrame
scores_by_grade = pd.DataFrame({
    "9th": ninthGraderMathScores,
    "10th": tenthGraderMathScores,
    "11th": eleventhGraderMathScores,
    "12th": twelfthGraderMathScores
})

#Minor data wrangling 
scores_by_grade["9th"] = scores_by_grade["9th"].map("{:.2f}".format)
scores_by_grade["10th"] = scores_by_grade["10th"].map("{:.2f}".format)
scores_by_grade["11th"] = scores_by_grade["11th"].map("{:.2f}".format)
scores_by_grade["12th"] = scores_by_grade["12th"].map("{:.2f}".format)
scores_by_grade.index.name = None

#Display the DataFrame
scores_by_grade
9th	10th	11th	12th
Bailey High School	77.08	77.00	77.52	76.49
Cabrera High School	83.09	83.15	82.77	83.28
Figueroa High School	76.40	76.54	76.88	77.15
Ford High School	77.36	77.67	76.92	76.18
Griffin High School	82.04	84.23	83.84	83.36
Hernandez High School	77.44	77.34	77.14	77.19
Holden High School	83.79	83.43	85.00	82.86
Huang High School	77.03	75.91	76.45	77.23
Johnson High School	77.19	76.69	77.49	76.86
Pena High School	83.63	83.37	84.33	84.12
Rodriguez High School	76.86	76.61	76.40	77.69
Shelton High School	83.42	82.92	83.38	83.78
Thomas High School	83.59	83.09	83.50	83.50
Wilson High School	83.09	83.72	83.20	83.04
Wright High School	83.26	84.01	83.84	83.64
Reading Score by Grade
Perform the same operations as above for reading scores
#Create data series of scores by grade levels using conditionals (9th, 10th, 11th, and 12th)
ninthGraders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenthGraders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventhGraders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfthGraders = school_data_complete[(school_data_complete["grade"] == "12th")]

#Group each by school name
ninthGraderReadingScores = ninthGraders.groupby(["school_name"]).mean()["reading_score"]
tenthGraderReadingScores = tenthGraders.groupby(["school_name"]).mean()["reading_score"]
eleventhGraderReadingScores = eleventhGraders.groupby(["school_name"]).mean()["reading_score"]
twelfthGraderReadingScores = twelfthGraders.groupby(["school_name"]).mean()["reading_score"]

#Combine series into single DataFrame
scores_by_grade = pd.DataFrame({
    "9th": ninthGraderReadingScores,
    "10th": tenthGraderReadingScores,
    "11th": eleventhGraderReadingScores,
    "12th": twelfthGraderReadingScores
})

#Minor data wrangling 
scores_by_grade["9th"] = scores_by_grade["9th"].map("{:.2f}".format)
scores_by_grade["10th"] = scores_by_grade["10th"].map("{:.2f}".format)
scores_by_grade["11th"] = scores_by_grade["11th"].map("{:.2f}".format)
scores_by_grade["12th"] = scores_by_grade["12th"].map("{:.2f}".format)
scores_by_grade.index.name = None

#Display the DataFrame
scores_by_grade
9th	10th	11th	12th
Bailey High School	81.30	80.91	80.95	80.91
Cabrera High School	83.68	84.25	83.79	84.29
Figueroa High School	81.20	81.41	80.64	81.38
Ford High School	80.63	81.26	80.40	80.66
Griffin High School	83.37	83.71	84.29	84.01
Hernandez High School	80.87	80.66	81.40	80.86
Holden High School	83.68	83.32	83.82	84.70
Huang High School	81.29	81.51	81.42	80.31
Johnson High School	81.26	80.77	80.62	81.23
Pena High School	83.81	83.61	84.34	84.59
Rodriguez High School	80.99	80.63	80.86	80.38
Shelton High School	84.12	83.44	84.37	82.78
Thomas High School	83.73	84.25	83.59	83.83
Wilson High School	83.94	84.02	83.76	84.32
Wright High School	83.83	83.81	84.16	84.07
Scores by School Spending
Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
Average Math Score
Average Reading Score
% Passing Math
% Passing Reading
Overall Passing Rate (Average of the above two)
#Establish the bins
spending_bins = [0, 585, 630, 645, 680]
group_names = ["<$585", "$585-630", "$630-645", "$645-680"]
#Create a copy
school_spending_df = per_school_summary
per_school_summary.head(2)
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68%	81.93%	54.64%
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%
#categorize spending based on the bins
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)
per_school_summary["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)
school_spending_df
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68%	81.93%	54.64%	$585-630
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%	<$585
Figueroa High School	District	2,949	$1,884,411.00	$639.00	76.71	81.16	65.99%	80.74%	53.20%	$630-645
Ford High School	District	2,739	$1,763,916.00	$644.00	77.10	80.75	68.31%	79.30%	54.29%	$630-645
Griffin High School	Charter	1,468	$917,500.00	$625.00	83.35	83.82	93.39%	97.14%	90.60%	$585-630
Hernandez High School	District	4,635	$3,022,020.00	$652.00	77.29	80.93	66.75%	80.86%	53.53%	$645-680
Holden High School	Charter	427	$248,087.00	$581.00	83.80	83.81	92.51%	96.25%	89.23%	<$585
Huang High School	District	2,917	$1,910,635.00	$655.00	76.63	81.18	65.68%	81.32%	53.51%	$645-680
Johnson High School	District	4,761	$3,094,650.00	$650.00	77.07	80.97	66.06%	81.22%	53.54%	$645-680
Pena High School	Charter	962	$585,858.00	$609.00	83.84	84.04	94.59%	95.95%	90.54%	$585-630
Rodriguez High School	District	3,999	$2,547,363.00	$637.00	76.84	80.74	66.37%	80.22%	52.99%	$630-645
Shelton High School	Charter	1,761	$1,056,600.00	$600.00	83.36	83.73	93.87%	95.85%	89.89%	$585-630
Thomas High School	Charter	1,635	$1,043,130.00	$638.00	83.42	83.85	93.27%	97.31%	90.95%	$630-645
Wilson High School	Charter	2,283	$1,319,574.00	$578.00	83.27	83.99	93.87%	96.54%	90.58%	<$585
Wright High School	Charter	1,800	$1,049,400.00	$583.00	83.68	83.95	93.33%	96.61%	90.33%	<$585
school_spending_copy = school_spending_df
school_spending_copy.head(3)
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68%	81.93%	54.64%	$585-630
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%	<$585
Figueroa High School	District	2,949	$1,884,411.00	$639.00	76.71	81.16	65.99%	80.74%	53.20%	$630-645
school_spending_copy = school_spending_df
school_spending_copy.head(3)
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68%	81.93%	54.64%	$585-630
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13%	97.04%	91.33%	<$585
Figueroa High School	District	2,949	$1,884,411.00	$639.00	76.71	81.16	65.99%	80.74%	53.20%	$630-645
school_spending_copy["Average Math Score"] = school_spending_copy["Average Math Score"].astype('float')
school_spending_copy["Average Reading Score"] = school_spending_copy["Average Reading Score"].astype('float')
school_spending_copy["% Passing Math"] = school_spending_copy["% Passing Math"].str.replace("%", "").astype('float')
school_spending_copy["% Passing Reading"] = school_spending_copy["% Passing Reading"].str.replace("%", "").astype('float')
school_spending_copy["% Overall Passing"] = school_spending_copy["% Overall Passing"].str.replace("%", "").astype('float')
school_spending_copy.dtypes
School Type                        object
Total Students                     object
Total School Budget                object
Per Student Budget                 object
Average Math Score                float64
Average Reading Score             float64
% Passing Math                    float64
% Passing Reading                 float64
% Overall Passing                 float64
Spending Ranges (Per Student)    category
dtype: object
school_spending_copy.head(2)
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
Bailey High School	District	4,976	$3,124,928.00	$628.00	77.05	81.03	66.68	81.93	54.64	$585-630
Cabrera High School	Charter	1,858	$1,081,356.00	$582.00	83.06	83.98	94.13	97.04	91.33	<$585
#calculate Averages for the desired columns
math_spending_scores = school_spending_copy.groupby(["Spending Ranges (Per Student)"]).mean("Average Math Score")
#math_spending_scores
reading_spending_scores = school_spending_copy.groupby(["Spending Ranges (Per Student)"]).mean("Average Reading Score")
#reading_spending_scores
math_spending_passing_scores = school_spending_copy.groupby(["Spending Ranges (Per Student)"]).mean("% Passing Math")

reading_spending_passing_scores = school_spending_copy.groupby(["Spending Ranges (Per Student)"]).mean("% Passing Reading")
#reading_spending_passing_scores

math_reading_spending_passing_scores = school_spending_copy.groupby(["Spending Ranges (Per Student)"]).mean("% Overall Passing")
math_reading_spending_passing_scores
Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Spending Ranges (Per Student)					
<$585	83.452500	83.932500	93.460000	96.610000	90.367500
$585-630	81.900000	83.155000	87.132500	92.717500	81.417500
$630-645	78.517500	81.625000	73.485000	84.392500	62.857500
$645-680	76.996667	81.026667	66.163333	81.133333	53.526667
Scores by School Size
Perform the same operations as above, based on school size.
#establish the bins
size_bins = [0, 1000, 2000, 5000]
group_names =["Small (<1000)", "Medium(1000-2000)", "Large (2000-5000)"]
school_spending_copy["Total Students"] = school_spending_copy["Total Students"].str.replace(",", "").astype('float')
per_school_summary.head(1)
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
Bailey High School	District	4976.0	$3,124,928.00	$628.00	77.05	81.03	66.68	81.93	54.64	$585-630
#categorize the spending based on the bins
per_school_summary["School Size"] = pd.cut(school_spending_copy["Total Students"], size_bins, labels=group_names)
per_school_summary
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)	School Size
Bailey High School	District	4976.0	$3,124,928.00	$628.00	77.05	81.03	66.68	81.93	54.64	$585-630	Large (2000-5000)
Cabrera High School	Charter	1858.0	$1,081,356.00	$582.00	83.06	83.98	94.13	97.04	91.33	<$585	Medium(1000-2000)
Figueroa High School	District	2949.0	$1,884,411.00	$639.00	76.71	81.16	65.99	80.74	53.20	$630-645	Large (2000-5000)
Ford High School	District	2739.0	$1,763,916.00	$644.00	77.10	80.75	68.31	79.30	54.29	$630-645	Large (2000-5000)
Griffin High School	Charter	1468.0	$917,500.00	$625.00	83.35	83.82	93.39	97.14	90.60	$585-630	Medium(1000-2000)
Hernandez High School	District	4635.0	$3,022,020.00	$652.00	77.29	80.93	66.75	80.86	53.53	$645-680	Large (2000-5000)
Holden High School	Charter	427.0	$248,087.00	$581.00	83.80	83.81	92.51	96.25	89.23	<$585	Small (<1000)
Huang High School	District	2917.0	$1,910,635.00	$655.00	76.63	81.18	65.68	81.32	53.51	$645-680	Large (2000-5000)
Johnson High School	District	4761.0	$3,094,650.00	$650.00	77.07	80.97	66.06	81.22	53.54	$645-680	Large (2000-5000)
Pena High School	Charter	962.0	$585,858.00	$609.00	83.84	84.04	94.59	95.95	90.54	$585-630	Small (<1000)
Rodriguez High School	District	3999.0	$2,547,363.00	$637.00	76.84	80.74	66.37	80.22	52.99	$630-645	Large (2000-5000)
Shelton High School	Charter	1761.0	$1,056,600.00	$600.00	83.36	83.73	93.87	95.85	89.89	$585-630	Medium(1000-2000)
Thomas High School	Charter	1635.0	$1,043,130.00	$638.00	83.42	83.85	93.27	97.31	90.95	$630-645	Medium(1000-2000)
Wilson High School	Charter	2283.0	$1,319,574.00	$578.00	83.27	83.99	93.87	96.54	90.58	<$585	Large (2000-5000)
Wright High School	Charter	1800.0	$1,049,400.00	$583.00	83.68	83.95	93.33	96.61	90.33	<$585	Medium(1000-2000)
#calculate Averages for the desired columns
math_spending_scores = per_school_summary.groupby(["School Size"]).mean("Average Math Score")
#math_spending_scores
reading_spending_scores= per_school_summary.groupby(["School Size"]).mean("Average Reading Score")
#reading_spending_scores
math_spending_passing_scores = per_school_summary.groupby(["School Size"]).mean("% Passing Math")

reading_spending_passing_scores = per_school_summary.groupby(["School Size"]).mean("% Passing Reading")
#reading_spending_passing_scores

math_reading_spending_passing_scores = per_school_summary.groupby(["School Size"]).mean("% Overall Passing")
math_reading_spending_passing_scores
Total Students	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
School Size						
Small (<1000)	694.500	83.820	83.92500	93.55000	96.10000	89.885
Medium(1000-2000)	1704.400	83.374	83.86600	93.59800	96.79000	90.620
Large (2000-5000)	3657.375	77.745	81.34375	69.96375	82.76625	58.285
Scores by School Type
Perform the same operations as above, based on school type
#Create new series using groupby for
#Type, avg math and reading, % passing math reading and overall
#calculate Averages for the desired columns
type_math_scores = per_school_summary.groupby(["School Type"]).mean("Average Math Score")
#math_spending_scores
type_reading_scores= per_school_summary.groupby(["School Type"]).mean("Average Reading Score")
#reading_spending_scores
type_passing_math = per_school_summary.groupby(["School Type"]).mean("% Passing Math")

type_passing_reading = per_school_summary.groupby(["School Type"]).mean("% Passing Reading")
#reading_spending_passing_scores

type_overall_passing = per_school_summary.groupby(["School Type"]).mean("% Overall Passing")
type_overall_passing
Total Students	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
School Type						
Charter	1524.250000	83.472500	83.896250	93.620000	96.586250	90.431250
District	3853.714286	76.955714	80.965714	66.548571	80.798571	53.671429
