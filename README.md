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
Analyzing video categories helps identify which content types are most visible and engaging.

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
In this section, significant insights and relationships within the dataset are highlighted through clear and concise visualizations.

**Engagement Over Time**

*Total Views Over Time*

**CODE SNIPPET:**

ggplot(daily_engagement, aes(x = trending_date, y = total_views)) 
+  geom_line(color = "steelblue") 
+  labs(
    title = "Total Views Over Time (U.S. Trending Videos)",
    x = "Trending Date",
    y = "Total Views"
  )

**PLOT:**

<img width="737" height="425" alt="Views Over Time" src="https://github.com/user-attachments/assets/cfdc778d-ceb7-4d6b-abde-c99ce8955e2c" />

**FINDING:** A steady increase in daily views is observed, with spikes corresponding to viral content or major platform events.

--------------------------------

*Total Likes Over Time*

**CODE SNIPPET:**

ggplot(daily_engagement, aes(x = trending_date, y = total_likes)) 
+  geom_line(color = "darkgreen") 
+  labs(
    title = "Total Likes Over Time (U.S. Trending Videos)",
    x = "Trending Date",
    y = "Total Likes"
  )

**PLOT:**

<img width="737" height="425" alt="Likes Over Time" src="https://github.com/user-attachments/assets/5548764c-87d3-466a-9366-5cf371d22e28" />

**FINDING:** Likes show a similar upward trend as views, with later peaks suggesting a concentration of high-performing videos.

--------------------------------

*Total Comments Over Time*

**CODE SNIPPET:**

ggplot(daily_engagement, aes(x = trending_date, y = total_comments)) 
+  geom_line(color = "purple") 
+  labs(
    title = "Total Comments Over Time (U.S. Trending Videos)",
    x = "Trending Date",
    y = "Total Comments"
) 

**PLOT:**

<img width="737" height="425" alt="Comments Over Time" src="https://github.com/user-attachments/assets/0e60a503-511f-448c-81f6-ba40a60d755d" />

**FINDING:**  Comment activity exhibits a sawtooth pattern, with a notable rise around April, likely driven by discussion-worthy or controversial videos.

-------------------------

**Top Categories**

*Top 10 Categories by Total Views*

**CODE SNIPPET:** 

top_categories_views <- yt_us %>%
+     group_by(category) %>%
+     summarise(total_views = sum(views, na.rm = TRUE)) %>%
+     arrange(desc(total_views)) %>%
+     slice_max(total_views, n = 10)

ggplot(top_categories_views, aes(x = reorder(category, total_views), y = total_views)) +
+     geom_col(fill = "skyblue") +
+     coord_flip() +
+     labs(
+         title = "Top 10 Categories by Total Views",
+         x = "Category",
+         y = "Total Views"
+     )

**PLOT:** 

<img width="737" height="425" alt="Top 10 categories" src="https://github.com/user-attachments/assets/60db899c-b2ba-42ab-b78d-61d721acee7c" />

**FINDING:** Music reigns as the most-watched category with over 40 billion total views, far surpassing others. It reflects the global popularity and replayable nature of music videos.

<img width="411" height="498" alt="Screenshot 2025-07-19 185054" src="https://github.com/user-attachments/assets/9469ec75-b874-484a-91b1-1ea59f39a389" />

------------------------

*Top Categories by Average Engagement (Likes)*

**CODE SNIPPET:** 

top_categories_likes <- yt_us %>%
+     group_by(category) %>%
+     summarise(avg_likes = mean(likes, na.rm = TRUE)) %>%
+     arrange(desc(avg_likes)) %>%
+     slice_max(avg_likes, n = 10)

ggplot(top_categories_likes, aes(x = reorder(category, avg_likes), y = avg_likes)) +
+     geom_col(fill = "coral") +
+     coord_flip() +
+     labs(
+         title = "Top 10 Categories by Average Likes",
+         x = "Category",
+         y = "Average Likes"
+     )

**PLOT:** 

<img width="737" height="425" alt="top 10 by avg likes" src="https://github.com/user-attachments/assets/ad5d2f78-43e0-4650-b7b0-26369b0e59cd" />

**FINDING:** Nonprofits & Activism tops the average likes chart, showing high user engagement per video. Music follows closely, proving its emotional and entertainment value.

<img width="400" height="486" alt="Screenshot 2025-07-19 185520" src="https://github.com/user-attachments/assets/79322893-160e-4ed6-bdb4-a616aae8f8a4" />

---------------------------------

**Best Time to Publish**

*Average Views by Publish Hour*

**CODE SNIPPET:**

