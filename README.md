# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle

## Introduction

### Project Overview and Data Sources
This summary reports on exploratory data analysis (EDA) of the Global Median Solver's (GMS) performance over 6+ years (Jan. 2018 - Jan. 2024) of the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords). Included are visual and statistical descriptions of trends in GMS solve times across this period, and the relationship between GMS performance and a number of variables. These variables included properties of the puzzle grids (e.g., number of answers, number of black squares), clues and answers (e.g., frequency of wordplay in clues, aggregate rarity of answers in a puzzle), constructor identity, and puzzle day-specific recent past performance prior to a given solve. This EDA led to identification of a set of features that hold promise as useful inputs to a predictive model of GMS future performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info has a contract with NYT for access to the raw data underlying these metrics, but I unfortunately do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site. Nonetheless, [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks)). The second, [XWStats](xwstats.com), was my source for historical GMS raw solve time data. From these raw solve times for each puzzle, Matt calculates the global median solve times (GMSTs). Per personal communication with the owner of XWStats (Matt), most puzzle dates have somewhere between 1-2K individual solver solve times recorded. I do *not* have the underlying raw data for solvers in the database apart from that for two experienced solvers, both of whom are subjects of similar analyses; [Individual Solver 1(IS1)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-1/blob/main/README.md) and [Individual Solver 2(IS2)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-2/blob/main/README.md). 

Please visit, explore and strongly consider financially supporting both of these wonderful data sites; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Crossword and GMS Characteristics
The NYT crossword has been published since 1942, and many consider the modern era of the puzzle to have started with the arrival of Will Shortz as (only) its 4th editor 30 years ago. A new puzzle for each day is published online at either 6 PM (Sunday and Monday puzzles) or 10 PM (Tuesday-Saturday puzzles) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick or trick of some sort (e.g., "rebuses" where the solver must enter more than one letter into one or more squares). Additionally, nearly all Sunday-Thursday puzzles have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions and longer (often multiword) answers than the early week puzzles. The clue sets tend to be more wordplay heavy/punny as the week goes on, and the answers become less common in the aggregate as well. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is somehwere between a tough Wednesday and an easy Thursday. 

**Figure 1** shows dimensionality reduction via Principal Component Analysis (PCA) of 23 grid, clue and answer-related features obtained from XWord Info (see **Supplemental Table 1** for a full list of included features). This analysis demonstrates that, while puzzles from a given puzzle day do indeed aggregate with each other in n-dimensional "puzzle property space", the puzzle days themselves nonetheless exist along a continuum. Sunday is well-separated from the other puzzle days in this analysis by PCA1, which undoubtedly incorporates one or more grid size-contingent features.   

**Figure 1. PCA of Select Puzzle Grid, Clue and Answer Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/fd334ea9-5318-4b60-a90d-0ec9cf35b1ae)
*<h5>The first 3 principal components accounted for 47.6% of total variance. All puzzles from Jan. 1, 2018- Jan. 24, 2024 were included in this analysis (N=2,215).*
###
The overlapping distributions in GMSTs in the solve time density plot in **Figure 2** show a parallel performance phenomenon to the continuum of puzzle properties seen in **Fig. 1**; namely that while solve difficulty increased as the week progressed, puzzle days of adjacent difficulty had substantially overlapping GMST distributions. Other than for the "easy" days (Monday and Tuesday), distributions of GMSTs were quite broad. Wednesday and Saturday also had somewhat bimodal solve time distributions, supporting the notion that there are "easy" and "hard" puzzle pools/constructors at the level of specific puzzle days. The broadness of each puzzle day-specific GMST distribution was also increased by the fairly dramatic improvement in GMS performance across the sample time range. This improvement will be highly evident in the next section's figures.     

One additional contextual note about the GMS is worth mention upfront. Though Matt from XWStats uses the word "global", and I adopt it as well, it is highly likely that the sample from which the GMST is pulled per-puzzle skews faster than the true population distributions. The reasons for this assumption are twofold; only solvers who actually complete a given puzzle are included in its sample, and each sample contains only solvers motivated enough by the prospect of improvement to track their own progress to begin with.

