# CA273 - Data Processing and Cleaning Assignment - Adam Tegart

The following is the errors and artefacts I encountered among the datasets I had to join and how I rectified the errors. There are also several entries that may not be classed as errors or artefacts, but I felt made the data easier to use and understand and overall cleaner.
I work through these errors as they appear in the datasets from newest to oldest, so at the time of finishing processing
I had corrected most of these.

## 1. Spelling errors within the columns names

In the 2020 jul-sep dataset it can be seen that the column names contain whitespace after the column name. This will stop
pandas from recognizing that this column has the same data as other columns with the same name, just without the space.

To fix this, I will use pandas to rename the columns with the following code.

![Spelling error in column](images/1.PNG)

## 2. Removing duplicate Jun in 2020 jul-dec

It could be seen in the 2020 jul-dec csv file that the month of June was included when it does not fall within
July - December. This resulted in duplication as this data was identical to the June data in the 2020 jan-jun file.

![Duplicated June in 2020](images/2.PNG)

As this was only one deletion I manually opened the csv file in notepad and deleted the chunk of data from June.

## 3. Fixing multiple representation error

Through the years, the names of columns changed. Examples of this include Date and Time in 2020 becoming just Time
in 2019. O'Connell St in 2020 had no ' in the name, and in 2019 it did. In 2019 columns like Grafton St @ CompuB
had the "at" abbreviated to @, so this had to be altered too.

![Spelling error in column](images/3a.PNG)

![Spelling error in column](images/3b.PNG)

To correct this I renamed the columns in pandas (first image) and when I was processing the 2008 file with a python script I output
the correct column names (second image).

## 4. Formatting incorrect dates

In the 2019 dataset the dates and time too were formatted differently to every other year. The measurements were
taken every 15 mins, the solution to this was seen in my processing notebook. The dates themselves had a - instead
of a / to separate the day, month and year. In addition to this, the first half had a mm/dd/yyyy format and the second half had a dd/mm/yyyy format. I decided to normalize it with the rest of the data and choose a dd/mm/yyyy format overall. This date issue can be seen below.

![mm/dd/yyyy format](images/4a.PNG)

![dd/mm/yyyy format](images/4b.PNG)

I used my processing python script to correct this also. If I specified the dates were to be switched it would use
the code below to fix it. it would also replace any -'s with /'s.

![Date formatting code](images/4c.PNG)
![Date formatting code](images/4d.PNG)


## 5. Coping with missing values

In the 2019 jan-jun dataset it can be seen that on 6/30/2019 (wrong date format in the raw data file) just before
9am there is about 18 values missing from 4:45 to 9:15. My python script ignores all hours in-between and just sums
up what is there. So the slot from 4-5am only has 45 mins worth of data, and the same is true for the 9am to 10am
slot. Below to the left is the pre-processed data in question and on the right the processed.

![Date formatting code](images/5a.PNG) ![Date formatting code](images/5b.PNG)

A solution to this that is past cleaning that I will not do in this project would be to analyse the typical patterns
of people on whatever day of the week this date corresponds to and estimate the amount of people there should be.


My program simply does what it can with the data given and so used the partial data and left a gap where the other
data should've been. I had to alter it so that even if 1, 2 or 3 values are given, it only does whatever is in 1
hour. ie, 4, 4:15, 4:30, and if there is no 4:45 it will just use those 3 to find 4:00.

![Date formatting code](images/5c.PNG)

## 6. Redefining old data

The data from 2010 and older had different sensor locations than the newer datasets. They had several locations
along Grafton St, some of which were no longer present there, eg. Korkys is no longer on Grafton St. This meant
I had to figure out how best to get meaning from the data.

I decided that I would average the people a sensor detected as majority of people walk the length of Grafton St.
This could then be interpreted as the people detected walking along Grafton St and therefore could be put into
the same column as Grafton St data in 2019-20.

I averaged the people in pandas for 2010 and with my python script for 2008, both shown below. In the python
script the first point shows if it is a Grafton St location just add the value. By the end of the script all
3 locational values will have been summed, then I can just average it at the second point before printing.

![Redefining old data using pandas](images/6a.PNG)

![Redefining old data using python](images/6b.PNG)

## 7. Ignoring redundant data

One issue with receiving many different files is that some may contain data which you do not need. In the
2008 file there was information before the tables such as Location Address: Dublin BID and the Entrance
name and so on. In addition, there was also a column for pedestrians leaving the area being monitored.
This data too was redundant.

![Redundant data in 2008 ods](images/7a.PNG)

To correct this, my program only looked for lines that contained a time, or lines with an entrance name
as that is the location the following data corresponds to.

![Ignoring redundant data](images/7b.PNG)

## 8. Removing blank rows from the data

The script used to ignore redundant data also removed blank rows as it ignored any rows in the 2008
files that did not contain data which I specified to take.

![Blank lines in 2008 ods](images/8.PNG)

## 9. Replacing null values

Null values can be a pain when using a dataset for any form of analysis or datamining even. So I decided
to default null values to 0 in my output csv file.

![Setting all nulls to 0](images/9.PNG)

## 10. Correcting non-numeric values

After exporting my csv file from my notebook I decided to check that there were no duplicates or invalid
types in any of the columns. Converting all the columns to numeric and using a numeric facet I found
that in row 12,602 of the processed file there was an O instead of a 0. This cannot be transformed into
a number and so must be changed.

![Non-numeric found in openrefine](images/10.PNG)

## 11. Extraneous whitespace in rows

Using openrefine I found that 2 rows within the 2020 dataset had whitespace in place of a value. This
meant that the value in this row would not default to NaN or 0 as there was an entry, it was just a
blank entry that we as humans cannot see at first glance. I manually changed these as there was only
2 rows that this applied to. 27,342 and 32,851 were the row numbers.

My assumption is that row 27,342 had values that were lost for whatever reason during collection, ie. connection dropped during data transfer, sensor malfunction. 32,851 was one of these rows as the hourly sensor data had yet to be inputted as it is the last row in the dataset.

!["Blank rows" as a result of whitespace](images/11.PNG)

## 12. Value entry error

Upon finalizing my cleaning.md and wrapping up my project I stumbled upon something strange. There were reportedly 340,000 on Grafton St at 5pm on 13/12/10. This seems like an absurdly high number. At first I thought to research it and see if there was a reason. I was lead to believe that this value might be accurate as this seemed like around the time christmas lights would've been turned on. Looking at the 2010 data itself before I average the Grafton St value revealed something to me.

It turns out that the value was indeed a mistake and a value of 999,999 was entered into the Grafton St at Korkys column. This was most definitely invalid data as this is a very specific number and does not seem likely that only 6-7k people were on the lower part of Grafton St but there was 1 million on the upper end. I decided to change this value manually, and I picked a number inbetween the hour before (6.8k) and the hour after (2.8k), so I went with 5k.

The high value in the describe can be seen below, along with the excel function I used to find the value and the value in excel. There is also a picture of the value itself in the 2010 dataset before being averaged.

![df.describe()](images/12a.PNG)

![Excel function](images/12b.PNG)

![Very large value](images/12c.PNG)

![Data point in 2010](images/12d.PNG)
