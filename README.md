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
