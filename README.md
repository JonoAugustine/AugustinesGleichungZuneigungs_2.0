# Augustines Gleichung Zuneigungs 2.0

**The second attempt at creating the ultimate decision making tool for the troubled mind**

*Using Unreal Engine 4 as a base to build instead of Java*

-----------------------------------------
----------------

**Roadmap:**
---------
- Possible Redesign of Candidate Range Calculation
	+ In its current state (3/20.32a1ea8), SB calculates the possible range of candidate scores once
		  all features and candidates have gone through their bordas.
	+ It works by sorting the features & their respective weights, then multiuplying through
		  the min candidate-feature score to the max by their respective weight. However, it
		  is now apparent that the method of itteratting by one each time through the candidate
		  scores might not be the right way.
		  In Theory, the candidate-feature scores should never be more than +/- 1 unit from the 
		  feature below and or above it in the array. That is why It has been working. However, while
		  attemptint to implement Negative Traits appraoch B, the min has been calculated incorreclty.
		  The current method of iterating from in to max by 1 and multiplying may be the culprit.
		  Tests will now be performed to determine if this is the source of the problem.
	+ **NOTE** that with the addition of negative features, feature weights are no longer consecutive.
		However, since weights are universal across all candidates, this should not matter.
		  
	- Approach A:
		 + Since it is still possible to obtain scores that are not consecutive, we can iterrate
			through the candidates and add each score to a temp array (no repeates), sorting them upon
			addition, then use that array to calculate max and min scores.
			
- Negative traits
	+ In SB's current state, features cannot be discerned between the type of effect they
		produce, only the magnitude. However, human opinions are not all of the same quality,
		some features are seen as negative while others are positive. SB (as of 3/19/18) is
		unable to include these aspects in its calculations.
		In order to add the aspect of positivity dichotomy to features in SB, the S_Feature
		structure (located in the Blueprints folder) must be modified to enable the use
		of negativity in calculations. Here are listed a rough outline as to how this modification
		will be approached:
		- Approach A:
			+ The struct S_Feature will be given a boolean: negative
			+ Negative feature scores will default to -1
			-  A new feature list of negative features will be added to feature setup
			+ On the confirmation of features & candidates. Feature weights will be added
				in value according to the feature's negativity.
			+ On calibration and Candidate Borda (CnB), the feature will be increased or decreased
			  on selection based on the feature's negativity.
			+ The rest of the code should be able to stay the same (cf)
			
		- Approach B:
			+	The struct S_Feature will be given an int called "negative".
			+ "Negative" will be set to 1 or -1.
			+ Negative feature scores will default to -1
			+ A new feature list of negative features will be added to feature setup
			+ On the confirmation of features & candidates. Feature weights will be added
				  in value according to the feature's negativity.
			+ On calibration and Candidate Borda (CnB), the feature will be increased regardless 
				  of negativity, then multiplied by the feature's negativity int.
			+ The rest of the code should be able to stay the same (cf)
			
- Candidate-specific Features
	-	In AGZ_1, and (as of) AGZ_2.0.72d634d, the inability to compare candidates that do not share
		  all of their features has been a limitation on the variaty of candidates that can be compared.
		  Expanding how AGZ understands candidates may be an interesting path to take. To be more specific,
		  adding the ability to calibrate from a single pool of features and have Candidates draw some (*but not necessarily 				all*) features from that pool.
	-	A possible complication with this expansion would involve a fundimental change to how bordas are
		  run in AGZ's code. If all features are calibrated within the same array (*feature weights* in Pii),
		  then making candidates with only some features will make issues while calculations for candidate
		  borda and min-max score range.
	- One way around this would be to set any of these (let's call them) "*null*" Cn-features to 0 within the respective candidate. This brings into question the meaning of a "0" score feature.
		- Prior to negative features, a feature of score 0 would mean it was poor, since a potentially high weight was zeroed out. 
		- Since the introduction of negative features, a Cn-feature score of 0 could equally zero out a negative weight as a positive one, so (hypothetically) it should be more of a balanced influence.
		- However the question remains: *should candidates without a certain feature be judged on the same scale as those with said feature, regardless of negativity?*
			- Consider this scenario:
				- 2 candidates: Phone & Computer
				- List some descriptive features of either: 
					- Phone calls, video editing, steam games, texting, portability
				- Let's assume you don't have some program that lets you make calls or text from a computer. Clearly, phone calls and texting will be at the bottom of the borda ranking for the Computer, so far down in fact, they should be considered automatic zeros (hint hint). Similarly, you wont be playing any steam games or doing heavy video editing on the phone, so those can also be automatic zeros. Portability, on the third hand, easily fits into both candidates feature box. 
				- Calibrating the entire set of features, regardless of where they will apply later on, and then giving bordas for the 2 Cn, should give appropriate scoring in the final calculations.
				- **A possible issue with this example** would be the range calculations. How would the range be interpreted if the perfect candidate would have features that span across the features that do not go into both boxes and therefore "cannot" exist?
			- Consider this scenario
				- 2 candidates: Computer & New Counter-top

***List of known bugs***
-----		
- UI:
	- Log Viewer scroll box does not scroll to fit expanded log entries

**Changelog**
--
3/24/18
- Possibly fixed a major bug/misimplementation of Cn score range calculation. In the Pii function *CalcCandidateScoreRange*, when calculating the *minimum* score, iterates forwards through the (temporarily) sorted *featureWeights* array. 
	- **2.0.02add04 and earlier** On each iteration a *min-temp* was multiplied by the loop's current weight, then the min-temp was incremented by 1. Although it seemed to be implemented correctly, this method caused constant miscalculations of the Cn score range minimum. 
	- **As of 2.0.508dd9c**  The min-temp in the Cn score range minimum's calculation was changed to the max-temp (along with a decrement each iteration instead of increment). This appears to produce correct calculations of the Cn score minimum.


Definitions
-----------------------------------------------------------------------------

**Candidates: the object of your indecision**

- Whether people, watches, clothes, insurance plans, or quite literally anything else. AGZ's purpose is
to aid you in understanding how you feel about about these candidates. 
- Candidates consist of a name, score, and set of features.

**Features: aspects of candidates**
- Features help AGZ understand what is most important to you. They can be anything from "*red*" to "*votes for president of space*" to *likes kittens*; as long as it describes some aspect of a candidate, it's a valid feature. 
- Features consist of a name, score, and negativity (+-1)

**Bordas: The solution**
- The Borda is the system by which Features and Candidates are scored and ranked against each other. A series of binary choices that help the program (as well as the user) understand where Features & Candidates stand in respect to each other. By presenting only two options in each comparison, the Borda is able to remove much of the overwhelming and confusing aspects of comparing and ranking several choices at once.

**Random terminology**
----
- Pii = Program Info Instance
- Cn = Candidate
- F, Feat, Ft = Feature
- Version numbers are followed by commit IDs. e.g. 2.0.508dd9c