**<h4>Figure 2. Distributions of GMSTs by Puzzle Day**                   

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b5845b13-ba8d-4038-a452-ace1edba65a1) 
*<h5>All puzzles from Jan. 1, 2018- Jan. 24, 2024 were included in this analysis (N=2,215).* 


 

## Results
### Global Median Solver (GMS) Performance Over Time

The GMS solved N = 2,215 puzzles in the full sample period (365 per year, except 2020: 366 and partial year 2024: 24). The total solve time for GMS was 31.9 days (2018: 6.8; 2019: 5.8; 2020: 5.2 days; 2021: 4.8; 2022: 4.7; 2023/24: 4.6). Note that GMS performance is tracked in the present analyses by puzzle *issue* date, as I did not have access to GMS puzzle completion dates. It's reasonably safe to assume, however, that the GMS (a different individual solver for most puzzles, presumably) solved in approximately the sequence of puzzle issue. Individual solver (IS1 and IS2) performance (see links in Introduction), in contrast, was tracked by puzzle *completion* date since I *was* able to obtain completion timestamps for those solvers' completed puzzles with Matt's assistance.

GMSTs improved over the complete set of puzzles (N=2,215) issued between January 1, 2018 and January 24, 2024 (**Figure 3**), with fairly dramatic improvement seen early on for some puzzle days and graded improvement continuing for each puzzle day until the end of the sample period (top panel). These improvement dynamics can also been seen in the aggregate raw solve time per year data reported above. The 2-year interval density plots of raw solve time distributions (bottom panels) show that performance on individual puzzle days became more consistent over time (higher peaks with narrower distributions). Because I did not have access to the raw solver data from which the GMSTs were drawn, however, it was not possible to disentangle improvement for individual "early adopters" of Matt's tracking software versus stronger solvers joining the solver pool over time.  

**Figure 3. Solve Time Overview by Puzzle Day: 10-Puzzle Moving Averages and Distributions of Raw Values**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/859ab075-c070-4f48-a96c-b33cb3af51c9)
*<h5>GMS Final (as of Jan. 24, 2024) 10-puzzle moving average of solve time (m), per puzzle day:*<br>
*Sun: 28.8, Mon: 5.7, Tue: 7.7, Wed: 11.3, Thu: 15.6, Fri: 17.8, Sat: 21.1*<br>

###
**Figure 4** shows the GMS solve time performance trajectory in violin plots with swarm plot overlays, broken out by 2 year (2+ for 2023/24) solve date intervals. Violin plots show both the range (vertical extent) and distribution characteristics (width as it varies across the y-axis range) for each puzzle day, per solve interval. Black lines on the violin plot demarcate solve time quartiles per puzzle day. Swarm plot overlays per puzzle day show individual puzzle raw solve times.   


**Figure 4. Solve Time Overview by Puzzle Day: Violin Plots With Swarm Plot Overlay**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/ea97753d-fc63-4232-93ce-dcc92f4ede29)
*<h5>Median[IQR] solve time (m), per puzzle day, per solve interval:*<*<br>
*2018/2019: Sun: 50.0[42.3-60.0], Mon: 8.6[7.6-9.8], Tue: 12.1[9.5-14.3], Wed: 16.5[13.1-20.1], Thu: 24.6[20.2-29.0], Fri: 24.7[21.6-29.5], Sat: 31.6[25.1-36.2]*<br>
*2020/2021: Sun: 37.1[32.2-41.5], Mon: 7.1[6.4-7.9], Tue: 9.5[8.2-10.9], Wed: 12.7[10.8-15.2], Thu: 20.3[17.6-24.1], Fri: 20.7[18.6-23.9], Sat: 26.9[22.8-31.2]*<br>
*2022-2024: Sun: 33.2[28.6-37.7], Mon: 6.0[5.6-6.4], Tue: 8.3[7.5-9.6], Wed: 11.8[10.0-13.9], Thu: 18.5[16.0-21.1], Fri: 18.9[17.0-21.5], Sat: 24.1[20.4-29.0]*  


