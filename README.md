Model Evaluation Notebook 
==========================

Overview
--------

This notebook provides a reusable evaluation pipeline for analyzing LLM predictions on the immigrant-serving organizations dataset. It standardizes the process of running evaluation metrics, exporting error cases, and supporting province-level filtering.

The workflow includes:

*   loading CSV model outputs
*   normalizing label values
*   computing confusion matrix, accuracy, precision, recall, and f1
*   calculating error rates by province
*   analyzing annotator disagreements
*   exporting false positives (FP) and false negatives (FN)
*   optionally filtering provinces (for example, removing Québec)
    
*   evaluating multiple model outputs consistently
    

The main function for running the full evaluation is:

full\_eval(df, pred\_col="prediction label", label="", drop\_provinces=None)

File Requirements
-----------------

Your CSV must include:

*   Label
*   prediction label
*   Province

Optional annotator label columns (automatically detected):

*   Label-Armita
*   Label-Tharu
*   Label-Samaneh
*   Label-Ashley
*   Label-Rimona
    
Optional annotator method columns:

*   Approach-Armita
*   Approach-Tharu
*   Approach-Samaneh
    
If any optional columns are missing, the notebook skips those parts of the analysis automatically.

How to Use the Notebook
-----------------------

### 1\. Load Your CSV

Use:

df = pd.read\_csv("your\_file.csv", dtype=str)

Your dataset must contain the required columns listed above.

### 2\. Run the Full Evaluation

Run:

full\_eval(df, pred\_col="prediction label", label="Model A")

This generates:

*   label distribution summary
*   confusion matrix   
*   classification report (precision, recall, f1)
*   accuracy
*   province-level error rate plot
*   method-level error rate (if method columns exist)
*   annotator disagreement table
*   an exported FP/FN file

The FP/FN export is saved as:

Model A\_hallucination\_review.xlsx

Filtering Provinces (Optional)
------------------------------

To exclude Québec:

full\_eval(df, label="Without QC", drop\_provinces=\["QC"\])

To exclude multiple provinces:

full\_eval(df, label="Filtered", drop\_provinces=\["QC", "nan"\])

Filtering changes only the rows evaluated; all other metrics run normally.

Running Multiple Models
-----------------------

If you have several CSV files to evaluate in one workflow:

files = ["model1.csv", "model2.csv", "model3.csv"]  for f in files:      df = pd.read_csv(f, dtype=str)      full_eval(df, label=f)   `

Each file will produce:

*   printed evaluation metrics 
*   province-level plots 
*   annotator disagreement table
*   an FP/FN Excel export
    

This enables consistent comparison across model versions.

Exported Error Cases
--------------------

Every evaluation automatically produces an Excel file named:

\_hallucination\_review.xlsx

This file contains two sheets:

*   False Negatives (actual yes, predicted no)
*   False Positives (actual no, predicted yes)
    

These sheets are used for manual hallucination/error analysis.

No extra steps are required; export is built into full\_eval().

Core Functions (Summary)
------------------------

### normalize\_label(x)

Standardizes label values to "yes" or "no" for consistent evaluation.

### full\_eval(df, pred\_col, label, drop\_provinces)

Runs the entire analysis, including:

*   province filtering
*   confusion matrix & classification report
*   accuracy
*   province-level error rate plot
*   method-level error analysis
*   annotator disagreement
*   FP/FN export
    

### export\_fp\_fn(df, pred\_col, path)

Writes the false positive and false negative cases to an Excel file.

### annotator\_error\_table(df)

Computes disagreement rates between each annotator and the model.
