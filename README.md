# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle
## Introduction
### Project Goal and Data Sources
This exploratory data analysis (EDA) of the most recent 6 years (2018-2023) of Global Median Solver (GMS) performance on the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords) resulted in discovery of numerous features with significant correlation to GMS performance at the individual puzzle level. Encompassing solver, constructor, grid, clue and answer characteristics, this set of identified features will be further refined and expanded upon to serve as inputs to a forward-predictive model of GMS performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info has a contract with NYT for access to the raw data underlying these metrics, but I unfortunately do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site (though [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks)). The second, [XWStats](xwstats.com), was my source for historical GMS solve time data. Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K solves recorded for them. It is from these raw solve times for each puzzle that he has calculated the raw global median solve times (GMSTs), as well as the difference in GMST from the 10-puzzle moving average of the median solver for a given puzzle. These two metrics serve as the GMS performance variables in the present analysis. I do not have the underlying raw data for solvers in the database apart from my own (Individual Solver 1) and that of one other person (Individual Solver 2); both of whom are subjects of similar analyses. Please explore and strongly consider supporting both of these wonderful data source sites financially; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Crossword and GMS Characteristics
The NYT crossword has been published since 1942, and many consider its modern era to have started with the arrival of Will Shortz as (only) its 4th editor 30 years ago. A new puzzle for each day is published online at either 6 PM (Sunday and Monday puzzles) or 10 PM (Tuesday-Saturday puzzles) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thu generally including a gimmick or trick of some sort (e.g., "rebuses" where the solver must enter more than one letter into one or more squares). Additionally, early week puzzles generally have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions and longer, often multi-word, answers than the early week puzzles. The clue sets tend to be more wordplay heavy as the week goes on, and the answers become less common in the aggregate as well. Sun puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is somehwere between a tough Wednesday and an easy Thursday. 

**Figure 1** shows dimensionality reduction via Principal Component Analysis (PCA) of 21 grid, clue and answer-pertinent features obtained from XWord Info. This analysis demonstrates that, while puzzles from a given day do indeed aggregate with each other in n-dimensional "puzzle property space", they nonetheless exist along a continuum (Sunday is separated from other days by PCA1, no doubt in some relation to the larger grid size). The overlapping distributions in GMSTs shown in the solve time density plot in **Figure 2** shows a similar phenomenon; namely that while solve difficulty increases as the week progresses, puzzle days of adjacent difficulty have substantially overlapping GMS solve time distributions. 

One contextual note about the GMS is worth mention upfront. Though Matt from XWStats uses the word "Global", and I adopt it as well, it is highly likely that the sample from which the GMST is pulled per-puzzle skews faster than the true population sample. The probable main reasons for this are that the sample does not contain solvers who fail to complete a given puzzle (a % that likely increases through the week's puzzle days), and that the sample contains only solvers motivated enough by the prospect of improvement to track their own progress. Bear this is mind, possibly as a way to be gentle on yourself, if comparing your own performance to that of the GMS. 

**Figure 1. PCA of Select Grid, Clue and Answer-Pertinent Puzzle Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/e3dd8622-0d4e-445f-88d1-ecd5eb8347f0)

*The first 3 principal components accounted for ~48% of variance in the data set. All puzzles for the years 2018-2023 were included in this analysis.*  



**Figure 2. Distribution of GMSTs by Puzzle Day**                   
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/41449f0d-4aee-475a-b126-6565d1c12724)



*Note that for other than the "easy" days (Mon and Tue), distributions of GMSTs were quite broad. Wed and Sat puzzles, in particular, had quasi-bimodal distributions of solve times, supporting the notion that there are "easy" and "hard" puzzle pools/constructors at the level of specific puzzle days. All puzzles for the years 2020-2023 were included in this specific figure (the GMS improved so much in 2018-2019 that individual solve day distribution broaden out even further with their inclusion; see Figs. 3 and 4).*  

## Results
### Global Median Solver (GMS) Performance Over Time

The GMS solve time (GMST) improved on each puzzle day over the 6-year sample timeframe, which is demonstrated clearly by puzzle day-specific plotting of the 10-puzzle moving average across this period (**Figure 3**). It must be noted here, however, that because I did not have access to the raw solver data, I had to make the assumption that the GMS (putatively, a different solver for most or all puzzles in the sample) solved puzzles in approximately the same sequence in which they were published. The improvement trend can also be clearly seen in year-by-year (per puzzle day) violin plots with swarm plot overlays (**Figure 4**). 

