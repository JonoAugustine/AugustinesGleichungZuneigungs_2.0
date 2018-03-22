# AugustinesGleichungZuneigungs_2.0
**The second attempt at creating the ultimate decision making tool for the troubled mind**

*Now using Unreal Engine 4 as a base to build instead of pure Java*

-----------------------------------------------------------------------------

**Roadmap:**

	- Possible Redesign of CAndidate Range Calculation
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
		  
		- Attempt A:
			Since it is still possible to obtain scores that are not consecutive, we can iterrate
			through the candidates and add each score to a temp array (no repeates), sorting them puon addition,
			then use that array to calculate max and min scores.
			
	- Negative traits
		In SB's current state, features cannot be discerned between the type of effect they
		produce, only the magnitude. However, human opinions are not all of the same quality,
		some features are seen as negative while others are positive. SB (as of 3/19/18) is
		unable to include these aspects in its calculations.
		In order to add the aspect of positivity dichotomy to features in SB, the S_Feature
		structure (located in the Blueprints folder) must be modified to enable the use
		of negativity in calculations. Here are listed a rough outline as to how this modification
		will be approached:
			Approach A:
			+ The struct S_Feature will be given a boolean: negative
			+ Negative feature scores will default to -1
			+ A new feature list of negative features will be added to feature setup
			+ On the confirmation of features & candidates. Feature weights will be added
			  in value according to the feature's negativity.
			+ On calibration and Candidate Borda (CnB), the feature will be increased or decreased
			  on selection based on the feature's negativity.
			+ The rest of the code should be able to stay the same (cf)
			
			Approach B:
			+ The struct S_Feature will be given an int called "negative".
			+ "Negative" will be set to 1 or -1.
			+ Negative feature scores will default to -1
			+ A new feature list of negative features will be added to feature setup
			+ On the confirmation of features & candidates. Feature weights will be added
			  in value according to the feature's negativity.
			+ On calibration and Candidate Borda (CnB), the feature will be increased regardless 
			  of negativity, then multiplied by the feature's negativity int.
			+ The rest of the code should be able to stay the same (cf)
			
	- Candidate-specific Features
		+ In AGZ_1, and (as of) AGZ_2.0.72d634d, the inability to compare candidates that do not share
		  all of their features has been a limitation on the variaty of candidates that can be compared.
		  Expanding how AGZ understands candidates may be an interesting path to take. To be more specific,
		  adding the ability to calibrate from a single pool of features, while 
		+ A possible complication with this expansion would involve a fundimental change to how bordas are
		  run in AGZ's code. If all features are calibrated within the same array (*feature weights* in Pii),
		  then making candidates with only some features will make issues while calculations for candidate borda
		  and min-max score range
		  


***List of known bugs***

	Borda:
		~~ A borda of 2 candidates and 2 features will result in a 100% and a ~90%~~
			~~for candidates that should have a score of zero.~~
		
	
	UI:
		- Log Viewer scroll box does not scroll to fit expanded log entries


-----------------------------------------------------------------------------

**Candidates**

	Candidates are the object of your indecision. Whether they are people, watches, clothes, insurance plans, or
	quite literally anything else. AGZ's purpose is to aid you in understanding how you feel about about
	these candidates. Candidates consist of a name, score, and set of features.

**Features:**

	Features are aspects of candidates that help AGZ understand what is most important to you. Features can be
	anything from *red* to *votes for president of space* to *likes kittens*; as long as it describes some aspect
	of a candidate, it's a valid feature. In AGZ_2.0.72d634d, Features consist of a name and score. The score of each
	feature fits

**Bordas:**

