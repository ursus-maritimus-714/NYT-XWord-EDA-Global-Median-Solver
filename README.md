# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle
## Introduction
### Project Goal and Data Sources
This exploratory data analysis (EDA) of Global Median Solver (GMS) performance on the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords) will serve as a key step toward the goal of building a predictive model of GMS performance on future NYT puzzles. This analysis yielded the discovery of GMS and constructor performance features, as well as numerous puzzle-structure, clue and answer-related features, with significant correlation to GMS performance on individual puzzles. These identified features will be further refined to serve as inputs into a forward-predictive model of GMS performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info have a contract with NYT for access to the raw data underlying these metrics, and I do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site (though [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks). The second, [XWStats](xwstats.com), is my source for historical GMS data. Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K solves recorded for them. It is from these raw solve times for each puzzle that he has extracted the raw global median solve time (GMST), as well as the deviation of GMST from the 10-puzzle moving average of the median solver for a given puzzle. These two metrics serve as the performance variables in the present EDA. I do not have the underlying raw data for solvers in the database apart from my own (Individual Solver 1) and that of one other person (Individual Solver 2); both of whom are the basis of similar exploratory data analyses. Please strongly consider supporting both of these wonderful sites financially; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Puzzle Properties and Solver Performance
The NYT crossword has been published since 1942, and many consider its modern era to have started with the arrival of Will Shortz (as only its 4th editor ever) 30 years ago. A new puzzle for each day is published online at either 6 PM (weekend) or 10 PM (weekend) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick of some sort (e.g., rebuses where the solver must enter more than one letter into one or more squares). Additionally, the early week puzzles generally have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions than the earlier week puzzles. Along with the open construction comes longer answers, often consisting of multiple words. The clues themselves tend to get trickier as the week goes on, and the answers become less common in the aggregate as well. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is somehwere between a tough Wednesday and an easyish-Thursday. 

**Figure 1** shows a principal components analysis (PCA) of >20 grid, clue and answer-pertinent features obtained from XWord Info. This analysis demonstrates that while puzzles from a give day do indeed aggregate with each other in "puzzle property space", they exist (apart from the much larger Sunday puzzles) they exist along a continuum. The overlapping distributions in GMS times shown in the density plot in **Figure 2** demonstrates a similar phenomenon; that puzzle day is strongly correlated to solve difficulty, but puzzle days of adjacent difficulty have overlapping solve time distributions along with the overlapping puzzle properties seen in **Figure 1**. Overall, GMS times are likely to be at least somewhat faster than the median of the overall solver population. The main reasons for this are that solvers must actually complete an individual puzzle to be included in the calculation yielding GMS for that puzzle, and the pool of solvers for which data are available are a self-selected subset of all solvers who are motivated to track and improve their performance. 

**Figure 1. Principal Components Analysis of Select Grid, Clue and Answer-Pertinent Puzzle Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/cba4a2b1-881f-46b4-8b4d-683905b0a1e8)


*The first 3 principal components accounted for ~48% of variance in the data set. All puzzles for the years 2018-2023 were included in this analysis.*  


**Figure 2. Density Plots of Global Median Solver Solve Times by Puzzle Day**                   
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/23a2f1c4-b468-4d32-8815-31b62d8891f8)


*Note that for other than the "easy" days (Mon and Tue), distributions of GMS solve times were quite broad. Wed and Sat puzzles in particular had a quasi-bimodal distribution of solve times, supporting the notion that there are "easy" and "hard" puzzle pools/constructors even for specific days. All puzzles for the years 2020-2023 were included in this analysis (a more restricted time range was employed here since, as will be seen in the first section of the results, the GMS has improved a great deal since the beginning of the sample (Jan. 1, 2018).*  

## Results
### Global Median Solver (GMS) Performance Over Time

The GMS has improved on each puzzle day over the 6-year sample timeframe. This trend is demonstrated clearly by charting of the puzzle day-specific 10-puzzle moving average over the sample period (**Figure 3**). It must be noted here, however, that because I did not have access to the raw solver data, I had to make the assumption that the GMS (putatively, a different solver for most puzzles in the sample) solved puzzles in more or less the sequence that they were published. The improvement trend can also be clearly seen in year-by-year (per puzzle day) violin plots with swarm plot overlays (**Figure 4**). 

**Figure 3. Day-of-Week Specific 10-Puzzle Moving Average of GMS Solve Times (2018-2023)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/10bff8b8-384d-4658-9af7-99e8d556b166)