**Figure 3. Day-of-Week Specific 10-Puzzle Moving Average of GMSTs**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/e9e634e9-5f61-4dfc-b443-85f5a4da39be)


**Figure 4. Day-of-Week Specific Violin Plots With Swarm Plot Overlay of GMSTs, Per Year**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b647aa52-b83e-4225-8dc1-01a241b3cf69)
*Violin plots show both the range (vertical extent) as well as distribution characteristics (width as it varies across the y-axis range) for each puzzle day, per year. Black lines demarcate the quartiles. The swarm plot overlays show individual data points (puzzle raw solve times). Note the reduction in y-axis range maximum from the first row to the second when comparing solve times across the entire sample period.* 

### GMS Performance By Puzzle Constructor(s)

Eighty-one percent of puzzles were created by either solo constructors or constructor teams contributing more than one puzzle over the sample period. This preponderance of repeat constructors afforded the opportunity to evaluate which constructors the GMS tended to struggle against and which they tended to do well against. Some constructors specialize in puzzles for one or several days of the week. Measuring GMS performance per-puzzle relative to the day-of-week specific 10-puzzle moving average (as % difference from the moving average; mean % diff from 10-p MA), then averaging this measure for all puzzles in the sample by given constructor(s), yields a single value indicating "constructor difficulty" for GMS that is normalized both for puzzle day and for GMS baseline solve performance improvement over time. **Figure 5** shows heatmapping of GMS performance against the 115 constructor(s) contributing >=5 puzzles over the sample period (encompassing 55% of all puzzles). Warmer colors (-%) indicate faster average solves against given constructor(s) than the mean for recent performance prior to a given solve, when also accounting for puzzle day.   

**Figure 5. Heatmapping of GMS Performance Against Individual Constructor(s)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a9caf4ac-c73e-4332-9756-b304405933de)


### Correlations of GMS Performance to Puzzle-Specific Features and Past Performance

Numerous potentially interesting features pertaining to puzzle grids and answers were obtained from XWord Info across the 6-year sample. Features showing strong correlation to GMS solve performance become strong candidates as input features for predictive modeling, in current forms and/or when combined in novel ways with other existing features. **Figure 6** shows correlation heatmapping separately for 15x15 puzzles (Mon-Sat) and 21x21 puzzles (Sun). The Pearson correlation coefficient (PCC) captures linear correlation strength between a given feature and solve times (top row and leftmost column of correlation matrix; red indicates a strong positive correlation and green a strong negative correlation). See **Supplementary Figure 1** for breakdown by individual puzzle days for the 15x15 puzzles. As can also be seen in these correlation matrix figures, a number of grid and/or answer-specific features correlated strongly with each other. For example, 'Mean Answer Length' and 'Freshness Factor' showed a strong negative correlation. This relationship makes intuitive sense because 'Freshness Factor' is a measure of aggregate answer rarity for a given puzzle, and longer answers have a higher likelihood of being uncommon than shorter ones. 

The second-to-last column/row per matrix shows the correlation between GMST across individual puzzles and a puzzle day-specific, time decay-weighted version of the 10-*prior* (to a given puzzle) puzzle moving average. For both 15x15 puzzles and 21x21 puzzles, this (positive) correlation was stronger than any other correlation to GMST. This finding generates a prediction that recent (relative to a puzzle date to be predicted) form per puzzle day is going to be more predictive of performance on a novel puzzle than is any grid or answer (or clue) feature. The last column/row per matrix shows the correlation between GMST and past performance against the constructor of that puzzle (see previous section for metric). While not as strongly positively correlated to GMST as day-specific past performance, this was still a fairly strong correlation for both 15x15 and 21x21 grid sizes, suggesting that constructor difficulty for the median solver tended to be "sticky" across puzzles.        

**Figure 7** through **Figure 18** are companion figures to the correlation heatmapping, and show across all 15x15 puzzle days (black) and by-puzzle-day (colored) scatter plots of features of interest vs raw solve times at the level of individual puzzles (with trend lines indicating aggregate correlation strength). A feature distribution density plot (FDP), per feature, shows puzzle day-specific trends in its distribution. A number of features with strong correlations to GMST can be seen here to covary with puzzle day. Subsequent predictive modeling outputs will quantify the relative importance of each of these features to puzzle difficulty.

**Figure 6. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past-Performance Features**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/39073fac-d10f-43ce-be65-cdcdba828448)


#### Regression Scatterplots for Individual Features vs GMSTs, with Associated Feature Distribution Density Plots (FDPs)


#### *Grid Features*