### GMS Performance By Puzzle Constructor(s)

A high proportion of puzzles solved by the GMS (%) were authored by either repeat individual constructors or specific constructor teams (both referred to as "constructor" from here forward). This afforded the opportunity to evaluate which constructors the GMS tended, in a relative sense, to struggle or do well against. Per constructor, the mean of % difference from 'recent performance baseline' (RPB) across all puzzles in the sample that they authored was computed. To compute RPB itself, each raw solve time was taken as a % difference from a decay-time weighted average of the *previous* 10 puzzles solved on the same puzzle day. Thus, taking the mean of this metric per constructor isolated "constructor difficulty" for the GMS via adjustment for both GMS "recent form" and variability in the mix of puzzle days for puzzles authored by each constructor.  

**Figure 5** shows heatmapping of GMS performance, using this normalized measure (RPB), against the n=115 constructors contributing >=5 puzzles over the sample period. While only 16% of constructors contributed this many puzzles in the sample period, this group contributed 55% of all puzzles solved by the GMS during this period. Warmer colors (-%) indicate that the GMS solved relatively fast against a given constructor; cooler colors (+%) indicate the opposite. At this puzzle number threshold, the mean difference from RPB for GMS solve time against different constructors ranged from -29.3% ("easiest constructor" Howard Barkin) to 31.1% ("hardest constructor" Jules Markey).   
      
**Figure 5. Heatmapping of GMS Performance Against Individual Constructors**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/65f530be-3716-4e54-addd-5ea5b9a25ec9)

The next correlational analysis was aimed at assessing the potential predictive value of the GMS' baseline-adjusted past performance against specific constructors on GMS future performance against the same constructor. **Figure 6** shows this correlation between GMS baseline-adjusted performance (as mean % difference from RPB) against a given constructor (x-axis) and performance on the next solved individual puzzle by that constructor (y-axis). The scatterplot in the left panel includes all n=1469 puzzles in the overall sample that were issued (and solved) *after* >=1 previous puzzles by the same constructor. The scatterplot on the right has a higher threshold for inclusion; the n=748 puzzles in the overall sample that were issued (and solved) *after* >=4 previous puzzles by the same constructor. Thus, at this higher threshold only puzzles by constructors included in **Fig. 6** are included, and only their "later" puzzles in the overall sample. There was a moderate positive correlation for "past" and "next" performance at the lower threshold, and a slightly stronger one at the higher threshold. The moderate correlation strengths and improvement with a larger "past" sample do provide some optimism that including constructor identity and past performance against constructor in a predictive model of GMS solve performance will be beneficial.    

**Figure 6. Scatterplots of GMS Past Performance Against Individual Constructor(s) Versus GMS 'Next' Individual Puzzle Performance Against the Same Constructor(s)**  

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/e28efe64-a30d-4b43-9835-0f06a9707270)
<h5>Pearson correlation coefficient (r): >=1 previous puzzle: .27, >=4 previous puzzles: .31


### Correlations of GMS Performance on Individual Puzzles to Puzzle-Specific Features and Recent Past Performance

Numerous potentially interesting features pertaining to puzzle grids, clues, and answers were obtained from XWord Info across the sample period. Features showing strong correlation to GMS solve performance become strong candidates as input features for predictive modeling, in current forms and/or when combined in novel ways with other existing features. **Figure 7** shows correlation heatmapping separately for 15x15 puzzles (Mon-Sat) and 21x21 puzzles (Sun) for a subset of all measured features with distributions amenable to linear regression and correlation analysis (see **Supplementary Figure 1** for individual 15x15 puzzle day matrices). The Pearson correlation coefficient (r) captures linear correlation strength between a given feature and solve times (top row and leftmost column of correlation matrix; red indicates a strong positive correlation and green a strong negative correlation). The rightmost column/bottom row per matrix shows the correlation between GMS solve times for individual puzzles and puzzle day-specific recent performance baseline (RPB). For both 15x15 puzzles and 21x21 puzzles, this (positive) correlation was stronger than any other measured feature correlation to GMS solve time. This finding generates a prediction that recent (relative to a puzzle date to be predicted) solver form per puzzle day will be more predictive of performance on a novel puzzle than will be any individual grid, clue or answer feature. 

