# Exploratory Data Analysis of "Global Median Solver" (GMS) Performance on the New York Times Crossword Puzzle

## Introduction

### Project Overview and Data Sources
This summary reports on exploratory data analysis (EDA) of "Global Median Solver" (GMS) performance over 6+ years (Jan. 2018 - Jan. 2024) of the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords). Included are visual and statistical descriptions of trends in GMS solve times across this period, and the relationship between GMS performance and a number of variables. These variables included properties of the puzzle grids (e.g., number of answers, number of black squares), clues and answers (e.g., frequency of wordplay in clues, aggregate rarity of answers in a puzzle), constructor identity, and puzzle day-specific recent past performance prior to a given solve. This EDA led to identification of a set of features that hold promise as useful inputs to a predictive model of GMS future performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info has a contract with NYT for access to the raw data underlying these metrics, but I unfortunately do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site. Nonetheless, [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks)). The second, [XWStats](xwstats.com), was my source for historical GMS raw solve time data. From these raw solve times for each puzzle, XWStats (Matt) calculates global median solve times (GMSTs). Thus, the GMS is a composite of many different individuals who just so happened to fall at the 50th percentile on (at least) one particular puzzle's solve time distribution. Per personal communication with Matt, most puzzle dates have somewhere between 1-2K individual solver solve times recorded (with consent from each individual solver). I do *not* have the underlying raw data for solvers in the database apart from that for two experienced solvers, both of whom are subjects of similar analyses; [Individual Solver 1(IS1)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-1/blob/main/README.md) and [Individual Solver 2(IS2)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-2/blob/main/README.md). 

Please visit, explore and strongly consider financially supporting both of these wonderful sites; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Crossword and GMS Characteristics
The NYT crossword has been published since 1942, and many consider the "modern era" to have started with the arrival of Will Shortz as (only) its 4th editor 30 years ago. A new puzzle for each day is published online at either 6 PM (Sunday and Monday puzzles) or 10 PM (Tuesday-Saturday puzzles) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick or trick of some sort (e.g., "rebuses" where the solver must enter more than one character into one or more squares). Additionally, nearly all Sunday through Thursday puzzles have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions and longer (often multiword) answers than the early week puzzles. The clue sets tend to be more wordplay heavy/punny as the week goes on, and the answers become less common in the aggregate as well. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is approximately somewhere between a tough Wednesday and an easy Thursday. 

**Figure 1** shows dimensionality reduction via Principal Component Analysis (PCA) of 23 grid, clue and answer-related features obtained from XWord Info (see **Supplementary Table 1** for a full list of included features). This analysis demonstrates that, while puzzles from a given puzzle day do indeed aggregate with each other in n-dimensional "puzzle property space", the puzzle days themselves nonetheless exist along a continuum. Sunday is well-separated from the other puzzle days in this analysis by PCA1, which undoubtedly incorporates one or more grid size-contingent features.   

**Figure 1. PCA of Select Puzzle Grid, Clue and Answer Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/2a64ecf0-29cf-4096-bcd4-32b15671269a)
*<h5>The first 3 principal components accounted for 47.6% of total variance. All puzzles from Jan. 1, 2018- Jan. 27, 2024 were included in this analysis (N=2,218).*
###
The overlapping distributions in GMSTs in the solve time density plot in **Figure 2** show a parallel performance phenomenon to the continuum of puzzle properties seen in **Fig. 1**; namely that while solve difficulty increased as the week progressed, puzzle days of adjacent difficulty had substantially overlapping GMST distributions. Other than for the "easy" days (Monday and Tuesday), distributions of GMSTs were quite broad. Wednesday and Saturday also had somewhat multimodal solve time distributions, supporting the notion that there were "easy" and "hard" puzzle pools/constructors at the level of specific puzzle days. The broadness of each puzzle day-specific GMST distribution over the entire sample timeframe depicted here (2018-2024) was also increased by the fairly dramatic improvement in GMS performance over those 6+ years. The temporal dynamics of this improvement will be highly evident in the next section's figures.     

