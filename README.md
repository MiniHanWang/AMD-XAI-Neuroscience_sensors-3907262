
A. Environment & Project Layout
A1. Conda environment
conda env create -f env.yml
conda activate amd-prognosis
python -m spacy download en_core_web_sm

A2. Suggested repository layout
project-root/
  config/
    preprocessing.yml
    logging.yml
    kg_mapping.csv
  data_raw/
    text/                 # raw exported XML/JSON
    dicom/                # modality subfolders or per-patient folders
  data_interim/
    text_json/
    images_png/
    sidecars/
    qc_reports/
  data_processed/
    tables/
      patients.parquet
      text_features.parquet
      imaging_features.parquet
      outcomes.parquet
  scripts/
    text_preprocess.py
    image_qc_export.py
    harmonize.py
  Makefile
  README.md
B. Configuration Files
B1. Preprocessing master config
config/preprocessing.yml
B2. Logging config
# config/logging.yml
B3. Knowledge-graph mapping (illustrative rows)
# config/kg_mapping.csv
feature,relation,target,confidence
drusen_volume,increases,ga_risk,0.8
srf,predicts,active_namd,0.9
hrf_density,increases,progression_risk,0.7
ez_disruption,increases,vision_loss_risk,0.85
ffa_late_leakage,predicts,active_cnv,0.9

C. Minimal, Reproducible Scripts
C1. Text preprocessing (de-ID, tokenization, NER, concept stubs)
# scripts/text_preprocess.py
C2. Imaging QC + DICOM→PNG export with sidecars
# scripts/image_qc_export.py
C3. Multimodal harmonization (join on StudyID–Eye–Time)
# scripts/harmonize.py
C4. Makefile (one-command pipeline)
# Makefile
CONFIG = config/preprocessing.yml

.PHONY: all text image qc harmonize

all: text image harmonize

text:
	python scripts/text_preprocess.py --config $(CONFIG)

image:
	python scripts/image_qc_export.py --config $(CONFIG)

harmonize:
	python scripts/harmonize.py --config $(CONFIG)
Professed data information_demo is about Imaging_Features demo.csv, lmaging_QC_Summary_demo.csv, Outcomes_demo.csv and Text Feaures demo.csv, Annotation Guidelines，Annotation Grading Form，Ontology / ID Dictionaries，Data Dictionary for All Features and QC Reports (Per Modality, Per Eye, Per Time Point)

