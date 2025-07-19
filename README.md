# yt-trending-data-insights
Analysis of YouTube trending videos using R. Includes insights on engagement, categories, upload timing, and outliers to understand what makes videos go viral.

**Table of Contents**
1. Introduction
2. Data Description
3. Data Cleaning and Preparation
4. Exploratory Data Analysis
5. Correlation, Insights, and Visualizations
6. Conclusions and Recommendations
7. Appendix

**1. Introduction & Business Task**
This case study explores the YouTube Trending Videos dataset to uncover patterns in video engagement, publishing behavior, and performance across countries and content categories. By analyzing key metrics such as views, likes, comments, and publish times, the objective is to understand what factors contribute to a video trending on the platform.
The goal is to derive actionable insights that can help content creators optimize for audience engagement, assist marketers in identifying high-performing content strategies, and guide platforms or advertisers in enhancing visibility and viewer retention. This analysis is conducted using R for data cleaning, exploration, and visualization.

**2. Data Description**
The dataset used for this analysis is the YouTube Trending Videos dataset, originally published on Kaggle. It contains daily records of trending videos across multiple countries, including metadata such as:

•	Video title and channel name

•	Publish and trending dates

•	View, like, dislike, and comment counts

•	Tags and video category

•	Thumbnail link and description

•	Country of trend

For this case study, the focus was on the United States dataset to maintain consistency in audience demographics and time zones.

**3. Data Cleaning & Preparation**
I used R to clean, transform, and prepare the YouTube Trending Videos (US) dataset. The following steps were performed to ensure the data was analysis-ready:

Tools and Packages

  •	R programming language
  
  •	tidyverse – for data manipulation and visualization
  
  •	janitor – for cleaning column names
  
  •	lubridate – for date-time conversions
  
  •	jsonlite – for parsing category metadata (JSON format)
  
  •	skimr – for summarizing and inspecting data

*Steps*

Loading Data
  
  •	Imported USvideos.csv and US_category_id.json.
  
  •	Cleaned column names using janitor::clean_names().
Date Conversion
  
  
  •	Converted trending_date to proper Date format using as.Date() with the format "%y.%d.%m".
 
  •	Parsed publish_time with ymd_hms() and extracted:
    
    o	publish_date – as a Date object
    
    o	publish_hour – hour of day using lubridate::hour()
    
    o	publish_wday – weekday using wday(label = TRUE)

Mapping Categories

  •	Used jsonlite to read the US_category_id.json file.
  
  •	Created a category variable by mapping category_id to category names using a lookup table.

Removing Unnecessary Columns
  
  •	Dropped thumbnail_link and description to reduce clutter for analysis.

Handling Missing Values

  •	Used skim() and colSums(is.na()) to assess missingness.
  
  •	Verified no missing values in key engagement metrics (e.g., views, likes, comments).

Feature Engineering

  •	Created like_view_ratio = likes/views to measure engagement quality.
  
  •	This metric was later used for outlier detection and visualization.

Duplicate Check

  •	Ensured each video-date combination was unique by inspecting video_id + trending_date.
  
  •	No exact duplicates were found post-cleaning.

Exporting Clean Data

  •	Cleaned dataset retained 40,949 rows across 17 variables and was used directly for further EDA.

This structured and transformed dataset provided a reliable foundation for the subsequent exploratory data analysis and insight generation phases.


**4. Exploratory Data Analysis**
I conducted exploratory data analysis (EDA) to examine content performance, user engagement patterns, and publishing behaviors. The analysis involved descriptive statistics, visualizations, and correlation assessments to uncover trends across views, likes, comments, and video categories. This helped identify what drives engagement and visibility on the platform.

**Engagement Over Time**

*Total Views Over Time*

  •	A steady upward trend is observed in total daily views.
  
  •	Spikes in viewership align with periods of viral content or major platform events.

*Total Likes Over Time*

  •	Likes followed a similar upward trajectory to views, confirming strong engagement growth.
  
  •	Higher peaks toward the later period may indicate a clustering of high-performing videos.

*Total Comments Over Time*

  •	Comment activity displays a saw-tooth pattern, likely influenced by specific videos that stirred discussions or controversy.
  
  •	A noticeable increase is seen around April, echoing trends in views and likes.

These trends demonstrate how YouTube's trending content has gained increasing traction over time, highlighting the platform’s dynamic nature.

**Top Categories**
Analyzing video categories helps identify which content types dominate in terms of visibility and engagement.

*Top 10 Categories by Total Views*

  •	Music and Entertainment are clear leaders in total viewership.
  
  •	Categories like Film & Animation, Comedy, and People & Blogs also perform well, reflecting broad viewer interest in creative and personality-driven content.

*Top Categories by Average Engagement (Likes)*
  
  •	While Music remains high in engagement, Nonprofits & Activism rank first in average likes, suggesting niche content can drive strong emotional reactions.

  •	Categories like Gaming and Film & Animation also show solid per-video engagement, indicating strong fan communities.

