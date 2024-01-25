# Exploratory Data Analysis of Global Median Solver Performance on the New York Times Crossword Puzzle

## Introduction

### Project Overview and Data Sources
This summary reports on exploratory data analysis (EDA) of the Global Median Solver's (GMS) performance over 6+ years (Jan. 2018 - Jan. 2024) of the [New York Times (NYT) crossword puzzle](https://www.nytimes.com/crosswords). Included are visual and statistical descriptions of trends in GMS solve times across this period, and the relationship between GMS performance and a number of variables. These variables included properties of the puzzle grids (e.g., number of answers, number of black squares), clues and answers (e.g., frequency of wordplay in clues, aggregate rarity of answers in a puzzle), constructor identity, and puzzle day-specific recent past performance prior to a given solve. This EDA led to identification of a set of features that hold promise as useful inputs to a predictive model of GMS future performance. 

This project relied crucially on two amazing data sources. The first, [XWord Info: New York Times Crossword Answers and Insights](https://www.xwordinfo.com/), was my source for data on the puzzles themselves. This included a number of proprietary metrics pertaining to the grids, answers, clues and constructors. XWord Info has a contract with NYT for access to the raw data underlying these metrics, but I unfortunately do not. Therefore, I will not be able to share raw or processed data that I've acquired from their site. Nonetheless, [Jupyter notebooks](https://jupyter.org/) with all of my Python code for analysis and figure generation can be found [here](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/tree/main/notebooks)). The second, [XWStats](xwstats.com), was my source for historical GMS raw solve time data. From these raw solve times for each puzzle, XWStats (Matt) calculates the global median solve times (GMSTs). Per personal communication with Matt, most puzzle dates have somewhere between 1-2K individual solver solve times recorded. I do *not* have the underlying raw data for solvers in the database apart from that for two experienced solvers, both of whom are subjects of similar analyses; [Individual Solver 1(IS1)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-1/blob/main/README.md) and [Individual Solver 2(IS2)](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Individual-Solver-2/blob/main/README.md). 

Please visit, explore and strongly consider financially supporting both of these wonderful sites; XWord Info via [membership purchase at one of several levels](https://www.xwordinfo.com/Pay) and XWStats via [BuyMeACoffee](https://www.buymeacoffee.com/xwstats). 

### Overview of NYT Crossword and GMS Characteristics
The NYT crossword has been published since 1942, and many consider its "modern era" to have started with the arrival of Will Shortz as (only) its 4th editor 30 years ago. A new puzzle for each day is published online at either 6 PM (Sunday and Monday puzzles) or 10 PM (Tuesday-Saturday puzzles) ET the prior evening. Difficulty for the 15x15 grids (Monday-Saturday) is intended to increase gradually across the week, with Thursday generally including a gimmick or trick of some sort (e.g., "rebuses" where the solver must enter more than one character into one or more squares). Additionally, nearly all Sunday-Thursday puzzles have themes, some of which are revealed via letters placed in circled or shaded squares. Friday and Saturday are almost always themeless puzzles, and tend to have considerably more open constructions and longer (often multiword) answers than the early week puzzles. The clue sets tend to be more wordplay heavy/punny as the week goes on, and the answers become less common in the aggregate as well. Sunday puzzles have larger grids (21x21), and almost always feature a wordplay-intensive theme to which the longest answers in the puzzle pertain. The intended difficulty of the Sunday puzzle is approximately somehwere between a tough Wednesday and an easy Thursday. 

**Figure 1** shows dimensionality reduction via Principal Component Analysis (PCA) of 23 grid, clue and answer-related features obtained from XWord Info (see **Supplemental Table 1** for a full list of included features). This analysis demonstrates that, while puzzles from a given puzzle day do indeed aggregate with each other in n-dimensional "puzzle property space", the puzzle days themselves nonetheless exist along a continuum. Sunday is well-separated from the other puzzle days in this analysis by PCA1, which undoubtedly incorporates one or more grid size-contingent features.   

**Figure 1. PCA of Select Puzzle Grid, Clue and Answer Features**                                                                  
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/fd334ea9-5318-4b60-a90d-0ec9cf35b1ae)
*<h5>The first 3 principal components accounted for 47.6% of total variance. All puzzles from Jan. 1, 2018- Jan. 24, 2024 were included in this analysis (N=2,215).*
###
The overlapping distributions in GMSTs in the solve time density plot in **Figure 2** show a parallel performance phenomenon to the continuum of puzzle properties seen in **Fig. 1**; namely that while solve difficulty increased as the week progressed, puzzle days of adjacent difficulty had substantially overlapping GMST distributions. Other than for the "easy" days (Monday and Tuesday), distributions of GMSTs were quite broad. Wednesday and Saturday also had somewhat multimodal solve time distributions, supporting the notion that there are "easy" and "hard" puzzle pools/constructors at the level of specific puzzle days. The broadness of each puzzle day-specific GMST distribution was also increased by the fairly dramatic improvement in GMS performance across the sample time range. This improvement will be highly evident in the next section's figures.     

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
*<h5>Global Median Solver solve times (GMSTs) and '# Answers' had a moderately strong negative correlation on 15x15 puzzles (r= -.58).<br>*

*More answers typically meant shorter answers (see correlation matrices above), and shorter answers tended to be more common/easier answers (see 'Average Answer Length' and 'Freshness Factor' analyses below). Aligned with this relationship, the FDP for this feature shows that the toughest puzzle days (Fri and Sat) tended to have the fewest answers. Additionally, Saturday itself showed a moderate negative correlation across a fairly broad range of '# Answers' values. The relatively strong reverse sign (positive) correlations seen for Monday and Wednesday suggest that, below a particular per-clue/answer difficulty threshold, merely having to read more clues to solve the puzzle may penalize the solver more they are rewarded for avoiding longer answers.*


**<h4>Figure 9. Number of Open Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b39e3d52-92a3-4558-8529-453053b9a166)
*<h5>GMSTs and '# Open Squares' had a borderline strong positive correlation on 15x15 puzzles (r= .59).<br>*