**Figure 4. Day-of-Week Specific Violin Plots With Swarm Plot Overlay of GMS Solve Times, Per Year (2018-2023)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/75227929-2389-42e7-868d-f421c50df6dc)
*Violin plots show both the range (vertical extent) as well as distribution characteristics (width as it varies across the y-axis range) for each puzzle day per solve year. Black lines demarcate the quartiles. The swarm plot overlays show individual raw data points (puzzle solve times). Note the reduction in y-axis range from the first row to the second when comparing across the entire sample period.* 

### Global Median Solver (GMS) Performance By Puzzle Constructor(s)

Eighty-one percent of puzzles were created by either solo constructors or constructor teams contributing more than one puzzle over the sample period. This preponderance of repeat constructors affords the opportunity to evaluate which constructors the GMS tends to struggle against and which they tend to do well against. Some constructors specialize in puzzles for one or several days of the week. Measuring GMS performance per-puzzle relative to the day-of-week specific 10-puzzle moving average (as % difference from the moving average; mean % diff from 10-p MA), then averaging this measure for all puzzles in the sample by given constructor(s) yields a single value indicating "constructor difficulty" for GMS that is normalized both for puzzle day and for GMS baseline solve performance improvement over time. **Figure 5** shows heatmapping of GMS performance against the 115 construtor(s) contributing >=5 puzzles over the sample period (encompassing 55% of all puzzles). Warmer colors (-%) indicate faster average solves against given constructor(s) than the mean for recent performance prior to a given solve, when also accounting for puzzle day.   

**Figure 5. Heatmapping of GMS Performance Against Individual Constructor(s) (2018-2023)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/5945e2e1-2afd-45e9-b7c2-1c66b8a8c029)

### Correlations of GMS Performance to Puzzle-Specific Features and Past Performance

Numerous potentially interesting features pertaining to puzzle grids an answers were obtained from XWord Info across the 6-year sample. Features showing strong correlation to GMS solve performance become strong candidates as input features for predictive modeling, in current forms and/or when combined in novel ways with other existing features. **Figure 6** shows correlation heatmapping separately for 15x15 puzzles (Mon-Sat) and 21x21 puzzles (Sun). The Pearson correlation coefficient (PCC) captures linear correlation strength between a given feature and solve times (top row and leftmost column of correlation matrix; red indicates a strong positive correlation and green a strong negative correlation). See **Supplementary Figure 1** for breakdown by individual puzzle days for the 15x15 puzzles. As can also be seen in these correlation matrix figures, a number of grid and/or answer-specific features correlated strongly with each other. For example, 'Mean Answer Length' and 'Freshness Factor' showed a strong negative correlation. This relationship makes intuitive sense because 'Freshness Factor' is a measure of aggregate answer rarity for a given puzzle, and longer answers have a higher likelihood of being uncommon than shorter ones. 

The second-to-last column/row per matrix shows the correlation between GMS solve time (GMST) across individual puzzles and a puzzle-day-specific, time decay-weighted version of the 10-*prior* (to a given puzzle) puzzle moving average. For both 15x15 puzzles and 21x21 puzzles, this (positive) correlation was stronger than any other correlation to GMST.This finding generates a prediction that "recent form" per puzzle day is going to be more predictive of performance on a novel puzzle than is any grid or answer (or clue) feature. The last column/row per matrix shows the correlation between GMST and past performance against the constructor of that puzzle (see previous section for metric). While not as strongly positively correlated to GMST as day-specific past performance, this was still a fairly strong correlation for both 15x15 and 21x21 grid sizes, suggesting that constructor difficulty for the median solver tends to be "sticky" across puzzles.        

**Figure 7** through **Figure N** are companion figures to the correlation heatmapping, and show across all 15x15 puzzle days (black) and by-puzzle-day (colored) scatter plots of features of interest vs raw solve times at the level of individual puzzles (with trend lines indicating aggregate correlation strength). Per feature, a density plot is included to show puzzle day-specific trends in its distribution. A number of features with strong correlations to GMS solve time can be seen here to covary with puzzle day. Subsequent predictive modeling outputs will indicate the relative importance of each of these features to puzzle difficulty.

**Figure 6. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past-Performance Features**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/39073fac-d10f-43ce-be65-cdcdba828448)


#### Regression Scatterplots per Feature vs GMS Solve Times, and Feature Distribution Plots (FDP) by Puzzle Day 


#### *Grid Features*

**Figure 7. Number of Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/5dbfa8ca-365a-4024-b9e1-426995d56d17)

*For 15x15 puzzles, there was a strong negative correlation (r= -0.56) between GMST and # Answers. More answers typically means shorter answers (see correlation matrices above), and shorter answers tend to be be more common/easier answers. This trend is apparent within several of the more difficult puzzle days, especially Sat. The FDP for this feature also demonstrates that the toughest puzzle days (Fri and Sat) tend to have the fewest answers.*