These insights highlight that while some categories dominate in scale, others excel in per-video audience interaction, valuable for both creators and marketers tailoring content strategy.

**Best Time to Publish**
Understanding when to publish content can significantly impact a video’s initial visibility and long-term engagement. This analysis focuses on average view counts by hour of the day and day of the week.

*Average Views by Publish Hour*

  •	Peak engagement occurs around 4 AM and again between 9–10 AM, likely reflecting YouTube’s internal content promotion schedule.
  
  •	Views drop steadily in the afternoon and evening hours, suggesting early-day uploads receive more algorithmic exposure.

*Average Views by Day of the Week*

  •	Friday and Sunday show the highest average views.
  
  •	Weekdays like Monday to Wednesday perform moderately, while Saturday tends to have the lowest average viewership.

These patterns suggest that publishing videos in the early morning on weekdays, especially Fridays and Sundays, can maximize initial traction and trending potential.

**Correlation Analysis**
To understand the relationships between key engagement metrics, a Pearson correlation matrix was generated for views, likes, dislikes, and comment_count.

*Key observations*:

  •	Views and Likes show a strong positive correlation (r = 0.85), suggesting likes are a reliable indicator of reach.
  
  •	Likes and Comments also correlate strongly (r = 0.80), reflecting higher user interaction on well-received videos.
  
  •	Dislikes show moderate correlations with other metrics, indicating that polarizing or controversial content still drives engagement.
  
  •	Views and Comment Count are moderately correlated (r = 0.62), possibly reflecting passive viewers vs. active commenters.

These insights confirm that while views drive exposure, engagement metrics like likes and comments offer a richer understanding of audience sentiment.

**Views vs Likes**
This scatter plot with a linear regression line reinforces the strong correlation between views and likes:

  •	A clear upward trend confirms that videos with higher views typically earn more likes.
  
  •	A few outliers (high-like but moderate-view videos) may indicate strong community support or virality in specific fan bases.
  
  •	The dense clustering in the lower-left shows that most trending videos gather moderate views and likes.

**Outliers**
To detect unusual patterns in engagement, a like-to-view ratio was calculated for each video. This metric reflects the percentage of viewers who took the extra step to like a video — a strong signal of quality or viewer sentiment.

Key takeaways:

  •	The majority of videos cluster between 1% to 10% like-to-view ratio, indicating standard engagement levels.
  
  •	The top 1% (far right of red line) represent videos with extremely high engagement, possibly from niche audiences or loyal fanbases.
  
  •	The bottom 1% (left of red line) include videos with disproportionately high views but very few likes — potentially due to controversy, misleading titles, or mass exposure without strong viewer connection.

Outlier analysis helps surface videos that over- or under-perform relative to their view count, offering deeper insight into what drives meaningful audience interaction.

**Visualizations Created**

**Engagement Over Time**

*Total Views Over Time*

<img width="737" height="425" alt="Views Over Time" src="https://github.com/user-attachments/assets/afec0f9f-f33c-4023-86cb-2bda7b5a377a" />

*Total Likes Over Time*

<img width="737" height="425" alt="Likes Over Time" src="https://github.com/user-attachments/assets/cf812cf9-b631-4342-b67c-a8bbd0ad10a6" />

*Total Comments Over Time*

<img width="737" height="425" alt="Comments Over Time" src="https://github.com/user-attachments/assets/775bcec8-aff0-47c7-8955-2432120ff2cc" />

**Top Categories**

*Top 10 Categories by Total Views*

<img width="737" height="425" alt="Top 10 categories" src="https://github.com/user-attachments/assets/0359d0f9-bf51-43af-8547-66b11084c0f3" />

*Top Categories by Average Engagement (Likes)*

<img width="737" height="425" alt="top 10 by avg likes" src="https://github.com/user-attachments/assets/394dcc7d-a10f-4069-829e-645b8a03b334" />

**Best Time to Publish**

*Average Views by Publish Hour*

<img width="737" height="425" alt="avg views by publish hr" src="https://github.com/user-attachments/assets/8460c4b0-70c2-40a9-8645-a6efbed38455" />

*Average Views by Day of the Week*

<img width="737" height="425" alt="avg views by day of wk" src="https://github.com/user-attachments/assets/2a489f0f-8cd3-4e92-be22-257e6e7d1cb3" />

**Correlation Analysis**

<img width="737" height="425" alt="Correlation heatmap" src="https://github.com/user-attachments/assets/be2b7beb-625f-4d96-acbd-03c2c35604ac" />

**Views vs Likes**

<img width="737" height="425" alt="Views vs Likes" src="https://github.com/user-attachments/assets/00ef9e9e-1c66-4d54-9949-360451f957ff" />

**Outliers**

<img width="737" height="425" alt="Dist of Like to view ratio" src="https://github.com/user-attachments/assets/42546438-16cd-4edf-a493-7f57f7008837" />


**5. Correlation, Insights, and Visualizations**
