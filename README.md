# Note on Submission<!-- omit from toc --> 

- You should submit to Canvas this week for this activity set.

---

- [Activity 4.1: Exploring ChimeraX](#activity-41-exploring-chimerax)
  - [Description](#description)
  - [Steps](#steps)
- [Activity 4.2: Homology Modelling with Swiss Model](#activity-42-homology-modelling-with-swiss-model)
  - [Background](#background)
  - [Description](#description-1)
  - [Steps](#steps-1)
- [Activity 4.3: Docking using Webina (A port of AutoDock Vina)](#activity-43-docking-using-webina-a-port-of-autodock-vina)
  - [Background](#background-1)
  - [Description](#description-2)
- [Activity 4.4: Interpreting Molecular Dynamics Plots](#activity-44-interpreting-molecular-dynamics-plots)
  - [Background](#background-2)
  - [Description](#description-3)

## Activity 4.1: Template Free Folding on RNA

### Description

In this exercise, you will perform an ab initio folding of an RNA sequence using a web server called [RNAComposer](https://rnacomposer.cs.put.poznan.pl/). In the interest of comparing the folded model to an actual structure you will use the [RCSB](https://www.rcsb.org/) to obtain a structure of the same sequence.

### Steps

1. Navigate to the [RCSB](https://www.rcsb.org/) and find `1EC6` (this is a complex that includes the structure of the RNA sequence you will fold).
2. Retrieve the fasta record associated with the RNA molecule.
3. Navigate to the [RNAComposer](https://rnacomposer.cs.put.poznan.pl/) web server.
4. Paste the fasta record into the interactive mode window.
   1. Rename the header portion of the record with a name of your choice. Do not include any spaces but keep the `>` at the start.  The original RCSB name will raise an error.
   2. Add `RNAfold` as a final third line.  This is a web server specific directive that will run the RNAfold program as part of the overall modeling process.
5. Hit `Compose` to start the modeling.  Runtime should be minutes at most.
6. Download the resulting model pdb file.
7. Compare the model against the structure in complex
   1. Open ChimeraX
   2. Load the model pdb file
   3. Load 1EC6 (`open 1ec6`)
   4. Run `tools -> structure analysis -> matchmaker`
      1. In matchmaker window
         1. Select `Alignment` tab
         2. Select `Nucleic` from matrix dropdown
      2. Select `OK`
         1. Matchmaker will now run.  Read the [documentation page](https://www.rbvi.ucsf.edu/chimerax/docs/user/tools/matchmaker.html) for Matchmaker to understand the major steps.
8. Report the following:
   1. Include an image of the superimposed structures.
   2. Answer the following questions:
      1. Is RNAComposer an ab initio method?
      2. What does Matchmaker perform before superimposing the structures?
      3. If we did not set the matrix to nucleic, the default was `blosum62` which would raise an error. Why would `blosum62` raise an exception for this comparison?
      4. Are all nucleotides in the model superimposed on the experimental structure? Why or why not? (Hint: Is it solely a modeling issue or is there something else biologically significant going on?)


---

## Activity 4.2: Homology Modelling with Swiss Model

### Background

Homology modelling attempts to predict the structure of a query primary sequence by comparison to a database of known structures.

### Description

In this exercise, you will use the Swiss Model server to predict the structure of a protein already in the RCSB.  Effectively this is a positive control on the approach since we know an existing experimental structure exists for comparison.

### Steps

1. Navigate to the [RCSB](https://www.rcsb.org/)
2. Obtain the fasta sequence for the entry with RCSB accession ID: '4hjy' (this can be obtained from the 'Download Files' dropdown menu on the right hand side of the 4HJY entry page)
3. Navigate to the [Swiss Model website](https://swissmodel.expasy.org/interactive)
4. Paste in the contents or upload the fasta file as the input
5. Add an email and project name, while both are optional, it is often advised to include them, especially when a server job may take a while to complete
6. Start the modelling by clicking 'Build Model'
7. This server takes around ~15 minutes to complete
8. Once finished, you should have two models built.
9. Using the documentation: https://swissmodel.expasy.org/docs/help
10. Report the following for each model
    1.  The template used
    2.  The sequence identity
    3.  Coverage (the range modelled of the full query sequence)
    4.  GMQE
    5.  QMEANDisCo Global
    6.  Your conclusion on whether the model is reliable and why?
11. For the best model, we will now check against the actual '4hjy' structure.
    1.  Download the pdb file for the model![](images/SwissModel.PNG)
    2.  Load the pdb file into ChimeraX along with 4hjy
    3.  Perform a matchmaker between the model and the experimental structure
        1.  Note: the model is only one chain of the two chain protein so you should see the model superimpose onto one of the two chains
    4.  Export an image and answer the following question
        1.  Does the model appear to be largely consistent with the experimental structure why or why not?
12. Upload the writeup, figure and figure caption to Canvas

---

## Activity 4.3: Running AlphaFold2 in Colab

### Background

In this exercise, you will run the AlphaFold2 protein structure prediction software in Google Colab.  You will also modify the default query sequence to see how the prediction changes.

### Description

1. Navigate to the [colab notebook](https://colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb)
2. Run the notebook as is.  While the notebook is running, read the Alphafold database [FAQ](https://alphafold.ebi.ac.uk/faq) specifically focusing on question `How confident should I be in a prediction?`.
3. Open a new instance of the notebook (we will be comparing between the original and this modified version), this time modify the query sequence by adding a string of Alanines to the end of the query (about 5-10 should be good).
4. Rerun the notebook.
5. Comparing the two predictions, answer the following questions:
   1. Which prediction is overall more confident?
   2. What regions of the modified query are predicted with low confidence?
   3. What is the secondary structure of the modified region (polymeric Alanine) in the prediction?
   4. What does the `Sequence coverage` plot denote?

---
## Activity 4.4: Docking using Webina (A port of AutoDock Vina)

### Background

Molecular Docking can be used to predict energetically plausible interactions between proteins and ligands.  Inputs are the two molecules of interest and often times a designation about the region of the protein to search interactions against.  In the exercise, you will use a web server called [Webina](https://academic.oup.com/bioinformatics/article/36/16/4513/5860016).

A technical note: In contrast to most web servers where the task is computed on the server computers, this web server takes advantage of an emerging technology called ["Web Assembly"](https://webassembly.org/) to compute the task on your machine through a web browser interface.  An advantage of this approach is the software setup is still done for the user but the actual compute resources are not subject to being removed by the maintainer.  Google Earth and other "applications" on the web use a similar approach and I suspect this method of delivering software is going to grow in scientific usage.

This activity will use [4TN6](https://www.rcsb.org/structure/4TN6) as a test case. This is a CK1d protein in complex with an inhibitor, PFO.  You will use Webina (the server) to perform docking with AutoDock Vina (the underlying docking software).  After getting results, you will how assess the docking complex in comparison to the experimental ligand location.

### Description

The input files have been prepared for you (using ChimeraX) and are as follows:
1. Receptor: [4tn6A_no_ligand.pdb](webina_input_files/4tn6A_no_ligand.pdb) , A pdb file of 4tn6 reduced to the A chain and with the ligand removed
2. Ligand: [PFO.pdb](webina_input_files/PFO.pdb) , A pdb file of the ligand PFO alone, not from 4tn6 (we do not want to bias dockig by giving the correct ligand conformation)
3. Correct Pose: [PFO_experimental.pdb](webina_input_files/PFO_experimental.pdb) , A pdb file of the ligand PFO alone from 4tn6, this will not affect the docking but Webina can display it for comparison of docking to experimental location.

1. Navigate to [Webina](https://durrantlab.pitt.edu/webina/)
2. Input the files as indicated above. You will get a pop-up about Converting the file to PDBQT format, keep all defaults and select Convert
   1. Additional pop-ups about "File Too Big" can be ignored in this example
3. Next observed the "Docking Box" section, this allows the user to select the region on the receptor the ligand will check.
   1. Selection of the docking box is something informed by the goal and external information e.g. for screening you likely know an existing ligand binding pocket to enclose in the Docking Box, tools like [prankweb](https://prankweb.cz/)
   2. In this activity, I will provide the docking box parameters as follows:
      1. Box Center
         1. x: -20
         2. y: -3
         3. z: 42
      2. Box Size
         1. x: 20
         2. y: 20
         3. z: 20
4. In the "Other Critical Parameters" section
   1. Set CPUs to 2 (if you have issues with CPUs=2, set it to 1)
   2. Set Exhaustiveness to 8 (this is an AutoDock Vina Parameters that effectively sets the granularity of the search, high values tend to be accurate but take more time to run)
5. Start the docking, it will likely take a few minutes to finish (Note: I recommend keeping the tab in the foreground, some browsers will put background tabs to sleep which will break the computation!)
6. On the Output page, "Visualization" section rotate the image to display the docking complex
   1. The Yellow ligand is the experimental "correct" pose, the grey one is the docked ligand
   2. Click through the mode rows and observe the docked ligand change position, this is because each mode represents a plausible solution.
   3. Take a screenshot of the mode you think is most consistent with the experimental ligand to include in your upload. (example) ![Webina_output](images/Webina_Output.PNG)
7. Answer the following questions:
   1. Which binding mode is most energetically favorable according to the docking and what is the energy?
   2. What does "Dist From Rmsd L.B." and "Dist From Rmsd U.B." mean This is output from AutoDock Vina so the [manual](https://vina.scripps.edu/manual/) is handy.
   3. Which binding mode looks the most similar to the experimental solution? (the one you took a screenshot of)
   4. If the binding mode that looks most similar is not the most energetically favorable, what does imply about an approach where you only analyze the best energy result?
8. Upload the answers to the questions and screenshot for this activity.

---