# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle
## Introduction and Data Sources
This exploratory data analysis (EDA) of Global Median Solver (GMS) performance on the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords) will serve as a key step toward the goal of building a predictive model of GMS performance on future NYT puzzles. This analysis has yielded the discovery of GMS and constructor performance features, as well as numerous puzzle-structure, clue and answer-related features, with significant correlation to GMS performance on individual puzzles. These identified features will be further refined to serve as inputs into a forward-predictive model of GMS performance. 

The current project relies crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), is my source for data on the puzzles themselves. This includes a number of proprietary metrics of the grids, answers, clues and constructors. They have a contract with NYT for access to the data underlying these metrics, and I do not. Because of this, I will not be able to share raw or processed data that I've acquired from their site. The second, [XWStats](xwstats.com), is my source for historical GMS data. Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K solves recorded for them. It is from these raw solve times for each puzzle that he has extracted the raw global median solve time (GMST), as well as the deviation of GMST from the 10-puzzle moving average of the median solver for a given puzzle. These two metrics serve as the performance variables in the present EDA. I do not have the underlying raw data for solvers in the database apart from my own (Individual Solver 1) and that of one other person (Individual Solver 2); both of whom are the basis of similar exploratory data analyses. Please strongly consider supporting both of these wonderful sites financially; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats).          