*'# Open Squares' is a proprietary measure from XWord Info that counts all white squares that are *not* bordered by black squares. '# Open Squares' was strongly positively correlated to 'Average Answer Length' (see matrices above), so it makes sense that a greater '# Open Squares' was also positively correlated with solve times. The FDP shows that the most difficult puzzle days (Fri and Sat) had a rightward shift in '# Open Squares' relative to the easier 15x15 puzzle days. A large amount of the overall 15x15 correlation for the GMS appears to be accounted for by these more difficult puzzles with large numbers (>~80) of open squares. The positive correlation for Saturday, approximating the strength of the overall 15x15 positive correlation, is an indication that this feature will likely have some independent preditive value in the modeling phase (see text above Fig. 7 for the logic employed here).* 


**<h4>Figure 10. Number of Black Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/b0766f9b-dbd0-4687-9cd5-61d167c16814)
*<h5>GMSTs and '# Black Squares' had borderline moderate negative correlation on 15x15 puzzles (r= -.39).<br>*

*This relationship was essentially the opposite (albeit a weaker form) of that between solve times and '# Open Squares' (more black squares = shorter answers = easier answers). Both Friday and Saturday were strongly left-shifted in the FDP, which suggests that most of the overall 15x15 puzzle negative correlation was due to the most difficult days tending to have relatively few black squares. For the GMS, correlations across puzzle days varied in strength but each day showed at least some degree of negative correlation. As with '# Open Squares', the day with the widest range of values (Sat) had a negative correlation nearly as strong as the overall 15x15 correlation. This is an indication that this feature will likely have some independent preditive value in the modeling phase (see text above Fig. 7).*


**<h4>Figure 11. Average Answer Length**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/bd307c6a-3103-4505-bca4-8b25fa601b03)
*<h5>GMSTs and 'Average Answer Length' had a strong positive correlation on 15x15 puzzles (r= .70).<br>*