**Figure 7. Number of Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/21f0bc4b-fb7f-4fc2-8b79-458db8b440eb)
*For 15x15 puzzles, there was a strong negative correlation (r= -0.56) between GMST and # Answers. More answers typically meant shorter answers (see correlation matrices above), and shorter answers tended to be be more common/easier answers (see Average Answer Length and Freshness Factor analyses below). The correlation strength, and even its directionality, varied across puzzle days. However, the FDP for this feature shows that the toughest puzzle days (Fri and Sat) tended to have the fewest answers (and the Sat trend mirrored the overall 15x15 trend).*

**Figure 8. Number of Open Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/38d2f2e0-9583-44a0-8816-b37c59a91ad9)
*For 15x15 puzzles, there was a strong positive correlation (r= 0.59) between GMST and # Open Squares. # Open Squares is a proprietary measure from XWord Info that counts all white squares that are *not* bordered by black squares. # Open Squares was strongly positively correlated to Average Answer Length (see matrices above), so it makes sense that more Open Squares was also positively correlated with solve times. Interestingly, the correlation of this feature with solve time can be seen with at least moderate strength across all puzzle days. The FDP shows that the most difficult puzzle days (Fri and Sat) had a rightward shift in # Open Squares relative to the easier 15x15 puzzle days.* 

**Figure 9. Number of Black Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/cc44b2d2-fac9-41d7-a522-2bdcaa98bc13)
*For 15x15 puzzles, there was a moderately strong negative correlation (r= -0.39) between GMST and # Black Squares. No surpise here, since this relationship was essentially the opposite of that between GMST and Open Squares (more black squares = shorter answers = easier answers). As with Open Squares the trend was very apparent in the FDP within each of the later week puzzle days, and Fri and Sat were prominently shifted away from the earlier week puzzle days.*

**Figure 10. Average Answer Length**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/80cbf9b0-a8e1-447b-8f56-3e98360d8e30)
*For 15x15 puzzles, there was a strong positive correlation (r= 0.67) between GMST and Average Answer Length. This finding was consistent with other grid feature relationships with GMST, as longer answers means more multiword and relatively-rare answers (see correlation matrices above and Figs. 14-16). This correlation was very apparent within each of the puzzle days, and perhaps moreso than any other puzzle feature, the sequence in peaks of puzzle day distributions in the FDP tracked with that in mean GMST by puzzle day. Perhaps this is an indication that this feature will be highly predictive of solve time in the modeling phase??*

**Figure 11. Number of Cheater Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f0085fe5-f4fb-4a6f-98cd-62b894befe77)
*For 15x15 puzzles, there was a weak positive correlation (r= 0.16) between GMST and # Cheater Squares. Cheater Squares, by definition, are black squares than can be removed without affecting the overall word count of the grid. These square make construction easier (hence the name), and it can be seen that large numbers of them (say, >10) almost always appear on the difficult puzzle days. Interestingly, within a given puzzle day, however, it's clear that puzzles with larger numbers of them tended to be easier for the GMS. So the seeming paradox between the overall 15x15 trend and the individual puzzle day trends is likely related to the competing effects of cheater squares allowing trickier constructions, but also reducing the number of answers and answer lengths overall. Incidentally, the reason they were only rarely seen in odd numbers is the only rarely-violated NYT requirement for grid symmetry.*

#### *Answer Features*

**Figure 12. Number of Fill-in-the-Blank Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/585982a9-c047-4dc7-8c5a-dd306baa78e3)
*For 15x15 puzzles, there was a moderate negative correlation (r= -0.30) between GMST and # Fill-in-the-Blank (FITB) answers. Taken together, the FDP and the scatterplots indicate that most of this correlation was related to the fact that the easiest puzzles (note the rightward FDP peak shift for Mon, even relative to Tue) employed a heavy dose of FITB answers. It will be interesting to see how important this feature is in the modeling phase to prediction of early week solve times, specifically.*

**Figure 13. Scrabble Average**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f5fced88-58b1-4b10-8028-b5e3ef456dec)
*For 15x15 puzzles, there was a weak negative correlation (r= -0.03) between GMST and Scrabble Average. Scrabble Average is another proprietary XWord Info measure, in which each letter in the answer grid is assigned its equivalent value in Scrabble. Since tile values in Scrabble increase with rarity of letter frequency in English texts, it would make sense that a higher Scrabble average would be associated with *answers* of greater rarity. If anything, the opposite was true in practice here and as can be seen in the next few figures there are direct measures of answer rarity that *do* have strong positive correlations to solve times. So this one is a candidate to either be left out of predictive modeling entirely or to be combined with other answer rarity/difficulty measures to generate a useful predictive feature.*