As can also be seen in the **Fig. 7** correlation matrices, a number of grid, clue and answer-related features correlated strongly with each other. For example, 'Average Answer Length' and 'Freshness Factor' showed a strong negative correlation. This relationship makes intuitive sense because 'Freshness Factor' is a measure of aggregate answer rarity for a given puzzle, and longer answers have a higher likelihood of being uncommon than shorter ones. Additionally, numerous features not selected for this analysis might still be useful in predictive modeling but either had non-continuous distributions (e.g., 0 or 1 for puzzles with normal vs non-standard symmetry) or pertain to a feature that is largely specific to only one or several puzzle days (e.g. Rebuses, Circles, Shaded Squares; see **Supplementary Figures 2-4**).   

  
**Figure 7. Correlation Heatmapping of GMS Raw Solve Times vs Grid, Clue, Answer and Past Performance Features**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/2a67a3b4-1601-4391-a182-7d6e41973a0e)
*<h5>Correlation heatmaps derived from N=1,273 15x15 (left panel) and N=212 21x21 (right panel) puzzles solved by GMS from 2020-2024.*

*Note on Inclusion Time Range*: As is clear in **Fig. 7**, RPB had the strongest correlation to individual solve times of any feature evaluated by a considerable degree. As such, there was a concern that shifts in baseline solve speed could overwhelm or mask correlations with puzzle-specific features. But normalizing raw solve times (e.g., with RPB) cancels out cross-puzzle day trends that I was interested in identifying prior to the modeling stage. As such, I arrived at a compromise in which I removed all solves from the first solve interval (2018/2019; n=730), which reduced baseline volatility in the solve pool to a considerable degree and allowed me to keep raw solve times as the y-axis variable across these analyses.*

###
**Figure 8** through **Figure 19** are companion figures to the correlation heatmapping shown in **Fig. 7**. These figures show, across all 15x15 puzzle days (black) and by-puzzle-day (colored), scatterplots of select features of interest vs GMS raw solve times at the level of individual puzzles. A feature distribution density plot (FDP) shows puzzle day-specific trends in the distribution of each plotted feature. 

Though most features had at least a moderate correlation strength with GMS solve times across all 15x15 puzzles, at the by-puzzle-day level these correlations were typically stronger for the more difficult puzzle days (Sat, in particular). Any feature showing a relatively strong correlation within the late week puzzles days is a strong candidate for having a causal impact on solve times in the modeling phase. Correlations robust enough to show through when many other variables are controlled for within a given puzzle day are likely to have a meaningful impact on solve times. Furthermore, there may be threshold values per feature below/above which that feature's impact is not significant in comparison with the effects of solver aptitude (see discussion above about **Fig. 7**, and also see **Fig. 19**). Because late week puzzle days almost uniformly had considerably wider ranges of feature values than earlier week puzzle days (compare per day widths in the FDPs or x-axis extents in the scatterplots), effects apparent on those late week days would be absent or severely clipped by the limited ranges on the early week days. Finally, early week puzzles may also simply not be difficult enough overall for given features (especially grid features, as a clue is only as difficult as the answer content that it houses) to have discernable impacts on solve times.    

#### Scatterplots for Individual Features vs Global Median Solver Solve Times (GMSTs), with Associated Feature Distribution Density Plots (FDPs)

#### *Grid Features*

**Figure 8. Number of Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/89fcda39-7415-49cc-930e-2f8dbdd68a9e)
*<h5>Global Median Solver (GMS) solve times and '# Answers' had a moderately strong negative correlation on 15x15 puzzles (r= -.58).<br>*