One additional contextual note about the GMS is worth mention upfront. Though Matt from XWStats uses the word "global", and I adopt it as well, it is highly likely that the sample from which the GMST is pulled per puzzle skews faster than the true population distributions. The reasons for this assumption are twofold; only solvers who actually complete a given puzzle are included in its sample, and each sample contains only solvers motivated enough by the prospect of improvement to track their own progress to begin with.

**<h4>Figure 2. Distributions of GMSTs by Puzzle Day for Full Sample Period**                   

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/6878dcf4-3805-4342-9758-acba67ae9a84)
*<h5>All puzzles from Jan. 1, 2018- Jan. 27, 2024 were included in this analysis (N=2,218).* 


## Results
### Global Median Solver (GMS) Performance Over Time

The GMS solved N = 2,218 puzzles in the full sample period (365 per year, except 2020: 366 and partial year 2024: 27). The total solve time for the GMS was 31.9 days (2018: 6.8; 2019: 5.8; 2020: 5.2; 2021: 4.8; 2022: 4.7; 2023/24: 4.6). Note that GMS performance was tracked by puzzle *issue* date, as I did not have access to GMS puzzle completion dates. It's reasonably safe to assume, however, that the GMS (a different individual solver for most puzzles, presumably) solved in approximately the sequence of puzzle issue. Individual solver (IS1 and IS2) performance (see links in Introduction), in contrast, was tracked by puzzle *completion* date since I *was* able to obtain completion timestamps for those solvers' completed puzzles with Matt's assistance.

GMSTs improved on each puzzle day over the full sample period (**Figure 3**). This improvement was fairly dramatic in the first few years for some puzzle days (most prominently for Sun), and graded improvement continued for each puzzle day until the end of the sample period (top panel). These improvement dynamics can also be seen in the aggregate raw solve time per year data reported above. The 2-year interval density plots of raw solve time distributions (bottom panels) show that performance on individual puzzle days became more consistent over time (higher peaks with narrower distributions). Because I did not have access to the raw solver data from which the GMSTs were drawn, however, it was not possible to disentangle improvement and increased consistency for individual "early adopters" of Matt's tracking software versus stronger solvers joining the solver pool over time.  

**Figure 3. Solve Time Overview by Puzzle Day: 10-Puzzle Moving Averages and Distributions of Raw Values**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/4ab29e1e-2e1c-4f93-8f36-65d3fb8d5751)
*<h5>GMS Final (as of Jan. 27, 2024) 10-puzzle moving average of solve time (m), per puzzle day:*<br>
*Sun: 28.8, Mon: 5.7, Tue: 7.7, Wed: 11.4, Thu: 15.2, Fri: 17.3, Sat: 21.2*<br>

###
**Figure 4** shows the GMS solve time performance trajectory in violin plots with swarm plot overlays, broken out by 2-year (2+ for 2023/24) solve date intervals. Violin plots show both the range (vertical extent) and distribution characteristics (width as it varies across the y-axis range) for each puzzle day, per solve interval. Black lines on the violin plots demarcate solve time quartiles per puzzle day. Swarm plot overlays per puzzle day show individual puzzle raw solve times. The narrow geometries of the later week violin plots relative to the earlier week ones correspond to greater relative performance variability on those solve days. This phenomenon will be discussed in the final section of the summary in the context of correlation between past and future performance and prospects for predictive modeling. 


**Figure 4. Solve Time Overview by Puzzle Day: Violin Plots with Swarm Plot Overlay**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/6e866ea5-9778-4819-ba5b-3ad739440ef4)
*<h5>Median[IQR] solve time (m), per puzzle day, per solve interval:*<br>
*2018/2019: Sun: 50.0[42.3-60.0], Mon: 8.6[7.6-9.8], Tue: 12.1[9.5-14.3], Wed: 16.5[13.1-20.1], Thu: 24.6[20.2-29.0], Fri: 24.7[21.6-29.5], Sat: 31.6[25.1-36.2]*<br>
*2020/2021: Sun: 37.1[32.2-41.5], Mon: 7.1[6.4-7.9], Tue: 9.5[8.2-10.9], Wed: 12.7[10.8-15.2], Thu: 20.3[17.6-24.1], Fri: 20.7[18.6-23.9], Sat: 26.9[22.8-31.2]*<br>
*2022-2024: Sun: 33.2[28.6-37.7], Mon: 6.0[5.6-6.4], Tue: 8.3[7.5-9.6], Wed: 11.8[10.0-13.9], Thu: 18.4[16.1-21.1], Fri: 18.8[16.9-21.4], Sat: 24.0[20.4-28.9]*  


