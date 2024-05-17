# Corpus Creation

This folder contains the scripts and keywords used for filtering scraped data to create the final domain-specific corpus. Currently, one generic scraping script is supplied in this folder for use in all languages in the scripts subdirectory. Any scripts with language specific rules and changes are found in that language's respective folder.

The relevant and irrelevant keywords for each language are in that language's respective folder. These keyword are meant for filtering for text related to political conflict and violence. If you are filtering for a different domain, you will need to develop your own keywords. Some folders contain multiple versions of the keyword files. We recommend using the latest version, but it is possible that older version may work better for your data.

In general, we have found that our filtered data usually ends up being ~80% the size of the unfiltered data. This is a good rule of thumb, but depending on your scraping guidelines, this number could be lower or higher. If the number is unexepectedly low, it is worth inspecting the irrelevant data file to determine if this is because the scraping yielded a lot of irrelevant data, or if the keywords need to be tailored to your dataset.

# Filtering Script

The filtering script will perform cleaning on the text in your CSV files, then filter based on a set of keywords and rules.

To use the scraping script, place all files to be filtered in a single directory called Filter with all files from each sources being located subdirectory with the same as the source as the name. For example, let there be 3 csv files from El Espectador:

The file structure is:
Filter
 -- elespectador
 -- -- elespactador01.csv
 -- -- elespactador02.csv
 -- -- elespactador03.csv

 These files should be in csv format with the attributes gathered by the scraping script.

If the Filter directory is not in your current working directory, change the file path in glob call in the third code cell to match the path to your Filter directory.

Change the references to the keyword files to match the path to the keyword files for the language you are in filtering in. You will require a relevant and irrelevant keyword file. Be sure you are referencing the correct file.

The script will create a directory called split which will contain:
1. Split files. By default, files will be split into <20MB partitions
2. Filtered files for each split file. These files will have the "_filtered" suffix.
3. A file containing the texts that were classified as irrelevant.

For training, all _filtered files should be concatenated into a single large file for ease of use.