*More answers typically meant shorter answers (see correlation matrices above), and shorter answers tended to be more common/easier answers (see 'Average Answer Length' and 'Freshness Factor' analyses below). Aligned with this relationship, the FDP for this feature shows that the toughest puzzle days (Fri and Sat) tended to have the fewest answers. Additionally, Saturday itself showed a moderate negative correlation across a fairly broad range of '# Answers' values. The relatively strong reverse sign (positive) correlations seen for Monday and Wednesday suggest that, below a particular per-clue/answer difficulty threshold, merely having to read more clues to solve the puzzle may penalize the solver more they are rewarded for avoiding longer answers.*


**<h4>Figure 9. Number of Open Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a5b8f21a-e212-490e-8de4-0095326c290d)
*<h5>For 15x15 puzzles, there was a strong positive correlation (r= .60) between GMST and '# Open Squares'. '# Open Squares' is a proprietary measure from XWord Info that counts all white squares that are *not* bordered by black squares. '# Open Squares' was strongly positively correlated to 'Average Answer Length' (see matrices above), so it makes sense that more open squares was also positively correlated with solve times. Interestingly, the correlation of this feature with solve time can be seen with at least moderate strength across all puzzle days. The FDP shows that the most difficult puzzle days (Fri and Sat) had a rightward shift in '# Open Squares' relative to the easier 15x15 puzzle days.* 

**<h4>Figure 10. Number of Black Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f9b5f8fc-db34-49e9-b0e2-fbf6e7eec361)
*<h5>For 15x15 puzzles, there was a moderate negative correlation (r= -.39) between GMST and '# Black Squares'. No surpise here, since this relationship was essentially the opposite of that between GMST and '# Open Squares' (more black squares = shorter answers = easier answers). As with '# Open Squares', the correlation was most apparent in the by-day scatterplots for the later week puzzle days, and Friday and Saturday were prominently shifted away from the earlier week puzzle days in the FDP.*

**<h4>Figure 11. Average Answer Length**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b2cb90af-3845-426a-b13e-f23e97517a0e)
*<h5>For 15x15 puzzles, there was a strong positive correlation (r= .67) between GMST and 'Average Answer Length'. This finding was consistent with other grid feature relationships with GMST, as longer answers means more multiword and relatively-rare answers (see correlation matrices above and Figs. 14-17). This correlation was very apparent within each of the puzzle days, and perhaps more so than any other puzzle feature, the sequence in peaks of puzzle day distributions in the FDP tracked with that in mean GMST by puzzle day. Perhaps this is an indication that this feature will be highly predictive of solve time in the modeling phase??*

**<h4>Figure 12. Number of Cheater Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/60c17f9a-bfd9-4c99-bca5-3aadfad0eb59)
*<h5>For 15x15 puzzles, there was a weak positive correlation (r= .16) between GMST and '# Cheater Squares'. Cheater Squares, by definition, are black squares than can be removed without affecting the overall word count of the grid. These square make construction easier (hence their name), and it can be seen that large numbers of them (say, >10) almost always appear on the difficult puzzle days. Interestingly, within a given puzzle day, however, it's clear that puzzles with larger numbers of them tended to be easier for the GMS. So the seeming paradox between the overall 15x15 trend and the individual puzzle day trends is likely related to the competing effects of cheater squares allowing trickier constructions, but also reducing the number of answers and answer lengths overall. Incidentally, the reason they were only rarely seen in odd numbers is the only rarely-violated NYT requirement for grid symmetry.*

#### *Answer and Clue Content Features*

**<h4>Figure 13. Number of Fill-in-the-Blank Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/2d727f7d-5f25-4157-9f6e-f9281d70e286)
*<h5>For 15x15 puzzles, there was a weak-to-moderate negative correlation (r= -.30) between GMST and '# Fill-in-the-Blank' (FITB) answers. Taken together, the FDP and the scatterplots indicate that most of this correlation was related to the fact that the easiest puzzles (note the rightward FDP peak shift for Mon, even relative to Tue) employed a heavy dose of FITB answers. It will be interesting to see how important this feature is in the modeling phase to prediction of early week solve times, specifically.*