### GMS Performance By Puzzle Constructor(s)

A high proportion of puzzles solved by the GMS (81.7%) were authored by either repeat individual constructors or repeat specific constructor teams (both referred to as "constructor" from here forward). This afforded the opportunity to evaluate which constructors the GMS tended, in a relative sense, to struggle against or do well against. Per constructor, the mean of % difference from 'recent performance baseline' (RPB) across all puzzles in the sample that they authored was computed. To compute RPB itself, each raw solve time was taken as a % difference from a decay-time weighted average of the *previous* 20 puzzles solved on the same puzzle day. Thus, taking the mean of this metric per constructor isolated "constructor difficulty" for the GMS via adjustment for both GMS "recent form" and variability in the mix of puzzle days for puzzles authored by each constructor.  

**Figure 5** shows heatmapping of GMS performance, using this normalized measure (RPB), against the n=115 constructors contributing >=5 puzzles over the sample period. While only 15.5% of constructors contributed this many puzzles, this group contributed 54.8% of all puzzles solved by the GMS. Warmer colors (-%) indicate that the GMS solved relatively fast against a given constructor; cooler colors (+%) indicate the opposite. At this puzzle number threshold, the mean difference from RPB for GMS solve time against different constructors ranged from -29.7% ("easiest constructor" Yacob Yonas) to 29.2% ("hardest constructor" Jules Markey).   
      
**Figure 5. Heatmapping of GMS Performance Against Individual Constructors**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/bc304a83-a8f3-4072-835b-6bf81b468331)

The next correlational analysis was aimed at assessing the potential predictive value of the GMS' baseline-adjusted past performance against specific constructors on GMS future performance against the same constructor. **Figure 6** shows this correlation between GMS baseline-adjusted performance (as mean % difference from RPB) against a given constructor (x-axis) and performance on the next solved individual puzzle by that constructor (y-axis). The scatterplot in the left panel includes all n=1470 puzzles in the overall sample that were issued (and solved) *after* >=1 previous puzzles by the same constructor. The scatterplot on the right has a higher threshold for inclusion; the n=749 puzzles in the overall sample that were issued (and solved) *after* >=4 previous puzzles by the same constructor. Thus, at this higher threshold only puzzles by constructors included in **Fig. 5** are included, and only their "later" puzzles in the overall sample. There was a moderate positive correlation for past and next performance at the lower threshold, and a slightly stronger one at the higher threshold. The moderate correlation strengths and improvement with a larger past sample do provide some optimism that including constructor identity and past performance against constructor in a predictive model of GMS solve performance will be beneficial.    

**Figure 6. Scatterplots of GMS Past Performance Against Individual Constructor(s) Versus GMS 'Next' Individual Puzzle Performance Against the Same Constructor(s)**  

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a841afc6-4f48-4747-b60e-7a4e5800dd11)
<h5>Pearson correlation coefficient (r): >=1 previous puzzle: .27, >=4 previous puzzles: .31


### Correlations of GMS Performance on Individual Puzzles to Puzzle-Specific Features and Recent Past Performance

Numerous potentially interesting features pertaining to puzzle grids, clues, and answers were obtained from XWord Info across the sample period. Features showing strong correlation to GMS solve performance become strong candidates as input features for predictive modeling, in current forms and/or when combined in novel ways with other existing features. **Figure 7** shows correlation heatmapping separately for 15x15 puzzles (Mon-Sat) and 21x21 puzzles (Sun) for a subset of all measured features with distributions amenable to linear regression and correlation analysis (see **Supplementary Figure 1** for individual 15x15 puzzle day matrices). The Pearson correlation coefficient (r) captures linear correlation strength between a given feature and solve times (top row and leftmost column of correlation matrix; red indicates a strong positive correlation and green a strong negative correlation). The rightmost column/bottom row per matrix shows the correlation between GMS solve times for individual puzzles and puzzle day-specific recent performance baseline (RPB). For both 15x15 puzzles and 21x21 puzzles, this (positive) correlation was stronger than any other measured feature correlation to GMS solve time. This finding generates a prediction that recent (relative to a puzzle date to be predicted) solver form per puzzle day will be more predictive of performance on a novel puzzle than will be any individual grid, clue or answer feature. 

