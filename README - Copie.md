# CATHEDRAL
***CATHEDRAL is a python-based script designed to compare candidates generated from different annotation and classify according to pre-established confidence levels strategies.***

## Installation
Pre-requisite :
  - Python (you should install *Anaconda Platform* - https://www.anaconda.com/download)
  - Jupyter Notebook application (*Anaconda Platform*)
  - RDKit environment (*c.f.* https://www.rdkit.org/docs/Install.html)
  - Several libraries must be installed in the RDKit environment for the script to work:
     1. In an Anaconda Prompt, head for CATHEDRAL folder path: ```cd C:/.../../../../CATHEDRAL```
     2. Activate your RDKit environement: ```conda activate my-rdkit-env```
     3. Write the command line: ```pip install -r requirements.txt``` 
*please, check requirements.txt file is in the CATHEDRAL folder*

![Plan de travail 11080](/pictures/cathedral_requirements.png)

## Prepare the pre_ranked_features.csv:
   1 - Metadata from FBMN / IIMN, NAP and MolNetEnhancer workflows must be merged via Cytoscape software, according to the corresponding keys:

![Plan de travail 11080](/pictures/cathedral_merge.png)

   2 - Resulting metadata table must be exported as pre_ranked_features.csv file.
   3 - New column (called PRED) is created in the dataframe:
       **if feature have FBMN / IIMN annotation, PRED=1
       **if feature have NAP (Metfrag) annotation, PRED=2
       **else PRED=3

![Plan de travail 11080](/pictures/cathedral_merge_pred.png)

## Run the script
### Open the script:
1. Open Anaconda Prompt
2. Activate RDKit environment (conda activate my-rdkit-env)
3. Go to the folder containing CATHEDREAL.ipynb (cd C:\...\...\...\...\CATHEDRAL)
4. Open Jupyter Notebook application (jupyter notebook)
5. Click on CATHEDRAL.ipynb 

![Plan de travail 11080](/pictures/cathedral_script.png)

### Run the script:
The workflow is detailed, just inform the path to access:
  - pre-scored features according to FBMN, NAP matches resuls (pre_ranked_features.csv)
  - SIRIUS project (must end with /*)
  - NMR candidates file (caramel_compounds.txt)
  - resume file to save results (comparison_results.tsv)
directly by running the appropriate cell.

![Plan de travail 11080](/pictures/input_files.png)

### Just follow the instructions until the end of the notebook
  1 - First, features annoted with FBMN / IIMN (PRED=1) are first selected:
      1.3.2 - the script will highlight the common annotations for GNPS, NAP and SIRIUS worflows (confidence level = 9)
      1.3.3 - annotations that are common only for GNPS and NAP are secondly highlighted (confidence level = 12)
      1.3.4 - ame for annotation that are common only for GNPS and SIRIUS (confidence level = 10)
      1.3.5 - SIRIUS NAP common annotations are finally retrieved (confidence level =11+)

      1.4 - each of previous classified annotations is then compared to NMR compounds:
           1.4.2 - if GNPS NAP SIRIUS common annotation is found in NMR compounds list, confidence level = 1
           1.4.3 - if GNPS NAP common annotation is found in NMR compounds list, confidence level = 4
           1.4.4 - if GNPS SIRIUS common annotation is found in NMR compounds list, confidence level = 2
           1.4.5 - if SIRIUS NAP common annotation is found in NMR compounds list, confidence level = 3+

  2 - Secondly, features that were not annotated with FBMN / IIMN but are with NAP (MetFrag, PRED=2) are considered:
      2.1 - annotations for NAP and SIRIUS are compared for single features (confidence level = 11)
      2.2 - common annotations for NAP and SIRIUS are then compared to NMR compounds list, confidence level = 3

  3 - Thirdly, annotations from individual annotation tool (FBMN / IIMN, NAP or SIRIUS) are compared to NMR compounds list, only for the remaing features that do not have a common candidate through at least 2 different mass annotation tools:
      3.3.1 - FBMN / IIMN vs. NMR (confidence level = 5)
      3.3.2 - NAP vs. NMR (confidence level = 7, for each of the 3 Metfrag candidates)
      3.5.2 - SIRIUS vs. NMR (confidence level = 6)

  4 - If an annotation is common to 2 mass annotation tools, the third one is compared to NMR coupounds list:
      4.1.1 - FBMN / IIMN vs. NMR
      4.1.2 - NAP vs. NMR
      4.1.3 - SIRIUS vs. NMR
      
  5 - Finally, comparison results are summarized in comparison_results.tsv file:
      - Feature_SU: feature ID of the annotation
      - m/z: m/z of the corresponding feature
      - Rt_SU: retention time of the corresponding feature
      - GNPS_SU: ID of the annotation spectra in GNPS spectral library
      - NAP_SU: Name and rank of the MetFrag annotation for the corresponding feature
      - NPClassifier_superclass_SU: Chemical superclass according to NPClassifier ontology
      - SIRIUS_SU: InChiKEY, Formula and adduct type corresponding to the annotation of the corresponding feature
      - Not_Matching_tool_annotation_SU: annotation tool from which the candidate did not match to the others
      - Is_NMR_Annotated_SU: 1 if commonly found in NMR compounds list, else 0 
      - 3rd_Tool_SU: if the third tool match to NMR coumpounds list
      - Molecular_Name_SU: Name of the NMR compound if matched
      - Confidence_Level: the confidence level applied according to the in-house scale

![Plan de travail 11080](/pictures/cathedral_results.png)


      
 