**<h4>Figure 14. Scrabble Average**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/fc874495-300a-4180-8310-1e99f70fdaf0)
*<h5>For 15x15 puzzles, there was a weak-to-nonexistant negative correlation (r= -.03) between GMST and 'Scrabble Average'. 'Scrabble Average' is another proprietary XWord Info measure, in which each letter in the answer grid is assigned its equivalent value in Scrabble. Since tile values in Scrabble increase with rarity of letter frequency in English texts, it would make sense that a higher value for this feature would be associated with *answers* of greater rarity. If anything, the opposite was true in practice here and as can be seen in the next few figures there are direct measures of answer rarity that *do* have strong positive correlations to solve times. So this one is a candidate to either be left out of predictive modeling entirely or to be combined with other answer rarity/difficulty measures to generate a useful predictive feature.*

**<h4>Figure 15. Number of Scrabble Illegal Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/1b05331b-58fd-4f53-82c1-5c480774d78c)
*<h5>For 15x15 puzzles, there was a weak positive correlation (r= .21) between GMST and '# Scrabble Illegal' answers. '# Scrabble Illegal' answers is a proprietary measure of XWord Info that gets at answer rarity more directly than does 'Scrabble Average' (though not as directly as the measures in Figs. 16 and 17). Interestingly this moderate positive correlation was seen both across all 15x15 puzzles and within each puzzle day. Also interesting is that, apart from a Monday relative leftward shift in the FDP, the distributions for the other 15x15 puzzle days were highly overlapping. I had assumed that the days with more open squares and longer average answers would also have substantially more answers that are not standard English vocabulary words. This finding suggests that more non-standard vocabulary *alone* may not signify or predict puzzle difficulty.*  

**<h4>Figure 16. Number of Unique Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b44a691b-5655-4401-b00d-97d2cc4ba985)
*<h5>For 15x15 puzzles, there was a moderate positive correlation (r= .39) between GMST and '# Unique Answers'. A unique answer is defined here as one that does not appear in any other NYT crossword puzzle in either the Shortz or pre-Shortz eras (either before or after the puzzle release date). This is perhaps an overly stringent criterion to define answer rarity (see Fig. 17 for a graded approach to defining answer rarity). Nonetheless, the positive correlation was clearly apparent when considering all 15x15 puzzles together and also especially within the most challenging puzzle day (Sat). It is also clear in the FDP that '# Unique Answers' tended to increase as puzzle day difficulty increased.*


**<h4>Figure 17. Freshness Factor**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/f0f70217-a24e-409f-810f-072f3c0dc128)
*<h5>For 15x15 puzzles, there was a strong positive correlation (r= .68) between GMST and 'Freshness Factor'. 'Freshness Factor' is yet another, and very possibly the most useful for solve time prediction(?), proprietary XWord Info measure that assesses the aggregate relative novelty of all answers in a given crossword puzzle as compared to those in all other crossword puzzles in the NYT archive. The much stronger correlation to GMSTs as compared to that of '# Unique Answers' suggests that there's much to be gained by taking a graded, as opposed to all-or-none, approach in assessing answer rarity. As with 'Average Answer Length', it can be seen in the FDP that puzzle days peaked in this measure in close concordance with the per-day sequence for solve times (interesting little second hump for Mon close to the Tue peak, however)*

**<h4>Figure 18. Number of Wordplay Clues**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/26f6a339-5a06-41cc-b45c-52b0e00b973b)
*<h5>For 15x15 puzzles, there was a moderate positive correlation (r= .44) between GMST and '# Wordplay' clues. '# Wordplay' clues is an admittedly somewhat subjective measure that I have manually evaluated and calculated clue-by-clue across (most of...) the 6+-year sample. The FDP for this feature has some interesting properties, including the clear result that later week (Thu, Fri, Sat) puzzles indeed have a larger allocation of 'trickier' clues than early week puzzles. But also of interest is the distinct bimodality for Monday puzzles. Coupled to the positive correlations with GMST seen in the scatterplots for early week puzzles, there's a suggestion here that this feature may end up being useful in predictive modeling. Interestingly, positive correlations were not present in the later week puzzle days. This suggests that once puzzles reach a sufficient level of difficulty, clue trickiness may not be very important for influencing solve times (but modeling will provide some answers here).*  