As can also be seen in the **Fig. 7** correlation matrices, a number of grid, clue and answer-related features correlated strongly with each other. For example, 'Average Answer Length' and 'Freshness Factor' showed a strong negative correlation. This relationship makes intuitive sense because 'Freshness Factor' is a measure of aggregate answer rarity for a given puzzle, and longer answers have a higher likelihood of being uncommon than shorter ones. Additionally, numerous features not selected for this analysis might still be useful in predictive modeling but either had non-continuous distributions (e.g., 0 or 1 for puzzles with normal vs non-standard symmetry) or pertain to a feature that is largely specific to only one or several puzzle days (e.g. Rebuses, Circles, Shaded Squares; see **Supplementary Figures 2-4**).   

  
**Figure 7. Correlation Heatmapping of GMS Raw Solve Times vs Grid, Clue, Answer and Past Performance Features**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/d33525ca-12c0-4c0f-87d0-c88adab9b9be)
*<h5>Correlation heatmaps derived from N=1,589 15x15 (left panel) and N=264 21x21 (right panel) puzzles solved by GMS from 2019-2024 (see note below).*

*Note on Inclusion Time Range: As is clear in **Fig. 7**, RPB had the strongest correlation to individual solve times of any feature evaluated by a considerable degree. As such, there was a concern that shifts in baseline solve speed could overwhelm or mask correlations with puzzle-specific features. But normalizing raw solve times (e.g., with RPB) cancels out cross-puzzle day trends that I was interested in identifying prior to the modeling stage. As such, I arrived at a compromise in which I removed all solves from the first year in the sample (n=365), which reduced baseline volatility in the solve pool to a considerable degree and allowed me to keep raw solve times as the y-axis variable across these analyses.*

###
**Figure 8** through **Figure 19** are companion figures to the correlation heatmapping shown in **Fig. 7**. These figures show, across all 15x15 puzzle days (black) and by puzzle day (colors), scatterplots of select features of interest vs GMS raw solve times at the level of individual puzzles. A feature distribution density plot (FDP) shows puzzle day-specific trends in the distribution of each plotted feature. 

Though most features had at least moderate strength correlations with GMS solve times across all 15x15 puzzles, at the by puzzle day level these correlations were typically stronger for the more difficult puzzle days (Sat, in particular). Correlations robust enough to show through within a given puzzle day, where many variables related to puzzle difficulty presumably had similar properties across puzzles, are likely to have a meaningful impact on solve times. As is evident across the scatterplot figures below, several factors make it more likely for later week puzzle days (e.g., Sat) to show within-day correlations that may also be associated with predictive value. For one, there may be threshold values per feature below/above which that feature's impact was not significant in comparison with the contribution of solver aptitude (see discussion above about **Fig. 7**, and also see **Fig. 19**). Because late week puzzle days almost uniformly had considerably wider ranges of feature values than earlier week puzzle days (compare per day widths in the FDPs or x-axis extents in the scatterplots), effects apparent on those late week days may have been severely clipped by limited ranges on the early week days. Secondly, early week puzzles may also simply not have been difficult enough overall for given features (especially grid features, as a clue is only as difficult as the answer content that it houses) to have had discernable impacts on solve times.

#### Scatterplots for Individual Features vs Global Median Solver Solve Times (GMSTs), with Associated Feature Distribution Density Plots (FDPs)

#### *Grid Features*

**Figure 8. Number of Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a3ecb582-81dc-49cb-8985-62a7488660b6)
*<h5>Global Median Solver solve times (GMSTs) and '# Answers' had a moderately strong negative correlation for 15x15 puzzles (r= -.57).<br>*

