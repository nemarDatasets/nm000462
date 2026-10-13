[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000462-blue)](https://doi.org/10.82901/nemar.nm000462)

# stim-seizures (Pennsieve 455): processed data, derivative BIDS-iEEG

This is a **derivative** dataset. It repackages the processed data released by Ojemann, Litt and Conrad on Pennsieve Discover
("stim-seizures", dataset 455 version 1, doi:10.26275/toip-zio0, Penn CNT) for the study
"Can electrical stimulation replace spontaneous seizures?" (preprint: Ojemann et al., medRxiv 2025, doi:10.1101/2025.08.29.25334082;
later version titled "Automated annotation of low-frequency stimulation-induced seizures uncovers seizure generating networks").
The release contains metadata and processed data checkpoints for the manuscript's analyses and figures. It contains **no raw iEEG
recordings**. Code: https://github.com/penn-cnt/stim-seizures-manuscript

Patients were evaluated with intracranial EEG at the Hospital of the University of Pennsylvania (HUP) and the Children's Hospital of
Philadelphia (CHOP) and received low-frequency (1 Hz) electrical stimulation. Related NEMAR dataset: nm000402 (Multi-Expert Seizure
Annotation, Penn CNT / CHOP, shared authors). Raw HUP recordings of this group are hosted on iEEG.org (recording names in the
`recording` column, e.g. HUP224_phaseII).

## Contents

- `participants.tsv`: all 105 patients of the release (55 HUP, 50 CHOP). Columns
  come from the released `METADATA/stim_seizure_information - metadata-4.csv` (HUP) and `METADATA/CHOP_metadata.csv` (CHOP),
  copied as released (see participants.json). HUP sex codes 1/2 are mapped to M/F as in the authors' `code/metadata_notebook.ipynb`.
  Age at recording is released only for CHOP patients (rounded here to 0.1 y). 72 of the listed patients appear only
  in the metadata tables (no seizure rows and no processed files), so they have no subject folder.
- `sub-<label>/ieeg/sub-<label>_seizures.tsv` (33 patients; listed in `.bidsignore`, not a BIDS file type): one row per
  seizure from two released tables: `stim_seizure_information_BIDS.csv` (clinician-marked times: approximate_onset, offset (= `end`),
  EEC, UEO in seconds from the start of the iEEG.org recording; stim flag, stimulation channels, typical semiology, mtl) and the
  optimized LSTM model annotations (`optimized_predicted_channels_LSTM_tuned_thresholds_v4_sz-mean_pt-mean_smooth-med.pkl`:
  model, threshold, onset (ueo) and spread (sec) channel sets, strict/loose). Values are copied; lists are space-separated.
- `sub-<label>/ieeg/sub-<label>_electrodes.tsv` + `_coordsystem.json` (31 patients): contact coordinates and atlas labels
  from `PROCESSED_DATA/<patient>/electrode_localizations_dkt.pkl` (HUP) or `electrode_localizations_CHOPR.pkl` (CHOP). The release does
  not state the coordinate space or units, so the coordinate system is `Other` and units `n/a`.
- `sub-HUP224`, `sub-HUP235`, `sub-HUP238` `_task-ictal_acq-<spontaneous|stiminduced>_ieeg.vhdr`: the four seizure traces the authors
  released for figure plotting (`plot_sz_224/235/238.pkl`, `plot_st_sz_224.pkl`): preprocessed bipolar SEEG, 128 Hz (the value used
  in the authors' plotting notebook), HUP224 spontaneous 331.0 s x 153 channels; HUP224 stiminduced 275.0 s x 149 channels; HUP235 spontaneous 371.0 s x 70 channels; HUP238 spontaneous 492.0 s x 69 channels. Amplitude units are not stated in the release (units n/a). Stored as BrainVision
  IEEE float32; round trip checked (float32 values identical to the released float64 values cast to float32).
- `sourcedata/pennsieve-455-v1/`: every released file (pickled DataFrames, HDF5 spread matrices, `stim_info*.mat`, metadata CSVs,
  figure PDFs, Pennsieve manifest/readme). `SOURCE_MANIFEST.json` lists the original SHA-256 of each file. Files are byte-identical
  to the release except file-creation timestamps in MAT headers and PDF metadata, which were set to day 01 (time removed, same length).

## Privacy

All files were scanned (text, CSV, MAT strings, HDF5, pickled DataFrames, PDF metadata) for names, initials, record numbers, full
dates and ages above 89. Patient codes (HUP###, CHOP###) are the pseudonyms used in the public release. Seizure times are seconds from
recording start. No ages above 89. Key status: destroyed (we hold no mapping to identities).

## Licence

CC-BY-NC-SA-4.0, as on the source record (Pennsieve: "Creative Commons Attribution - NonCommercial-ShareAlike"; DataCite rightsList:
Creative Commons Attribution Non Commercial Share Alike 4.0 International, cc-by-nc-sa-4.0).

## Ethics

Verbatim from the preprint (medRxiv doi:10.1101/2025.08.29.25334082, Author Declarations): "The Institutional Review Boards (IRBs) of
the Hospital of the University of Pennsylvania (HUP) and the Children's Hospital of Philadelphia (CHOP) approved retrospective analysis
of EEG and clinical data for all patients. Stimulation was performed under IRB-approved research by the IRB at each institution (14 HUP
patients; all CHOP patients) or clinical protocols (HUP, N = 41)."