*This finding was consistent with other grid feature relationships to GMSTs, which makes sense since longer answers meant more multiword and relatively-rare answers (see correlation matrices above and **Figs. 15-17**). This positive correlation was present for all of the individual puzzle days and, as was typical for grid features for the GMS, it was strongest for Saturday. Monday stood out here for having the weakest positive correlation (bordering on a weak sign reversal, in fact) of all puzzle days. Given that this feature and '# Answers' are themselves highly negatively correlated (see **Fig. 8**), it makes sense that an easy puzzle day would again serve as the exception that proves the rule. Longer answers that are still easy may increase solver speed in the aggregate by reducing the amount of clues consumed needed for a solve, without a counterbalancing 'difficulty penalty' that might occur with longer answers on later puzzle days.* 


**<h4>Figure 12. Number of Cheater Squares**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/4896acff-ba9a-4e47-89d2-30dd68e5e2dc)
*<h5>GMSTs and '# Cheater Squares' had a weak positive correlation on 15x15 puzzles (r= .21).<br>*

*Cheater Squares are black squares than can be removed without affecting the overall word count of the grid. These squares make construction easier (hence their name). It can be seen in the FDP that large numbers of them (say, >10) almost always appeared on the difficult puzzle days (Fri and Sat), which accounts for the (modest) positive correlation across all 15x15 puzzles. There were, however, some degree of reverse sign (negative) correlations seen within each individual puzzle day. This raises the possibility of "diminishing returns" on '# Cheater Squares'; they may help make constructions "trickier" up to a point, but beyond that point they have a net effect of speeding solvers up simply by lowering the number of fill squares (a la high '# Black Squares'). Incidentally, the reason cheater squares were only rarely seen in odd numbers is the NYT general requirement for grid symmetry.*


#### *Answer and Clue Content Features*

**<h4>Figure 13. Number of Fill-in-the-Blank Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/41e2148f-8c1f-45b8-9adc-d2dab0d0e844)
*<h5>GMSTs and '# Fill-in-the-Blank Answers' had a weak-to-moderate negative correlation on 15x15 puzzles (r= -.26).<br>*

*Taken together, the FDP and scatterplots indicate that most of the strength of this correlation was due to the easiest puzzles (note the rightward FDP peak shift for Mon, even relative to Tue) employing a heavy dose of FITB answers. It's also noteworthy that the most difficult puzzle day (Sat) clearly made less use of FITB answers than the other puzzle days, and more '# FITB' on those days was associated with (slightly) speedier solves for GMS. These properties provide some optimism that this feature will have some predictive value, though the within day correlation strengths were certainly not remarkable.*


**<h4>Figure 14. Scrabble Average**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/74449e3e-6389-4430-93fa-921b1224bad8)
*<h5>GMSTs and 'Scrabble Average' had a very weak negative correlation on 15x15 puzzles (r= -.05).<br>*

*'Scrabble Average' is another proprietary XWord Info measure, in which each letter in the answer grid is assigned its equivalent value in Scrabble. Since tile values in Scrabble increase with rarity of letter frequency in English texts, it would make sense that a higher value for this feature would be associated with *answers* of greater rarity. If anything, the opposite was true in practice here. Furthermore, as can be seen in the next series of figures, there are direct measures of answer rarity that *do* have strong positive correlations to solve times. So this one is a candidate to either be left out of predictive modeling entirely or to be combined with other answer rarity/difficulty measures to generate a useful predictive feature.*


**<h4>Figure 15. Number of Scrabble Illegal Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/56922603-b2fa-4cd3-8664-2b13b41eee3c)
*<h5>GMSTs and '# Scrabble Illegal' had a weak positive correlation on 15x15 puzzles (r= .18).<br>*

*'# Scrabble Illegal' answers is a proprietary measure of XWord Info that gets at answer rarity more directly than does 'Scrabble Average' (though not as directly as the measures in **Figs. 16 and 17**). Interestingly, the distributions for 15x15 puzzle days in the FDP were highly overlapping, other than a leftward shift for Monday and Tuesday that appears to account for the (modest) overall 15x15 puzzles positive correlation. The very low '# Scrabble Illegal' (<20) seen nearly exclusively on those early week puzzle days was associated with very fast solves. Early week puzzles aside, however, I had previously assumed that the puzzle days with higher '# Open Squares' and 'Average Answer Length' (ie, later week days) would also have had proportionally more answers that were not standard English vocabulary words. A discernable trend for correlation strength within the more difficult puzzle days across the range of '# Scrabble Illegal' values was also lacking, with the exception of Saturday at the very high end of the feature value distribution. Taken together, these findings suggest that more non-standard vocabulary *alone* may not strongly signify or predict puzzle difficulty.*  


