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

## CTCF-excluded nucleosome detection (non-CTCF TFs)

Two additional analyses address a confound in the base Nucleosome detection
analysis above: CTCF is itself the strongest known positioner of phased
nucleosomes in the human genome, so any phasing seen at another TF's peaks
could really be coming from a nearby CTCF site rather than the TF itself.

- **Nucleosome detection, CTCF-excluded** (`nucleosome_no_ctcf/`) — same
  peak-center-referenced method, but on a fresh, larger peak set (155
  non-CTCF, non-histone-mark targets from
  `ENCODE_intact_Hi-C_shared_data/ChIP-Seq/GM12878`) with every peak that has
  a CTCF peak center within its own ±2000bp aggregation window removed
  first. Two tracks: `full` (h.bw) and `mnase` (the Hi-C MNase-cutter
  subset) — both Hi-C-derived, so agreement between them mainly reflects
  shared underlying reads.
- **Nucleosome detection, CTCF-excluded (real MNase-seq)**
  (`nucleosome_mnase_seq/`) — the same CTCF-excluded peak set, aggregated
  against a genuinely independent signal: ENCODE `ENCSR000CXP`, a real
  standalone GM12878 MNase-seq experiment (Snyder lab, 2011, hg19→hg38
  liftOver), for orthogonal replication.

See `project_outlines/ctcf_excluded_nucleosome_detection.md` in the parent
research repo for full methodology, data sources, and caveats.

## Source

Generated from `ref_bias/accessibility_analysis/{motif_accessibility_profiles,
nucleosome_detection}` and their DNase-I-only and MNase-only counterparts
(`dnase_motif_accessibility_profiles`, `dnase_nucleosome_detection`,
`mnase_motif_accessibility_profiles`, `mnase_nucleosome_detection`). See
`project_outlines/chrombpnet_hic_accessibility.md` in the parent research
repo for the full methodology behind the DNase-I-only subset, and
`project_outlines/mnase_nucleosome_detection.md` for the MNase-only subset.