*Most of the strength of the all 15x15 puzzles correlation (black) was related to the large leftward shift in the FDP for the two most difficult puzzle days (Fri and Sat). '# Answers' was strongly negatively correlated with 'Average Answer Length' (see Fig. 7) and measures of answer rarity (e.g., 'Freshness Factor'; see Fig. 17). Thus, when puzzles were difficult (Fri and Sat), this fewer answers/more long answers combination meant more answers that were rarely encountered/unique. Within Saturday a correlation of the same directionality as the overall 15x15 puzzles correlation was seen, emphasizing this relationship. Moreover, correlations of the reverse sign (positive) were seen for the early week puzzle days, prominently for the easiest puzzle day (Monday). This finding suggests that, below a particular per-clue/answer difficulty threshold mostly only attained in later week (Thu-Sat) puzzles, the time penalty incurred by having to read relatively more clues was greater than the time savings from encountering relatively fewer longer answers.*       

**<h4>Figure 9. Number of Open Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f500fb40-0789-4870-af1c-43dbd65b6d5c)
*<h5>GMSTs and '# Open Squares' had a borderline strong positive correlation for 15x15 puzzles (r= .58).<br>*

*'# Open Squares' is a proprietary measure from XWord Info that counts all white squares that are *not* bordered by black squares. Most of the strength of the all 15x15 puzzles correlation (black) was related to the large rightward shift in the FDP for the two most difficult puzzle days (Fri and Sat), with nearly all 15x15 puzzles with >~80 open squares falling on those days. '# Open Squares' was strongly negatively correlated with '# Answers' and strongly positively correlated with '#Average Answer Length' (see Fig. 7). For difficult puzzle days (Fri and Sat), these relationships translated to longer answers that were also more difficult. Unlike for '# Answers', however, some degree of positive correlation was seen for all puzzles days. This indicates that there may not be a difficulty threshold for this feature; the more '# Open Squares', the more time a puzzle tended to take to solve regardless of overall difficulty.*   
 
**<h4>Figure 10. Number of Black Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/99b67b59-e3bb-4c0e-96fa-0f7299af38d2)
*<h5>GMSTs and '# Black Squares' had a borderline moderate negative correlation for 15x15 puzzles (r= -.39).<br>*

*Most of the strength of the all 15x15 puzzles correlation (black) was related to the large leftward shift in the FDP for the two most difficult puzzle days (Fri and Sat), with nearly all 15x15 puzzles with <~32 black squares falling on those days. '# Black Squares' was strongly negatively correlated with both '# Open Squares' and 'Average Answer Length'. So an increase in '# Black Squares' meant shorter, easier answers and a faster solve on average. Within Saturday a relatively strong correlation of the same directionality as the overall 15x15 puzzles correlation was seen, emphasizing these dynamics. The more modest and mixed-sign correlations on the earlier week puzzle days, however, might indicate a difficulty threshold for '# Black Squares' to have an impact on solve times, though the lack of puzzles with <~32 black squares on early week puzzle days makes it hard to discern that from a range effect.*

**<h4>Figure 11. Average Answer Length**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/026d5d2b-1f1c-4525-b948-b599609cce32)
*<h5>GMSTs and 'Average Answer Length' had a strong positive correlation for 15x15 puzzles (r= .69).<br>*

*The strong all 15x15 puzzles correlation for GMS was related to the large rightward shift in the FDP for the two most difficult puzzle days (Fri and Sat). Saturday also showed a strong positive correlation across a wide range of feature values, with higher 'Average Answer Length' being associated with slower solves. As discussed in the context of Figs. 8 and 9, 'Average Answer Length' was strongly negatively correlated with '# Answers' and strongly positively correlated with measures of answer rarity (e.g. 'Freshness Factor; see Fig. 17). So it makes intuitive sense that as 'Average Answer Length' increased, particularly on difficult puzzle days, the answers themselves became more difficult and slowed down solve times even as they decreased in absolute number.* 

*Another interesting observation regarding this feature is that, unlike for the other grid features discussed thus far, the Thursday peak was well-differentiated to the right of those for the earlier week puzzle days in the FDP. Given the relatively strong positive correlations overall and for several late week puzzle days with wide feature value ranges (including Thursday) and the need to explain why solve times for Thursday were well right-shifted from the earlier week puzzle days (see Fig. 3), this feature becomes an attractive candidate for having some predictive value across multiple puzzle days in the modeling phase.* 


**<h4>Figure 12. Number of Cheater Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/fc03f9fc-d3a5-4acc-9262-524278a53643)
*<h5>GMSTs and '# Cheater Squares' had a weak positive correlation for 15x15 puzzles (r= .20).<br>*

