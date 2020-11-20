# Adam Tegart Processing and Cleaning project overview

I began my approach to this topic by first scoping out the datasets. I wanted to understand all the attributes, values etc before I began working on joining them together. I approached the 2020 dataset first as it seemed the easiest.

I imported the dataset into pandas in a [Jupyter notebook](notebooks/ca273-processing.ipynb) and check out the .head() to see the data was imported correctly and checked out all the column names. My whole processing process is documented in the Jupyter notebook.

Using the append command in pandas I joined all of these datasets together into one dataframe. I exported this from jupyter into a [processed csv file](data/processed/adamtegart-a3-processed.csv). I then put this file into openrefine so that I could scrutinize the columns and find duplicates, non-numeric values etc. I wasn't disappointed!

Upon correcting these errors and having full cleaned the dataset, I had a finalized dataset in both a [csv](data/processed/adamtegart-a3-finalized.csv) and [xlsx](data/processed/adamtegart-a3-finalized.xlsx) format. I completed a [cleaning markdown file](notebooks/cleaning.md) to explain my whole process of cleaning the dataset.

With the dataset full finalized I imported it into my jupyter notebook and did some final tasks using the .describe() and .info() methods. This lead me to an interesting discovery which I'm still not completely sure is an error or a valid record. Finally I added code to my notebook to solve the final question of how many pedestrians were recorded on O'Connell St outside Clerys in March of all years excluding 2008.

I saved my final draft of my notebook to my git repository and made sure to make a final commit including all recent changes, then pushed changes to the gitlab server.

### Different tools used

*   Jupyter (processing notebook)
*   Cygwin (to run python scripts)
*   Atom (to edit md files)
*   Microsoft Excel (Visual Basic Application)
*   OpenRefine
*   Google and Google maps (for domain knowledge)
