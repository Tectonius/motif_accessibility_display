# GM12878 TF Accessibility Profiles

Static, dropdown-navigable viewer for aggregate Hi-C-derived accessibility
signal around transcription factor binding sites in GM12878, comparing the
**full mixed-enzyme pool** (`h.bw` — a mix of MNase/DNase-I/benzonase/CviJI/
dsDNA-fragmentase/quadRE Hi-C replicates) against a **DNase-I-only subset**
(65 of 237 replicates confirmed via ENCODE metadata to be DNase-I digested,
merged additively) and an **MNase-only subset** (70 of 237 replicates
confirmed via ENCODE metadata to be micrococcal-nuclease digested, merged
the same way).

Two analyses, each run against all three tracks:

- **Motif accessibility** (`motif/`) — aggregate signal centered on JASPAR
  motif instances for a TF, restricted to motif instances that overlap a
  real ChIP-seq peak for that TF.
- **Nucleosome detection** (`nucleosome/`) — aggregate signal centered on raw
  ChIP-seq peak centers (no motif filtering), used to look for periodic
  nucleosome-phasing ripples flanking the peak. TFs are rankable by an
  autocorrelation-based oscillation score (`data/oscillation_*.tsv`).

Motif accessibility is additionally split by the strand of the JASPAR
motif-scan hit itself (`full_plus`/`full_minus`, `dnase_plus`/`dnase_minus`),
for both the full and DNase-I-only tracks — deepTools orients minus-strand
regions when building the matrix, so a real difference between the two
strand tracks reflects genuine strand-linked asymmetry, not a coordinate
artifact.

Open `index.html` (works from GitHub Pages or any static file server — no
build step) and use the dropdowns to pick analysis type, track, sort order,
and TF.

## Source

Generated from `ref_bias/accessibility_analysis/{motif_accessibility_profiles,
nucleosome_detection}` and their DNase-I-only and MNase-only counterparts
(`dnase_motif_accessibility_profiles`, `dnase_nucleosome_detection`,
`mnase_motif_accessibility_profiles`, `mnase_nucleosome_detection`). See
`project_outlines/chrombpnet_hic_accessibility.md` in the parent research
repo for the full methodology behind the DNase-I-only subset, and
`project_outlines/mnase_nucleosome_detection.md` for the MNase-only subset.
