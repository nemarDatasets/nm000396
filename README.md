[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000396-blue)](https://doi.org/10.82901/nemar.nm000396)

# Developmental ECoG (Miles, Weaver, Webb, Ojemann 2025): resting-state electrocorticography, ages 3-33

Resting-state intracranial recordings (subdural grids and strips; depth electrodes in some participants) from **13
patients with epilepsy aged 3.4-33.9 years** (6 male, 7 female) undergoing intracranial monitoring at Seattle
Children's Hospital and Harborview Medical Center (Seattle, WA, USA). Re-packaged in iEEG-BIDS from the authors'
public release (Zenodo record [16954184](https://doi.org/10.5281/zenodo.16954184), "Developmental ECoG",
CC-BY-NC-4.0).

Reference article: Miles JT, Weaver KE, Webb SJ, Ojemann JG. *Developmental relationships between the human alpha
rhythm and intrinsic neural timescales are dependent on neural hierarchy.* Journal of Neurophysiology 135:143-152
(2026). https://doi.org/10.1152/jn.00435.2025 (preprint: https://doi.org/10.1101/2025.08.21.671637).

## Participants
`participants.tsv` gives age, sex, implanted electrodes, coverage and epileptiform zones for each participant from
Table 1 of the paper. The release orders participants by age (sub01 = P1, 3.42 years ... sub13 = P13, 33.92 years);
the ages in the release montage files match Table 1 (checked by the converter). `release_id` is the pseudonymous
code of the release folder.

## Task
`task-rest`: dedicated resting-state period. Participants were asked to lie quietly, upright, for at least three
minutes before other research experiments (paper, Methods). There are no events.

## Recording
Sampling rates differ by site and system (512-4800 Hz; release channel tables: 1200 Hz for 9 participants, 4800,
1525.878906, 1220.703125, 1000 and 512 Hz for the others). Filter settings and notch ranges are copied from the
release channel tables into `channels.tsv`. Power line frequency: 60 Hz.

- **Units.** The release channel tables give the ECoG units as `unscaled` for 12 of the 13 participants: the values
  are as exported from the acquisition system and the release does not give a scale factor to volts. `channels.tsv`
  therefore lists `n/a` units for these channels, and the BrainVision header says `unscaled`. Amplitudes are not
  comparable across participants. sub-02's values are in microvolts.
- **sub-02** also has the depth electrodes (`A'`...`S'`, named as in the ROSA planning file), one ECG channel
  (`EKG-Ref`) and one unlabelled channel (`C154-Ref`, type MISC). These are not in the release channel table. Their
  types are set from the channel names, and the units are assumed to be the same microvolts as the other channels of
  that export. The release Sample/Time columns show that the excerpt starts at sample 4234240 (8270 s) of the clinical
  file (see `RecordingDescription` in the sidecar). The release's per-contact anatomical labels for the depth
  electrodes (`d419f2_Anatomical_Labels.txt`) are kept in `sourcedata/`.
- **sub-08**: channels 49-64 are marked `bad` (release note: "49-64 are empty channels"; 1-32 are a left parietal
  half grid and 33-48 a left interhemispheric strip).
- `analysis_region` in `channels.tsv` marks the 12 channels the paper analysed (two per gyrus: middle frontal,
  precentral, postcentral, supramarginal, superior temporal and middle temporal), from the release montage files.
- Electrode coordinates are not part of the release. `electrodes.tsv` lists the ECoG/sEEG electrode names with `n/a` coordinates (space `Other`).

## What was converted, and how
Each `RestingState_<id>_iEEG.csv` (samples × channels) became one BrainVision run (`sub-XX_task-rest_ieeg.vhdr`,
IEEE float32). The file values are the CSV values rounded to float32, and the round-trip check compares every run
with the CSV. No filtering, resampling, re-referencing or channel removal was applied. The paper's processing
(resampling to 1024 Hz, bipolar re-referencing, drift and line-noise filtering, z-scoring) is **not** applied.

`sourcedata/zenodo-16954184/` holds the original release zip byte for byte (CSV tables, channel tables, montage files,
the sub02 anatomical labels and the sub08 note) and the Zenodo record metadata.

## License

CC-BY-NC-4.0

## Additional metadata and localisation (added 2026-10-08)

Compiled after the upload from the article, its supplement and the source deposit (each statement names its source). Text and sidecar metadata only; no data file was changed.

**Recording system.** Sampled "at a minimum of 512 Hz and maximum of 4800 Hz (depending on acquisition system and site)" (preprint Methods "Signal processing"); per-subject rates in the deposit channels.txt: 1200 Hz (9 subjects), 512, 4800, 1525.88, 1220.70 and 1000 Hz. channels.txt lists low_cutoff 0, high_cutoff 500, notch 58-62, units "unscaled" (deposit, e.g. sub06 channels.txt). Amplifier make: n/a (not stated).

**Reference scheme.** The deposit CSVs hold the recorded channels; the paper analysed bipolar pairs of two adjacent contacts within the same gyrus (preprint Methods). channels.txt status_description reads "keep for referencing". Original recording reference: n/a.

**Electrode types.** Subdural grids (8x8, 4x8) and strips, some patients also with depth electrodes (preprint Table 1). Deposit note sub08_a9952e/note.txt: "1-32 are parietal half grid (left); 33-48 are intrahemispheric strip (left); 49-64 are empty channels".

**Localisation method.** "ECoG contact locations were identified by clinical MRI reconstructions aligned to CT scans. Electrode orientations (and, by extension, channel numbering) were cross-referenced to a combination of intra-operative placement photos, surgical notes, and clinical monitoring notes." "Gyri were identified based on automated registration of Harvard-Oxford atlas labels to patient clinical neuroimaging using the Localizing Electrodes GUI and manual inspection of co-registered CT and T1-weighted MR images." (preprint Methods). The deposit montage.csv per subject gives the gyrus of each analysed contact (empty `ch` = gyrus not analysed for that subject). No coordinates or imaging are deposited. For sub02 the deposit also contains d419f2_Anatomical_Labels.txt (ROSA-planned depth electrodes A'-S', 116 contacts, atlas labels "not independently verified"); those depth channels are not part of the deposited resting-state recording.

These gyrus labels are now in the `anat_label` column of each `*_electrodes.tsv` (n/a for contacts that montage.csv does not list).
