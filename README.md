# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle
## Introduction
### Project Goal and Data Sources
This exploratory data analysis (EDA) of Global Median Solver (GMS) performance on the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords) will serve as a key step toward the goal of building a predictive model of GMS performance on future NYT puzzles. This analysis yielded the discovery of GMS and constructor performance features, as well as numerous puzzle-structure, clue and answer-related features, with significant correlation to GMS performance on individual puzzles. These identified features will be further refined to serve as inputs into a forward-predictive model of GMS performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info have a contract with NYT for access to the raw data underlying these metrics, and I do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site (though Jupyter notebooks with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks). The second, [XWStats](xwstats.com), is my source for historical GMS data. Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K solves recorded for them. It is from these raw solve times for each puzzle that he has extracted the raw global median solve time (GMST), as well as the deviation of GMST from the 10-puzzle moving average of the median solver for a given puzzle. These two metrics serve as the performance variables in the present EDA. I do not have the underlying raw data for solvers in the database apart from my own (Individual Solver 1) and that of one other person (Individual Solver 2); both of whom are the basis of similar exploratory data analyses. Please strongly consider supporting both of these wonderful sites financially; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Puzzle Properties and Solver Performance
The NYT crossword has been published since 1942, and many consider its modern era to have started with the arrival of Will Shortz (as only its 4th editor ever) 30 years ago. A new puzzle is published online each day; actually at either 6 or 10 PM EST the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick of some sort (e.g., rebuses where the solver must enter more than one letter into one or more squares). Additionally, the early week puzzles generally have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions than the earlier week puzzles. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is somehwere between a tough Wednesday and an easyish-Thursday. **Figure 1** shows a principal components analysis (PCA) of >20 grid, clue and answer features obtained from XWord info (all puzzles between January 1, 2018 and December 26, 2023 are included). It can be seen through this attempt at     