*Cheater Squares are black squares than can be removed without affecting the overall word count of the grid. These squares make construction easier (hence their name). It can be seen in the FDP that large numbers of them (>~10) almost always appeared on the difficult puzzle days (Fri and Sat), which likely accounted for the (modest) positive correlation across all 15x15 puzzles. Saturday, with by far the widest feature value range of any of the 15x15 puzzle days, showed a moderate reverse sign (negative) correlation. This is not really at odds with the overall 15x15 positive correlation, since these squares are ultimately just black squares even if they facilitate tricky constructions for difficult puzzles. Add enough of them and they will reduce solve time simply by lowering the amount of fill. Incidentally, the reason cheater squares were only rarely seen in odd numbers is the NYT general requirement for grid symmetry.*


#### *Answer and Clue Content Features*

**<h4>Figure 13. Number of Fill-in-the-Blank Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f323b2f7-832a-497d-b154-31f7d86dc053)
*<h5>GMSTs and '# Fill-in-the-Blank Answers' had a weak-to-moderate negative correlation for 15x15 puzzles (r= -.31).<br>*

*Most of the strength of this weak-to-moderate correlation for all 15x15 puzzles was related to the moderate rightward shift in the FDP for Monday. Along with the easiest puzzle day employing the largest dose of '#Fill-in-the-Blank', the hardest puzzle day (Saturday) was also slightly left-shifted relative to the other 15x15 puzzle days. There was a moderate negative correlation within Saturday as well, with the rare puzzles employing >~5 fill-in-the blank clues associating with distinctly faster solve times. This indicates that above a certain level of difficulty, fill-in-the blanks likely provide a speed-up mechanism. The lack of correlations within the other puzzles days, however, indicates that the difficulty threshold for this feature to matter was quite high.* 

**<h4>Figure 14. Scrabble Average**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/88cd507d-4c8f-4ea1-acf6-95289e8233c8)
*<h5>GMSTs and 'Scrabble Average' had a weak-to-non-existent negative correlation for 15x15 puzzles (r= -.03).<br>*

*'Scrabble Average' is another proprietary XWord Info measure, in which each letter in the answer grid is assigned its equivalent value in Scrabble. Since tile values in Scrabble increase with rarity of letter frequency in English texts, it would make sense that a higher value for this feature would be associated with *answers* of greater rarity. The later week 15x15 puzzle days (Fri and Sat) did show this tendency with positive correlations, suggesting that maybe there's a difficulty threshold for that relationship to manifest, though the magnitudes were not strong. Furthermore, 'Scrabble Average' had only moderate positive correlations with other more direct measures of answer rarity both across 15x15 puzzles (Fig. 7) and specifically within the later-week puzzle days (Supp. Fig. 1). Given that these more direct measures of answer rarity DID have strong positive correlations to solve times (see Figs. 15-17), this feature is a candidate to either be left out of predictive modeling entirely or to be combined with other answer rarity/difficulty measures to generate a novel predictive feature of slightly different flavor.*

**<h4>Figure 15. Number of Scrabble Illegal Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/1d0852a5-103b-4aca-b3ce-d45f75b9e870)
*<h5>GMSTs and '# Scrabble Illegal' had a weak positive correlation for 15x15 puzzles (r= .19).<br>*

*'# Scrabble Illegal' answers is a proprietary measure of XWord Info that gets at answer rarity more directly than does 'Scrabble Average'. Interestingly, the distributions for 15x15 puzzle days in the FDP were highly overlapping, other than a small leftward shift for Monday and Tuesday that likely accounted for the (modest) overall 15x15 puzzles positive correlation. '# Scrabble Illegal" had only moderate positive correlations with the most direct measures of answer rarity ('# Unique Answers' and 'Freshness Factor'; see Figs. 16 and 17), which was somewhat surprising to me. There were hints, particularly in the Monday and Saturday correlation plots, that this feature might have impacted solve times at the extreme low and high ends of its value range. However, taken together with the weak correlation to solve times shown by this feature, the findings here suggest that more non-standard vocabulary *alone* may not strongly signify or predict puzzle difficulty.*  


