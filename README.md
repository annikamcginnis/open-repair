# Analyzing Open Repair Data on Community Electronics Repair: [Story](https://annikamcginnis.github.io/electronics-repair/)

## Goal
- To analyze repairability of various types of electronic devices and brands over time
- To understand why devices couldn't be repaired and the trends in these issues over time
- To understand the problems why people need to repair their electronics and the trends in these problems over time by category and brand, as well as repairability by each problem category

## Main Findings

- The Open Repair Alliance has logged over 208,000 devices brought for repair at more than 20,000 community-led events in 31 countries on all continents.
- People are using their devices long past warranty periods, which generally cover only about one to two years. While the average age of a phone brought to a cafe is almost five years, the typical repair age for watches, projectors and sewing machines is 26-28 years. The Alliance has successfully repaired some mobile phones and computers that people have held onto for over 40 years. And some of the oldest items, like clocks, were constructed as far back as the 1880s.
- Of the laptop brands with 400 or more repairs logged, Lenovo and Asus laptops were generally brought for repair sooner than the typical laptop, and HP and Apple lasted slightly longer.
- The average phone was repaired at four to five years old. But for phones logged at the events, Apple, Huawei, LG, Motorola, Microsoft, Xiaomi and Alcatel phones typically broke down at just two to three years, with Samsung slightly ahead at about four years.
- Some older phones manufactured by Nokia, Siemens and Panasonic, however, were repaired at over a decade of age. Though this is partly because these brands have simply been around longer or no longer operate, driving up the median age, this disparity also speaks to the durability of older phones compared to some newer, short-lived varieties. Phones made by the Chinese company Xiaomi, for instance, needed repair at an average of just two years post manufacture.
- Display or screen issues are some of the most common reasons people took their phones for repair, especially in the last decade.
- Overall, about half of all items taken to events were fixed, but a fourth were declared “end of life.” The main reasons devices couldn't be repaired (in order) were that spare parts were too expensive, parts were unavailable, there was no way to open the product, or there was no available repair information.
- Repairability at the cafes has declined for products manufactured in the last two decades compared to those made in the 1980s-90s. But the repair rate differs significantly between various categories of devices, with kitchen appliances, lighting and personal care items like hair dryers performing increasingly poorly.
- Phones and computers, however, saw some improvement in the 2000s in the ability for non-manufacturers to fix them - perhaps partly because of their ubiquity. But in the last three years, their repairability has also dropped.
  
## Data Collection
I acquired datasets from the Open Repair Alliance, a consortium of repair organizations from around the world that share data on electronics repairs: [Data](https://openrepair.org/open-data/downloads/)
  
## Data Analysis
I used the following languages, libraries, APIs and models in my data cleaning and analysis: 
- Python
- Pandas
- Regex
- R
- ggplot
- bart-large-mnli
- Google Cloud Translate API
- Counter

I did most of my data cleaning and analysis using the Pandas library in Python. As part of this, I used Counter to do some word counts. I do extensive data analysis of most columns in the dataset, including repairability by device type, brand and country over year of manufacture and the reasons for non-repair. I look in detail into the data on mobile phones and laptops. My analysis requires pivoting the data into several sub-dataframes. When looking at phone and laptop data by brand, I filter the data to include only brands that appear at least 10 times. 

Working with R in the same Jupyter Notebook, I produce several exploratory charts using the ggplot library, including multiple box and whisker charts and distributions.

I save several pivoted datasets as CSVs that I use for visualization in Datawrapper.

Working with the dataframe as a list of dictionaries, I create a service account on the Google Cloud Translate API and attempt to translate all the values in this column to English. But this was very expensive and only ended up working for a small sub-section of the data.

I also attempted to categorize the listed problems (the specific problem each individual faced on their device, in their own words) into broader categories. To create the categories, I used Pandas to generate two random samplse of 500 rows each. I fed one to the ChatGPT LLM and the other to the Perplexity LLM and asked the chatbots to generate categories, which I further narrowed using the LLMs. I then brought both categorizations to ChatGPT and merged the two to create a set of 50 categories that I hoped would largely apply to most of the data. Next, I downloaded the [bart-large-mnli zero-shot classification model](https://huggingface.co/facebook/bart-large-mnli) by Facebook and asked the model to categorize all 3,373 rows in the problem column for the mobile phones data. Although this was completed, I realized most of the categorization was incorrect, other than categories with a specific keyword that the model was able to recognize (like "display" or "screen", which helped it categorize the row correctly as "Display/Screen Issues"). Otherwise, this method was unreliable in determining accurate categories, especially for non-English languages. 


## Reflection
I improved my skills in in-depth analysis using Pandas and R-ggplot on a very large (208,000+ rows) dataset. I also learned how to use a service account to send data to an API (as well as the importance of understanding the expected cost of a large-scale API request before running it!). I learned the concept of AI zero-shot classification and was able to practice it, though I also learned some of its current limitations and likelihood for error. 

I also tried out some new charts in Datawrapper. For my multiple dot plot, I needed to create a new numeric column in my data where each brand was given a unique number in order of its mean product age value for mobile devices.

Speaking to one of the team leads at the organization behind the data helped me pivot from conducting change-over-time analysis by the event year category to the year of manufacture category, which is more accurate since products of any age can be brought in anytime. 

I also learned that considering sample size is very important when conducting comparisons. In this dataset, some countries have very few data compared to others with thousands. The same disparities exist for years of manufacture and brands. For some of my analysis and charts that took the average for specific categories over years, I had to throw out categories or years with too small data because these averages based on very small sample sizes could mislead viewers into thinking they were comparable to other categories with much larger sample sizes. In the end, I didn't use any country comparisons for conclusions or visualizations because the sample sizes were too small for non-European countries.

**In the future:** 
I would troubleshoot errors with the bart-large-mnli model and explore how to get it to more accurately categorize the data. I would also look for other more reliable models that do the same thing. Once I can more accurately categorize the problems in the entire dataset, I can realize my goal of analyzing electronics problems by device category, brand and year of manufacture, which wasn't possible at this stage.