**Figure 8. Open Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/72956055-a1b1-4940-9475-6e8359c629ed)
*For 15x15 puzzles, there was a strong positive correlation (r= 0.59) between GMST and # Open Squares. # Open Squares is a proprietary measure of XWord Info that counts white squares that are not bordered by black squares. More of these squares is strongly positively correlated to Average Answer Length, so it makes sense that more Open Squares is positively correlated with solve times. Interestingly, the correlation of this feature with solve time can be seen with at least moderate strength across all puzzle days. The FDP shows that the most difficult puzzle days (Fri and Sat) have a rightward shift in # Open Squares relative to the easier 15x15 puzzle days.* 

**Figure 9. Black Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/d9599597-cc34-4f31-acb5-6d0d39fa18ee)
*For 15x15 puzzles, there was a moderately strong negative correlation (r= -0.39) between GMST and # Black Squares. No surpise here, since this relationship is essentially the opposite of that between GMST and Open Squares (more black squares = shorter answers = easier answers). As with Open Squares, the trend is very apparent within each of the later week puzzle days and Fri and Sat are prominently shifted away from the earlier week puzzle days in the FDP.*

**Figure 10. Average Answer Length**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/79fad870-1e75-4fe1-9900-daf2a85ca1af)
*For 15x15 puzzles, there was a strong positive correlation (r= 0.67) between GMST and Average Word Length. This finding is consistent with the previous grid feature relationships with GMST, as longer answers means more multiword, relatively-rare answers. The trend is very apparent within each of the puzzle days, and perhaps moreso than any other puzzle feature the sequence in peaks of puzzle day distributions in the FDP tracks with that in mean solve time by puzzle day. Perhaps this is an indication that this feature will be highly predictive of solve time in the modeling phase*

**Figure 11. Cheater Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/eb9b0bb7-2ff5-4eb1-83da-738db2774c17)
*For 15x15 puzzles, there was a weak positive correlation (r= 0.16) between GMST and # Cheater Squares. Cheater Squares, by definition, are black squares than can be removed without affecting the overall word count of the grid. While these make construction easier (hence the name) it's not entirely apparent what their net effect on puzzle difficulty would be. Most of the individual puzzle days appear to show a weak negative correlation, and that would make sense in terms of more cheater squares = more black squares. However, the individual day regression lines (the all 15x15 regression line is driven by a few out-of-range points) appear to be driven by the small number of puzzles per puzzle day with a large number of cheater squares. It's possible that there's a non-linear effect here, with smaller (but non-zero) numbers of cheater squares allowing for difficult puzzle construction trickery, but beyond a certain threshold the sheer number of black squares has a counterbalancing effect.*

#### *Grid Features*

**Figure 12. Fill-in-the Blank**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/c0cae7b8-9adf-4d78-8ddd-21964a52c0df)
*For 15x15 puzzles, there was a moderate negative correlation (r= -0.30) between GMST and # Fill-in-the-Blank (FITB) answers. Taken together, the feature distribution plot and the scatterplots indicate that most of this correlation is related to the fact that the easiest puzzles (note the rightward peak shift for Mon even relative to Tue in the FDP) employ a heavy dose of FITB answers. It will be interesting to see how important this feature is in the modeling phase to prediction of early week solve times specifically.*

**Figure 13. Scrabble Average**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/4d104dab-b515-4a11-98f6-038c1ba6ed68)
*For 15x15 puzzles, there was a weak negative correlation (r= -0.03) between GMST and Scrabble Average. Scrabble Average is another proprietary XWord Info measure, in which each letter in the answer grid is assigned its equivalent value in Scrabble. Since tile values in Scrabble increase with rarity of letter frequency in English texts, it would make sense that a higher Scrabble average would be associated with *answers* of greater rarity. If anything, the opposite is true in practice here and as can be seen in the next few figures there are direct measures of answer rarity that do have strong correlations to solve times. So this one is a candidate to either be left out of predictive modeling entirely or to potentially be combined with other answer rarity/difficulty measures to generate a useful predictive feature.*

**Figure 14. Scrabble Illegal Answers**

*For 15x15 puzzles, there was a moderate positive correlation (r= 0.21) between GMST and # of Scrabble Illegal answers. # of Scrabble Illegal answers is another proprietary measure of XWord Info, that gets a answer rarity more directly than Scrabble Aveage does (though not as directly as the next two measures). Interestingly this moderate positive correlation is seen both across all 15x15 puzzles and within each puzzle day. Also interesting is that, apart from a Monday relative leftward shift in the FDP, the distributions for the other 15x15 puzzle days are highly overlapping. It might be surmised that the days with more open squares and longer average answers would have more substantially more answers that are not standard English vocabulary words. This finding suggests that more non-standard vocabulary alone may not signify puzzle difficulty.*  