**Figure 14. Number of Scrabble Illegal Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/43739da0-4578-4004-8212-33d984719366)
*For 15x15 puzzles, there was a moderate positive correlation (r= 0.21) between GMST and # Scrabble Illegal answers. # Scrabble Illegal answers is a proprietary measure of XWord Info that gets at answer rarity more directly than does Scrabble Average (though not as directly as the measures in Fig. 15 and Fig. 16). Interestingly this moderate positive correlation was seen both across all 15x15 puzzles and within each puzzle day. Also interesting is that, apart from a Mon relative leftward shift in the FDP, the distributions for the other 15x15 puzzle days were highly overlapping. I had assumed that the days with more open squares and longer average answers would also have substantially more answers that are not standard English vocabulary words. This finding suggests that more non-standard vocabulary *alone* may not signify or predict puzzle difficulty.*  

**Figure 15. Number of Unique Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/39bc5dbd-a68a-403e-a3b6-86c663c3c619)
*For 15x15 puzzles, there was a moderately strong positive correlation (r= 0.39) between GMST and # Unique Answers. A Unique Answer is one that does not appear in any other NYT crossword puzzle in either Shortz or pre-Shortz eras (either before or after the puzzle release date). This is perhaps an overly stringent criterion to define answer rarity (see Fig. 16 for a graded approach to defining answer rarity). Nonetheless, the positive correlation was clearly apparent when considering all 15x15 puzzles together and also especially within the most challenging puzzle day (Sat). It is also clear in the FDP that # Unique Answers tended to increase as puzzle day difficulty increased.*


**Figure 16. Freshness Factor**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/876063d8-dee3-4369-b5bb-693c68ecdc12)
*For 15x15 puzzles, there was a strong positive correlation (r= 0.67) between GMST and Freshness Factor. Freshness Factor is yet another, and very possibly the most useful for prediction, proprietary XWord Info measure that assesses the aggregate relative novelty of all answers in a given crossword puzzle as compared to those in all other crossword puzzles in the NYT archive. The much stronger correlation to GMSTs as compared to that of # Unique Answers suggests that there's much to be gained by taking a graded, as opposed to all-or-none, approach in assessing answer rarity. As with Average Answer Length, it can be seen in the FDP that puzzle days peaked in this measure in close concordance with the per-day sequence for solve times (interesting little second hump for Mon close to the Tue peak, however)* 

#### *Past Performance Features*

**Figure 17. GMS Adjusted Recent Performance**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/158926a9-5829-4d27-a49e-18708598ffeb)
*For 15x15 puzzles, there was a very strong positive correlation (r= 0.84) between GMST and GMS Adjusted Recent Performance (GMS-ARP). To obtain GMS-ARP for a given puzzle the 10 most recent *prior* puzzles from the same puzzle day were averaged after first being decay weighted (10 for the most recent prior puzzle, 9 for the one before that and so on down to a weight of 1 for the 10th prior puzzle). Recent past performance across all 15x15 puzzles by this measure was more strongly correlated to performance on the "next" puzzle than the specific characteristics of that puzzle.*

*Very interestingly, correlation strengths for the early week 15x15 days (Mon=.54, Tue=.51, Wed=.45) were higher than those for the later week days (Thu=.36, Fri=.41, Sat=.38). This attests to the relative heterogeneity of later week puzzles, and the likely fact that the likelihood of a solver getting stuck in one particular spot for an extended period of time goes way up. I have zero actual evidence for this, but I will further speculate as an experienced solver that Thu puzzles have the lowest correlation of all because they are the most heterogeneous of all puzzle days due to the varied gimmicks and tricks employed.*

**Figure 18. Past Peformance Against Constructor(s)**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/afe4f398-1ba1-4973-b1a1-456d98aa5091)
*For 15x15 puzzles, there was a moderate positive correlation (r= 0.20) between GMS solve time and GMS Past Performance Against Constructor(s). This correlation (see text above Fig. 5 for description of the Constructor metric) was seen across all puzzle days in roughly the same strength. This result provides at least some optimism that past performance against given constructor(s) will have predictive power above and beyond the features associated with a given puzzle; ie; that knowledge of Constructor(s) will contain a je ne sais quoi that may be hard to capture via reductionism. Interestingly, the correlation was the weakest by a considerable amount on Mon (0.16), which may attest to the "generic" nature of that day's puzzle.*


# Supplementary Figures

**Supplementary Figure 1. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past-Performance Features By Puzzle Day (15x15 Puzzle Days)**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/3470636d-d701-412e-a803-b0cd4bb6ae9c)