views_by_hour <- yt_us %>%
+     group_by(publish_hour) %>%
+     summarise(avg_views = mean(views, na.rm = TRUE)) %>%
+     arrange(publish_hour)
> ggplot(views_by_hour, aes(x = publish_hour, y = avg_views)) +
+     geom_line(group = 1, linewidth = 1.2) +
+     geom_point(size = 2) +
+     scale_x_continuous(breaks = 0:23) +
+     labs(
+         title = "Average Views by Publish Hour",
+         x = "Hour of Day (0 = Midnight)",
+         y = "Average Views"
+     )

**PLOT:**

<img width="737" height="425" alt="avg views by publish hr" src="https://github.com/user-attachments/assets/a455793a-d0cb-4068-957a-5cfa53f79e43" />

**FINDING:** Engagement peaks around 4 AM and 9–10 AM—likely due to YouTube’s promotion algorithm—while views decline through the afternoon and evening.

-------------------------------

*Average Views by Day of the Week*

**CODE SNIPPET:**

yt_us <- yt_us %>%
+     mutate(publish_wday = wday(publish_date, label = TRUE))
> 
> views_by_wday <- yt_us %>%
+     group_by(publish_wday) %>%
+     summarise(avg_views = mean(views, na.rm = TRUE))
> ggplot(views_by_wday, aes(x = publish_wday, y = avg_views)) +
+     geom_col(fill = "darkseagreen3") +
+     labs(
+         title = "Average Views by Day of the Week",
+         x = "Day of Week",
+         y = "Average Views"
+     )

**PLOT:**

<img width="737" height="425" alt="avg views by day of wk" src="https://github.com/user-attachments/assets/84968ac5-2604-4db6-bdfb-09b531e3aa94" />

**FINDING:** Friday and Sunday yield the highest average views, while Monday to Wednesday perform moderately and Saturday sees the lowest viewership.

--------------------------------------

**Correlation Analysis**

**CODE SNIPPET:**

cor_data <- yt_us %>%
+     select(views, likes, dislikes, comment_count)
> cor_matrix <- cor(cor_data, use = "complete.obs")
> print(cor_matrix)
                  views     likes  dislikes comment_count
views         1.0000000 0.8491765 0.4722132     0.6176213
likes         0.8491765 1.0000000 0.4471865     0.8030569
dislikes      0.4722132 0.4471865 1.0000000     0.7001836
comment_count 0.6176213 0.8030569 0.7001836     1.0000000

corrplot(cor_matrix, method = "color", type = "upper", 
+          tl.col = "black", tl.srt = 45, addCoef.col = "black", 
+          number.cex = 0.8, col = colorRampPalette(c("lightblue", "white", "red"))(200))

**PLOT:**

<img width="737" height="425" alt="Correlation heatmap" src="https://github.com/user-attachments/assets/26577ed6-72bf-43b2-aacc-1428443749d4" />

**FINDING:** Correlation analysis shows that views and likes are strongly linked (r = 0.85), with moderate ties among comments, dislikes, and likes—indicating that both appreciation and controversy fuel engagement.

-----------------------------

**Views vs Likes**

**CODE SNIPPET:**

ggplot(yt_us, aes(x = views, y = likes)) 
+     geom_point(alpha = 0.4) 
+     geom_smooth(method = "lm", se = FALSE, color = "red") 
+     labs(title = "Views vs Likes", x = "Views", y = "Likes")

**PLOT:**

<img width="737" height="425" alt="Views vs Likes" src="https://github.com/user-attachments/assets/c65383f8-17d0-418b-a4e3-f46631b456d0" />

**FINDING:** The plot reveals a strong positive linear relationship between views and likes, confirming that increased visibility generally leads to higher user appreciation despite some outliers.

---------------------------

**Outliers**

**CODE SNIPPET:**

yt_us <- yt_us %>%
+     mutate(
+         like_view_ratio = ifelse(views == 0, NA, likes / views)
+     )
> threshold_high <- quantile(yt_us$like_view_ratio, 0.99, na.rm = TRUE)
> threshold_low <- quantile(yt_us$like_view_ratio, 0.01, na.rm = TRUE)
> 
> high_like_low_view <- yt_us %>%
+     filter(like_view_ratio >= threshold_high) %>%
+     select(title, channel_title, views, likes, like_view_ratio) %>%
+     arrange(desc(like_view_ratio))
> high_view_low_like <- yt_us %>%
+     filter(like_view_ratio <= threshold_low) %>%
+     select(title, channel_title, views, likes, like_view_ratio) %>%
+     arrange(like_view_ratio)
> head(high_like_low_view, 5)

# A tibble: 5 × 5
  title                                   channel_title  views  likes like_view_ratio
  <chr>                                   <chr>          <dbl>  <dbl>           <dbl>