**<h4>Figure 16. Number of Unique Answers**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/08eb1fc6-9163-43bb-82d4-8659b0083ed7)
*<h5>GMSTs and '# Unique Answers' had a moderate positive correlation on 15x15 puzzles (r= .41).<br>*

*<h5>A unique answer is defined here as one that does not appear in any other NYT crossword puzzle in either the Shortz or pre-Shortz eras (either before or after the puzzle release date). This is perhaps an overly stringent criterion to define answer rarity (see **Fig. 17** for a graded approach to defining answer rarity). Nonetheless the positive correlation was clearly apparent when considering all 15x15 puzzles together, consistent with the most difficult puzzle days (Fri and Sat) pulling most of the weight given their prominent rightward shifts in the FDP, and also within most of the individual puzzle days. As with several other features previously evaluated, this correlation was strongest (albeit still modestly) within individual puzzle days for Saturday. Given that Saturday also had the widest feature value range of the 15x15 puzzle days, this provides some optimism for the predictive value of '# Unique Answers'.*


**<h4>Figure 17. Freshness Factor**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/3bd30c01-8cad-4710-bd1c-e5874169449e)
*<h5>GMSTs and 'Freshness Factor' had a strong positive correlation on 15x15 puzzles (r= .70).<br>*

*<h5>'Freshness Factor' is yet another proprietary XWord Info measure that assesses the aggregate relative novelty of all answers in a given crossword puzzle as compared to those in all other crossword puzzles in the NYT archive. The much stronger correlation to GMSTs as compared to that for '# Unique Answers' suggests that there's much to be gained by taking a graded, as opposed to all-or-none, approach in assessing answer rarity.  More so than any other grid, clue or puzzle feature, 15x15 puzzle days peaked in this measure (seen in the FDP) in close concordance with the peaks in the per day sequence for solve times (see **Figs. 1 and 2**). The positive correlation was also seen to a consistent degree within each of the individual puzzle days, with Saturday once again having the strongest across thewidest range of feature values of the 15x15 puzzle days. This finding generates a prediction that, apart from recent puzzle day-specific solver performance prior to a given solve (see **Figure 19**), 'Freshness Factor' will be the most useful feature evaluated in this analysis for predictive modeling of solve performance.*


**<h4>Figure 18. Number of Wordplay Clues**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/db9e2346-8cc7-4b27-a9e9-c096d9a50fd6)
*<h5>GMS solve times and '# Wordplay Clues' had a moderate positive correlation on 15x15 puzzles (r= .45).<br>*

*<h5>'# Wordplay' clues is an admittedly somewhat subjective measure that I have manually evaluated and calculated clue-by-clue across (nearly) the entire puzzle sample completed by the GMS. The FDP for this feature has some interesting properties, including the clear result that later week (Thu, Fri, Sat) puzzles indeed have a larger allocation of 'trickier' clues than early week puzzles. There's also a prominent strong leftward shift for Monday puzzles, though with a strong second peak aligned with the Tuesday peak. Taken together, these early and late week distribution offsets seem to account for the overall positive correlation across 15x15 puzzles. The rare early week puzzles with a relatively large '# Wordplay' clues clearly had slower GMSTs, which makes intuitive sense in the context of how straightforward those puzzles generally are. Past Tuesday, however, there were only very week within-puzzle day correlations for this feature. This suggests that there may be an overall difficulty threshold that largely dictates the impact that wordplay will have on solve speed.* 
  

#### *Past Performance Features*

**<h4>Figure 19. GMS Adjusted Recent Performance**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/320ff2a4-5f6e-4638-9593-8630ddf562b7)
*<h5>GMSTs and 'GMS Recent Performance Baseline' had a very strong positive correlation for 15x15 puzzles (r= .86).<br>

