# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle
## Introduction
### Project Goal and Data Sources
This exploratory data analysis (EDA) of Global Median Solver (GMS) performance on the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords) will serve as a key step toward the goal of building a predictive model of GMS performance on future NYT puzzles. This analysis yielded the discovery of GMS and constructor performance features, as well as numerous puzzle-structure, clue and answer-related features, with significant correlation to GMS performance on individual puzzles. These identified features will be further refined to serve as inputs into a forward-predictive model of GMS performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info have a contract with NYT for access to the raw data underlying these metrics, and I do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site (though [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks). The second, [XWStats](xwstats.com), is my source for historical GMS data. Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K solves recorded for them. It is from these raw solve times for each puzzle that he has extracted the raw global median solve time (GMST), as well as the deviation of GMST from the 10-puzzle moving average of the median solver for a given puzzle. These two metrics serve as the performance variables in the present EDA. I do not have the underlying raw data for solvers in the database apart from my own (Individual Solver 1) and that of one other person (Individual Solver 2); both of whom are the basis of similar exploratory data analyses. Please strongly consider supporting both of these wonderful sites financially; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Puzzle Properties and Solver Performance
The NYT crossword has been published since 1942, and many consider its modern era to have started with the arrival of Will Shortz (as only its 4th editor ever) 30 years ago. A new puzzle for each day is published online at either 6 PM (weekend) or 10 PM (weekend) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick of some sort (e.g., rebuses where the solver must enter more than one letter into one or more squares). Additionally, the early week puzzles generally have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions than the earlier week puzzles. Along with the open construction comes longer answers, often consisting of multiple words. The clues themselves tend to get trickier as the week goes on, and the answers become less common in the aggregate as well. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is somehwere between a tough Wednesday and an easyish-Thursday. 

**Figure 1** shows a principal components analysis (PCA) of >20 grid, clue and answer-pertinent features obtained from XWord Info. This analysis demonstrates that while puzzles from a give day do indeed aggregate with each other in "puzzle property space", they exist (apart from the much larger Sunday puzzles) they exist along a continuum. The overlapping distributions in GMS times shown in the density plot in **Figure 2** demonstrates a similar phenomenon; that puzzle day is strongly correlated to solve difficulty, but puzzle days of adjacent difficulty have overlapping solve time distributions along with the overlapping puzzle properties seen in **Figure 1**.  

**Figure 1. Principal Components Analysis of Select Grid, Clue and Answer-Pertinent Puzzle Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/cba4a2b1-881f-46b4-8b4d-683905b0a1e8)


*The first 3 principal components accounted for ~48% of variance in the data set. All puzzles for the years 2018-2023 were included in this analysis.*  



**Figure 2. Density Plots of Global Median Solver Solve Times by Puzzle Day**                   
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/23a2f1c4-b468-4d32-8815-31b62d8891f8)


*Note that for other than the "easy" days (Mon and Tue), distributions of GMS solve times were quite broad. Wed and Sat puzzles in particular had a quasi-bimodal distribution of solve times, supporting the notion that there are "easy" and "hard" puzzle pools/constructors even for specific days. All puzzles for the years 2020-2023 were included in this analysis (a more restricted time range was employed here since, as will be seen in the first section of the results, the GMS has improved a great deal since the beginning of the sample (Jan. 1, 2018).*  

## Results
### Global Median Solver (GMS) Performance Over Time

The GMS has improved on each puzzle day over the 6-year sample timeframe. This trend is demonstrated clearly by charting of the puzzle day-specific 10-puzzle moving average over the sample period (**Figure 3**). It must be noted here, however, that because I did not have access to the raw solver data, I had to make the assumption that the GMS (putatively, a different solver for most puzzles in the sample) solved puzzles in more or less the sequence that they were published. The improvement trend can also be clearly seen in year-by-year (per puzzle day) violin plots with swarm plot overlays (**Figure 4**). 

**Figure 3. Day-Specific 10-Puzzle Moving Average of Global Median Solver Solve Times (2018-2023)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/10bff8b8-384d-4658-9af7-99e8d556b166)

**Figure 4. Day-Specific Violin Plots With Swarm Plot Overlay of Global Median Solver Solve Times Per Year (2018-2023)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0e166911-a1a3-48dd-aa9a-1148b5054881)
*Violin plots show both the range (vertical extent) as well as distribution characteristics (width as it varies across the y-axis range) for each puzzle day per solve year. Black lines demarcate the quartiles. The swarm plot overlays show individual raw data points (puzzle solve times).* 