**<h4>Figure 16. Number of Unique Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a5603a43-c506-49b9-9ca1-ad8d9a6c2e61)
*<h5>GMSTs and '# Unique Answers' had a moderate positive correlation for 15x15 puzzles (r= .40).<br>*

*<h5>A unique answer is defined here as one that does not appear in any other NYT crossword puzzle in either the Shortz or pre-Shortz eras (either before or after the puzzle release date). The moderate strength of the all 15x15 puzzle day correlation was related to the easiest puzzle day (Monday) having a leftward shift in the FDP while the most difficult puzzle days (Fri and Sat) had rightward shifts. Given the relatively low correlations and the results shown in the next figure, uniqueness is perhaps an overly stringent criterion with which to define answer rarity. However, as evidenced in the all 15x15 puzzles scatterplot, there may still be some use for this feature in the predictive modeling phase when values reach both extremes of the range*

**<h4>Figure 17. Freshness Factor**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f98ef594-6e5c-49dd-840a-e5cb935c49ee)
*<h5>GMSTs and 'Freshness Factor' had a strong positive correlation for 15x15 puzzles (r= .70).<br>*

*<h5>'Freshness Factor' is yet another proprietary XWord Info measure that assesses the aggregate relative novelty of all answers in a given crossword puzzle as compared to those in all other crossword puzzles in the NYT archive. The much stronger correlation to GMSTs as compared to that for '# Unique Answers' suggests that there's much to be gained by taking a graded, as opposed to all-or-none, approach in assessing answer rarity. Overall, this feature had the strongest correlation with GMS solve times of any grid, clue or answer feature evaluated (but see Fig. 19 for a past performance feature with a stronger correlation to solve time).* 

*Additionally, with potential relevance to predictive modeling, the 15x15 puzzle days peaked in this measure (seen in the FDP) in relatively close concordance to the by day peaks in raw solve times (see **Figs. 1 and 2**) for the GMS. The only other puzzle feature coming close to this degree and concordance of peak separation was 'Average Answer Length', though 'Freshness Factor' showed a Monday-Tuesday separation more in line with the solve time peaks separation for those two easy puzzle days. The positive correlation for this feature was also seen to a relatively consistent degree within each of the individual puzzle days. Saturday, strikingly, had both the strongest within-day correlation and also the widest range of feature values of any individual puzzle day. The observations discussed here lead me to believe that 'Freshness Factor' will be the most useful puzzle feature evaluated presently in terms of predictive modeling of solve performance.*


**<h4>Figure 18. Number of Wordplay Clues**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0816c344-9a94-454f-9c5b-8ef6f1cdd51a)
*<h5>GMS solve times and '# Wordplay Clues' had a moderate positive correlation for 15x15 puzzles (r= .44).<br>*

*<h5>'# Wordplay' clues is an (admittedly) somewhat subjective measure that I have manually evaluated and calculated clue-by-clue across (nearly) the entire puzzle sample completed by the GMS. The FDP for this feature had some interesting properties, including the clear result that later week (Thu-Sat) puzzles indeed had a larger allocation of 'trickier' clues than early week puzzles. There was also a prominent leftward shift for Monday puzzles, though with a strong second peak aligned with the Tuesday peak. Taken together, these early and late week distribution offsets were related to the moderate overall positive correlation across all 15x15 puzzles. The rare early week puzzles with a relatively large '# Wordplay' clues clearly had slower GMSTs, which makes intuitive sense in the context of how straightforward those puzzles generally were. Past Tuesday, however, there were only very weak within-puzzle day correlations for this feature. This suggests that there may be an overall difficulty threshold that largely dictates the impact that '# Wordplay Clues' might have on solve speed.* 
  

#### *Past Performance Features*

**<h4>Figure 19. Decay-Time Weighted Recent Performance (RPB)**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/824b50e1-40bd-4ebd-8993-cb3f5e6406ad)
*<h5>Next GMST and GMS 'Recent Performance Baseline (RPB)' had a very strong positive correlation for 15x15 puzzles (r= .86).<br>*