*<h5>The lowest puzzle day correlation for the GMS (.18) was for Thursday. This finding makes intuitive sense, as Thursday is arguably the most heterogenous puzzle day of all, with nearly every puzzle involving a gimmick of some sort (e.g., rebuses of various flavors). Monday having an extremely high correlation relative to the other puzzle days was also unsurprising, given how few "degrees of freedom" there are in those puzzles. There's just not a lot of room for surprises, so a fast Monday solver will just continue to be a fast Monday solver, more or less. Ditto for Tuesday having the second highest correlation, and a gap can be seen in the all 15x15 scatterplot, essentially delimiting Monday and Tuesday puzzles from the remainder of the 15x15 puzzle days. The strengths of the later week puzzle day correlations are relatively low compared to Monday and Tuesday, which reflects the heterogeneity evidenced across both the GMS solve time distributions (see Figs. 2 and 3) and the feature distributions laid out in Figs. 8-19.*

*Finally, it does bear mention once again in the context of "past" performance that I had to assume that the GMS largely solved puzzles in the sequence of their issue. The more out of sequence solving that occured for the GMS, the less accurate the assumptions that went into calculation of RPB were. I will also reiterate here that this assumption was NOT necessary for the individual solvers for whom I link analyses in the Introduction, as I did have access to their completion timestamps per solve.*


## Supplementary Figures

**<h4>Supplementary Figure 1. Correlation Heatmapping of GMS Individual Puzzle Performance vs Grid, Answer and Past Performance Features by Puzzle Day (15x15 Puzzle Days)**
![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/8890427c-19e9-4972-8570-7cb999aa634e)


**Supplementary Figure 2. Number of Rebus Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/1b064ce6-02de-4047-88d8-2861f30c63ae)

*<h5>Only Sunday and Thursday had an appreciable '# Rebus Squares' for the set of GMS solves. Rebus squares are those that must be filled with more than one letter, number or symbol for a given puzzle to be solved. There were weak-to-moderate positive correlations for both Sunday (r=.23) and Thursday (r=.20). The directionality of the correlations do make intuitive sense, as both their existence and the "rules" for any given rebus can often take a little while to figure out. Additionally, they increase solve time by some degree simply by requiring additional fill and menu toggling relative to a non-rebus puzzle. One caveat here is that the very large number of 0 rebus puzzles, even on Sunday and Thursday, make the strength of these correlations hard to interpret (ie, these are not exactly continuous distributions).*<br>
 
**<h4>Supplementary Figure 3. Number of Circled Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/a12a9253-5407-4a1b-9438-59ec3231c07a)
*<h5>Circled squares were virtually non-existent on the tougher (Fri and Sat) puzzle days. The modest negative correlation seen across all 15x15 puzzle days (-.13) is attributable to the fact that most 15x15 puzzles with circles appeared early in the week. The smattering of puzzles with circles on Sunday almost all fell in the middle of the solve time range regardless of "# Circles", indicating that this feature didn't likely have a major impact on solve times.*

**<h4>Supplementary Figure 4. Number of Shaded Squares vs GMS Solve Time by Puzzle Day**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0be7c072-e115-48ea-8f3b-af76a1022974)
*<h5>Shaded squares, like circled squares, were virtually non-existent in the tougher (Fri and Sat) puzzles. Also like with circled squares, their function is to reveal a puzzle theme and their presence may provide assistance to solvers on clues in which they are embedded. Most puzzles with shaded squares were within the bottom third of GMS 15x15 puzzle solve times, most likely due to shaded squares almost exclusively showing up only in early week puzzles. Also as with '# Circles', the smattering of Sunday puzzles almost mostly fell in the middle of the solve time range regardless of "# Shaded Squares", indicating that this feature also isn't likely having a major impact on solve times.* 
 
## Supplementary Tables

**<h4>Supplementary Table 1. Features Included in Puzzle Features Principal Component Analysis**

![image](https://github.com/ursus-maritimus-714/NYT-XWord-EDA-Global-Median-Solver/assets/90933302/0ef75602-216c-4a0d-b84e-e32723f7f847)



