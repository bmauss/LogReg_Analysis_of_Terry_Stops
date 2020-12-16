# Seattle_Terry_Stops
A Logistic Analysis of Terry Stops

## Objectives
* Determine which modeling method performs best for this dataset.
* Determine if there is a relationship between Terry Stops and a subject's race
* Do the differences in races of the police officer and the subject play a role?
* How do Terry Stops in Seattle compare to the circumstances around Terry v. Ohio?

## What modeling algorithm works best for this dataset?
We had a few contenders.  Decision Trees had perfect precision, recall, accuracy, and F1 scores.  XGBoost and Random Forest had scores that were just as good.  In the end, the decision came down to feature importance.  The only model that gave us any real information was our Random Forest model.

![github](https://raw.githubusercontent.com/bmauss/LogReg_Analysis_of_Terry_Stops/master/Images/images/dec_tree_feats.png)

Decision Tree and XGBoost only used the feature "Stop Resolution".  Random Forest was the **only** high performing model that considered other variables.  The reason for this is simply that **the nature of the algorithm** made it so that **some of the trees** in the forest **could not rely on "Stop_Resolution"** since that column wasn't assigned to them.  Thus, those trees in the forest were forced to find correlations in other features.

So the question then is ***why did the other models rely so heavily on "Stop Resolution"?*** Well this is an accidental by-product of a distinction made during preprocessing.  Some rows in the dataset would have the value "Arrest" under the column "Stop Resolution", but under the column "Arrest Flag" (our target variable) it would have the value "N".  We looked into the description of the columns and saw that an "Arrest Flag" value of "Y" meant that a **physical** arrest took place.  After a little research we discovered that an Arrest takes two forms: **Non-Custodial** (giving the subject a citation) and **Physical** Arrest (subject is taken into custody of the officer).  To more easily **distinguish** between **Non-Custodial** Arrests and **Physical** Arrests during data analysis, we **renamed** of all Stop Resolutions with a value of "Arrest" but **did not have an Arrest Flag** (indicator of a physical arrest) to **"Non-Custodial"**.  Consequently, this created a **100% correlation** between the **Stop Resolution value "Arrest"** and our **target variable** "Arrest Flag". This is the reason why our models were so accurate.

## Is there a relationship between Terry Stops and a subject's race?

![github](https://raw.githubusercontent.com/bmauss/LogReg_Analysis_of_Terry_Stops/master/Images/images/racerelations.png)

In short, yes.  Native Americans and African-Americans are stopped disproportionately to their respective populations.  Native Americans are stopped the most with African Americans coming next.  On the surface level, it definitely looks as though racial discrimination plays a factor in who will be subjected to a Terry Stop.  There is still a couple of problems, however.  One is: **Who is perpetuating this discrimination?**  The dataset **does not contain enough information** on whether the Terry Stop was **initiated** by an **officer** on the street, like in *Terry v. Ohio* or if it was **in response to a citizen** who called in reporting a suspicious person. After all, if it is in response to a citizen's complaint, an officer can only go off of a description given by the citizen.  The other problem is that we don't have enough information to determine if this is just a glimpse into a much more complex social issue: crimes commited as a result of socio-economic inequality. 

### Recommendation:
First, you may want to consider reinstating the "Freedom Patrols" of 1965, where members of the community join police officers and observe the officers' activities while working their Beat. While the details of the citizen's safety will need to be worked out, Freedom Patrols can lead to an increase in public trust, a better understanding of the people in their jurisdiction, as well as educate people about why certain policies are in place.

Next, the reporting system should be updated to differentiate between officer initiated stops and stops that are in response to civilian reports.  After more data is gathered, we can reassess and develop a plan.

## Do the differences in races of the police officer and the subject play a role?

![github](https://raw.githubusercontent.com/bmauss/LogReg_Analysis_of_Terry_Stops/master/Images/images/same_race.png)

Yes, but in a surprising way!  Seattle officers that are of a different race from the subject are less likely to arrest or frisk them.  There could be a couple of reasons for this.  One is that an officer whose race is different is more hesitant and doesn't want to risk the possibility of their actions being considered racist.  Another explanation is that the officers in Seattle are assigned to beats where the local demographics match their own.  Either way, assuming that these arrests and frisk searches were warranted, it's good to see that the enforcement of the law is roughly equal. 

From a racial relations perspective, there isn't much of a problem, but a concern is raised when we look at the gap between percentage of people arrested and percentage of those who were frisked.  When we add these columns together, roughly **40% of people stopped are frisk searched**, but only around **10% of people are actually arrested**. We looked a little deeper into this found this:

![github](https://raw.githubusercontent.com/bmauss/LogReg_Analysis_of_Terry_Stops/master/Images/images/arrests%20vs%20frisk.PNG)

When you filter out the people who are both arrested and frisked, you can still see this gap.  If people were carrying something illegal, they'd probably be arrested and that gap would be smaller.  If we were to assume that these stops were made under the same circumstances as *Terry v. Ohio*, we would make the assumption that a majority of people are having their 4th Amendment Rights violated.  As mentioned before, the **gap** between the **number of people frisked** and **number of arrests** is **indicative** of this.  This also hurts public perception of the police considering that these subjects are still **racially disproportionate**.  

### Recommendation:
Our **recommendation is for officers to show more restraint** in performing frisks until they have sufficient evidence based on the their **observations of the subject's behavior**.

## How do Terry Stops in Seattle compare to the circumstances around Terry v. Ohio?

In ***Terry v. Ohio***, Officer McFadden observed two men look through a store window, walk down the street, and then turn around, walk back to the store and peer through the window again.  They did this several times.  McFadden suspected that the men were casing the store before they robbed it.  He then stopped and frisked the men.  He discovered they were illegally carrying a handgun and arrested them.  Terry stops became synonymous with "stop and frisks".

In Seattle, Terry Stops take on a much broader definition.

![github](https://raw.githubusercontent.com/bmauss/LogReg_Analysis_of_Terry_Stops/master/Images/images/stopresolutions.png)

A **large majority** of officer interactions with subjects falls under the category of obtaining an **offense report**. Again, activities associated with this are **interviewing witnesses** and **obtaining information from victims** of crimes.  The next biggest outcome of a Terry Stop is a subject being issued a citation (**non-custodial arrest**).  These include traffic citations.  A field contact can be anything from community outreach to frisking someone.  These definitions are very broad and make if difficult to classify what leads to an arrest.  These circumstances also don't resemble those surrounding *Terry v. Ohio*.  We've also seen that the officers in Seattle appear to be a little to quick to perform frisk searches.

### Recommendation:
Education.  **Invite behavioral analysts** and experts in the **field of Criminal Behavioral Science** to come and educate officers.  Doing so will **reduce** the number of people being unnecessarily frisked and will go a long way towards improving public perception.  

# Conclusion
To summarize our findings and recommendations:
* Seattle's **definition of a "Terry Stop"** is much **too broad** and barely resembles the circumstances surrounding their origin.  In order to improve public perception, the **data needs to become more precise** in what it is reporting.  This means **making distinctions** between **officer initiated** encounters and those that came as a result of a **citizen's report**.

* The treatment of officers towards subjects that are of a different race is slightly better than officers of the same race, but all of the officers could **benefit from a more reserved attitude toward frisking subjects**.

* To that end, we encourage **inviting experts in Criminal Behavior Science** to give lectures and **educate your officers** (possibly yearly) so that the number of innocent individuals being frisked is reduced. 

* Lastly, you may want to consider **reinstating the "Freedom Patrols" of 1965**, where **members of the community join police officers** and observe the officers' activities while working their Beat.  While the details of the citizen's safety will need to be worked out, Freedom Patrols can lead to an **increase in public trust**, a **better understanding** of the people in their jurisdiction, as well as **educate people** about why certain policies are in place. 