*<h5>The all 15x15 puzzles correlation for this feature was considerably stronger than that for any puzzle feature. The implication is that time decay-time weighted GMS recent past performance (RPB over the 20 day-specific puzzles previous to a given solve) will likely have more predictive value for the next GMS raw solve time than any single puzzle, clue or answer feature. Not all puzzle days were created equally, however, with regard to correlational strength (and, potentially, predictive power) of RPB and next raw solve time. Monday (r=.59) having an extremely strong correlation relative to the other 15x15 puzzle days was unsurprising, given how few "degrees of freedom" there were in the easiest puzzles. It is notworthy, however, that the lowest correlation for the GMS was for Saturday (.22). My suspicion is that this largely relates to the volatility of the Saturday solver pool that the GMS is drawn from. Because Saturday was consistently considerably more difficult than the other puzzle days, that puzzle day may have had both a lower N to draw from for a given puzzle as well as a much more variable roster of puzzle completers. Of course, there may also be a substantial contribution to the relatively low Saturday correlation for the GMS both from the heterogeneity of Saturday puzzles (e.g., typically wide feature value ranges) and the high likelihood of middle-of-the-pack solvers getting "stuck" for extended stretches on one or several tough clues or answers.*

*It is noteworthy in comparison to the GMS that the lowest correlation between next raw solve times and RPB for one studied individual solver (IS1) and the second lowest for the other (IS2) was for Thursday. This is perhaps unsurprising, since Thursday puzzles had a large degree of heterogeneity, with nearly all puzzles on that day involving a gimmick or trick of some variety (including rebuses of various flavors; see Supp. Fig. 2). Thursday came in as the day with the second lowest correlation for the GMS as well (.35), so variability in performance on that puzzle day may generalize across the solver pool.*

*<h5>'Next' GMST and GMS RPB Correlation Strength by Puzzle Day:*<br>
*Sun: .55, Mon: .59, Tue: .49, Wed: .39, Thu: .35, Fri: .40, Sat: .22*<br>


## Supplementary Figures

**<h4>Figure S1. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past Performance Features by Puzzle Day (15x15 Puzzle Days)**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/614eeef8-e1bb-4b1e-84a7-e504c208cdb0)



**Figure S2. Number of Rebus Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/953008aa-62d2-4575-90eb-6ce3d6f9f0c5)
*<h5>Only Sunday and Thursday had an appreciable '# Rebus Squares' for the set of GMS solves. Rebus squares are those that must be filled with more than one letter, number or symbol for a given puzzle to be solved. There were weak-to-moderate positive correlations for both Sunday (r=.12) and Thursday (r=.15). The directionality of the correlations does make intuitive sense, as both their existence and the "rules" for any given rebus can often take a little while to figure out. Additionally, they increase solve time by some degree simply by requiring additional fill and menu toggling relative to a non-rebus puzzle. One caveat here is that the very large number of 0 rebus puzzles, even on Sunday and Thursday, make the strength of these correlations hard to interpret (ie, these are not exactly continuous distributions).*<br>
 

**<h4>Figure S3. Number of Circled Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/3fdfde5a-0417-4d35-bf5a-9ccc91bdd2cf)
*<h5>Circled squares were virtually non-existent on the tougher (Fri and Sat) puzzle days. The modest negative correlation seen across all 15x15 puzzle days (-.11) is attributable to the fact that most 15x15 puzzles with circles appeared early in the week. The smattering of puzzles with circles on Sunday almost all fell in the middle of the solve time range regardless of '# Circles', indicating that this feature likely likely have a major impact on solve times.*


**<h4>Figure S4. Number of Shaded Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/76df6209-f483-44a5-af55-ae42e8d0f409)
*<h5>Shaded squares, like circled squares, were virtually non-existent in the tougher (Fri and Sat) puzzles. Also like with circled squares, their function is to reveal a puzzle theme and their presence may provide assistance to solvers on clues in which they are embedded. Most puzzles with shaded squares were within the bottom third of GMS 15x15 puzzle solve times, most likely due to shaded squares almost exclusively showing up only in early week puzzles. Also as with '# Circles', the smattering of Sunday puzzles mostly fell in the middle of the solve time range regardless of "# Shaded Squares", indicating that this feature also likely didn't have a major impact on solve times.* 
 

## Supplementary Tables

**<h4>Table S1. Features Included in Puzzle Features Principal Component Analysis**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0ef75602-216c-4a0d-b84e-e32723f7f847)