#### *Past Performance Features*

**<h4>Figure 19. GMS Adjusted Recent Performance**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b7653864-042f-489f-aa1d-125686f24217)
*<h5>For 15x15 puzzles, there was a very strong positive correlation (r= .84) between GMST and GMS Adjusted Recent Performance (GMS-ARP). To obtain 'GMS-ARP' for a given puzzle the 10 most recent *prior* puzzles from the same puzzle day were averaged after first being decay weighted (10 for the most recent prior puzzle, 9 for the one before that and so on down to a weight of 1 for the 10th prior puzzle). Recent past performance across all 15x15 puzzles by this measure was more strongly correlated to performance on the "next" puzzle than the specific characteristics of that puzzle.*

*<h5>Very interestingly, correlation strengths for the early week 15x15 days were higher than those for the later week days (see below in caption). This attests to the relative heterogeneity of later week puzzles, and the likely fact that the likelihood of a solver getting stuck in one particular spot for an extended period of time goes way up. I have zero actual evidence for this, but I will further speculate as an experienced solver that Thursday puzzles have the lowest correlation of all because they are the most heterogeneous of all puzzle days due to the varied gimmicks and tricks employed. See **Supp. Fig. 1** for a visually clear depiction of this by-puzzle day trend (top row of each matrix, rightmost square gets less red across days)*

*<h5>Correlation Strength by Puzzle Day:*<br>
*Sun: .62, Mon: .55, Tue: .51, Wed: .45, Thu: .37, Fri: .40, Sat: .39*




## Supplementary Figures

**Supplementary Figure 1. Scatterplots of Number of Rebus Squares vs Global Median Solver Solve Time (GMST) by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/019762eb-bba6-415d-874a-0a4fa721d62d)
*<h5>Only Sunday and Thursday had an appreciable '# Rebus Squares', which are squares that must be filled with more than one letter, number or symbol for a given puzzle to be solved. There was weak-to-moderate positive correlation between '# Rebus Squares' for these puzzle days, though a caveat here is that the very large number of 0 rebus puzzles makes these correlations hard to interpret (ie, these are not exactly continuous distributions).*<br>

*Correlation Strength by Puzzle Day: Sun: .30, Thu: .13.* 

**<h4>Supplementary Figure 2. Scatterplots of Number of Circled Squares vs Global Median Solver Solve Time (GMST) by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b55aa818-a2a8-41d9-a044-1888a6f36e23)
*<h5>Circled squares were virtually non-existent in the tougher (Fri and Sat) puzzles. Their function is to reveal a puzzle theme, and in theory a solver can use this knowledge to "back in" to some full answers. Despite the apparent negative correlation for all 15x15 puzzles, the puzzle days with considerable '# Circles' mostly showed weakly positive correlations. One could speculate here, but it's probably not worth the effort; but there's potential for a small enhancement to modeling on a day-specific basis.*

**<h4>Supplementary Figure 3. Scatterplots of Number of Shaded Squares vs Global Median Solver Solve Time (GMST) by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/234db5a9-4cfb-4a31-a4a5-e5d035cacbce)
**<h5>Shaded squares, like circled squares, were virtually non-existent in the tougher (Fri and Sat) puzzles. Also like with circled squares, their function is to reveal a puzzle theme and their presence may provide assistance to solvers on clues in which they are embedded. Though most puzzles with shaded squares were within the bottom third of GMS 15x15 puzzle solve times, this was likely mostly due to the fact that they essentially only occurred in early week puzzles. As with circles, it can't hurt to include his feature in first-pass modeling and there might be some puzzle day-specific accuracy improvements with its inclusion.*  

**<h4>Supplementary Figure 4. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past-Performance Features by Puzzle Day (15x15 Puzzle Days)**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/393a31e4-7f41-4968-a9dc-91b91d50e9ce)


## Supplementary Tables

**<h4>Supplementary Table 1. Features Included in Puzzle Features Principal Component Analysis**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0ef75602-216c-4a0d-b84e-e32723f7f847)



