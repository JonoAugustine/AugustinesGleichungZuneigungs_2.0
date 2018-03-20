# AugustinesGleichungZuneigungs_2.0
**The second attempt at creating the ultimate decision making tool for the troubled mind**

*Now using Unreal Engine 4 as a base to build instead of pure Java*
-----------------------------------------------------------------------------

Roadmap:

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
			


List of known bugs

	Borda:
		~~ A borda of 2 candidates and 2 features will result in a 100% and a ~90%~~
			~~for candidates that should have a score of zero.~~
		
	
	UI:
		- Log Viewer scroll box does not scroll to fit expanded log entries