1 Bruno Mars - Finesse (Remix) [Feat. Ca… Bruno Mars    5.49e5 1.59e5           0.290
2 Luis Fonsi, Demi Lovato - Échame La Cu… LuisFonsiVEVO 5.00e5 1.35e5           0.271
3 j-hope 'Airplane' MV                    ibighit       5.28e6 1.40e6           0.266
4 dodie - Secret For The Mad              dodieVEVO     1.29e5 3.28e4           0.254
5 Louis Tomlinson - Miss You (Official V… LouisTomlins… 9.86e5 2.42e5           0.245

> head(high_view_low_like, 5)

title                                     channel_title views likes like_view_ratio
  <chr>                                     <chr>         <dbl> <dbl>           <dbl>
1 Apple Clips sample                        Steve Kovach   2259     0               0
2 Breaking Bad's Bryan Cranston on Meeting… hudsonunions… 15058     0               0
3 Kelly Oubre Punches John Wall in the Lea… Rob Andretti   2197     0               0
4 Breaking Bad's Bryan Cranston on Meeting… hudsonunions… 34207     0               0
5 Kelly Oubre Punches John Wall in the Lea… Rob Andretti   2447     0               0

yt_us %>%
+     filter(!is.na(like_view_ratio), like_view_ratio > 0) %>%
+     ggplot(aes(x = like_view_ratio)) +
+     geom_histogram(bins = 100, fill = "steelblue", color = "white") +
+     scale_x_log10() +
+     geom_vline(xintercept = quantile(yt_us$like_view_ratio, 0.99, na.rm = TRUE),
+                color = "red", linetype = "dashed", linewidth = 1) +
+     geom_vline(xintercept = quantile(yt_us$like_view_ratio, 0.01, na.rm = TRUE),
+                color = "red", linetype = "dashed", linewidth = 1) +
+     labs(
+         title = "Distribution of Like-to-View Ratio (Log Scale)",
+         x = "Like-to-View Ratio (log10)",
+         y = "Number of Videos"
+     )

**PLOT:** 

<img width="737" height="425" alt="Dist of Like to view ratio" src="https://github.com/user-attachments/assets/8b347a23-9434-49d4-8fbc-200722838284" />

**FINDING:** Most videos have a like-to-view ratio between 0.01 and 0.05, but extreme outliers reveal unique viewer responses—either high resonance or low approval—offering key insights for content strategy.

---------------------------

**6. Conclusions and Recommendations**

**Conclusion:**

•	Engagement is multifaceted: While views indicate reach, metrics like likes and comments better reflect audience sentiment and content impact.

•	Category matters: Music and Entertainment dominate in viewership, but niche categories like Nonprofits & Activism generate stronger per-video engagement.

•	Timing influences visibility: Early morning uploads (especially around 4 AM and 9–10 AM) and end-of-week days (Friday, Sunday) consistently show higher engagement, suggesting strategic publishing times.

•	Viewer response patterns vary: Most videos follow expected engagement ratios, but outliers—both positive and negative—reveal insights into content that resonates deeply or underperforms despite visibility.

**Recommendations:**

•	Optimize for timing: Publish videos in the early morning and on high-engagement days (Friday, Sunday) to maximize visibility.

•	Prioritize engagement: Create content that encourages likes and comments, especially in high-performing or emotionally resonant categories.

•	Refine with data: Regularly analyze top and low-performing videos to understand what drives strong audience reactions and adjust strategy accordingly.

**7. Appendix**

**Data Files Used**

•	USvideos.csv – Raw dataset containing metadata for trending YouTube videos in the U.S.

•	US_category_id.json – JSON file mapping category_id to readable category names

**R Packages**

•	tidyverse – Data manipulation and visualization

•	lubridate – Date-time parsing and feature engineering

•	janitor – Cleaning column names

•	ggplot2 – Plotting visualizations

•	corrplot – Generating correlation heatmaps

•	skimr – Exploring data structure and summaries

•	jsonlite – Reading and parsing JSON files

**Key Steps**

•	Cleaned column names using janitor::clean_names()

•	Parsed trending_date and publish_time using lubridate to create publish_date and publish_hour

•	Mapped category_id to category names using US_category_id.json via jsonlite

•	Created new features like like_view_ratio and publish_wday for analysis

•	Filtered or excluded rows with non-finite values during visualization stages

**Sample Code Snippet**

*Load and clean base dataset*

yt_us <- read_csv("USvideos.csv") %>%
  clean_names()

*Parse dates and create time features*

yt_us <- yt_us %>%
  mutate(
    trending_date = as.Date(trending_date, format = "%y.%d.%m"),
    publish_date = as.Date(publish_time),
    publish_hour = hour(publish_time)
  )

*Load and join category names*

category_json <- fromJSON("US_category_id.json")
category_df <- as_tibble(category_json$items)
category_lookup <- category_df %>%
  transmute(
    category_id = as.integer(id),
    category = snippet$title
  )

yt_us <- yt_us %>%
  left_join(category_lookup, by = "category_id") %>%
  select(-thumbnail_link, -description)

**End of report**
