# OceanOmics Public Datasets on Amazon Web Services

This document describes the format and gives simple examples for getting started with the OceanOmics data stored on AWS.

# Ocean Genomes

The OceanOmics Ocean Genomes data aims to sequence all marine vertebrates. Its main product is single individual-genome sequencing data and genome assemblies.

Data is stored in FASTA, BAM, and FASTQ format in the following buckets:

    s3://minderoo-oceanomics/

To list all species in the S3 bucket, run

    aws s3 ls --no-sign-request s3://minderoo-oceanomics/species/

To download a species' curated genome assembly, run 

    aws s3 cp --no-sign-request s3://minderoo-oceanomics/species/Enoplosus_armatus/fEnoArm2/assembly_curated/ . --recursive

The structure of the data follows the [GenomeArk structure](https://genomeark.s3.amazonaws.com/index.html). 

One folder per species with the accepted scientific name,  
this folder contains samples in the form of Tree of Life IDs (ToLID), 
then several folders containing genome assembly and sequencing data:

| Folder name | Folder contents |
| ------------- | ------------- | 
| assembly_MT/ | Mitogenome assembly based on HiFi reads, contains rotated fasta and genbank with gene predictions based on [EMMA](https://github.com/ian-small/Emma) | 
| assembly_curated/ | 'Final' genome assembly after manual curation using HiC contact matrices. Contains fasta.gz files of pseudo-chromosomes or scaffolds. | 
| assembly_oceangenomes_v1 | contains evaluation data and pre-curation assembly steps | 
| assembly_oceangenomes_v1/evaluation/busco | Contains BUSCO genome completeness results |
| assembly_oceangenomes_v1/evaluation/genomescope | Contains GenomeScope genome size estimate results | 
| assembly_oceangenomes_v1/evaluation/{hap1,hap2} | Contains BUSCO results per haplotype | 
| assembly_oceangenomes_v1/evaluation/merqury | Contains Merqury genome quality evaluation results |
| assembly_oceangenomes_v1/intermediates/ | Contains hifiasm assembled contigs and contig graphs for hap1 and hap2 | 
| genomic_data/ | contains raw reads of all data types |
| genomic_data/illumina | raw Illumina paired ends reads, fastq.gz | 
| gneomic_data/hic | raw HiC data in fastq.gz format |
| genomic_data/pacbio_hifi | raw PacBio HiFi reads in bam and fastq.ggz format |

Example: Assembly fArrgeo1, *Arripis georgianus*

```
Arripis_georgianus/
└── fArrGeo1
    ├── assembly_MT
    │   ├── fArrGeo1.20230529.mitohifi.fasta
    |   └── fArrGeo1.20230529.mitohifi.gb
    ├── assembly_curated
    │   ├── fArrGeo1.hap1.cur.20230529.agp
    │   ├── fArrGeo1.hap1.cur.20230529.fasta.gz
    │   ├── fArrGeo1.hap1.cur.20230529.gfastats.txt
    │   ├── fArrGeo1.hap1.cur.map.pretext
    │   ├── fArrGeo1.hap1.cur.pretext_snapshot.png
    │   ├── fArrGeo1.hap2.cur.20230529.agp
    │   ├── fArrGeo1.hap2.cur.20230529.fasta.gz
    │   ├── fArrGeo1.hap2.cur.20230529.gfastats.txt
    │   ├── fArrGeo1.hap2.cur.map.pretext
    │   └── fArrGeo1.hap2.cur.pretext_snapshot.png
    ├── assembly_oceangenomes_v1
    │   ├── evaluation
    │   │   ├── busco
    │   │   │   ├── fArrGeo1.hap1.cur.busco_full_table.tsv
    │   │   │   ├── fArrGeo1.hap1.cur.busco_short_summary.txt
    │   │   │   ├── fArrGeo1.hap1.cur.missing_busco_list.tsv
    │   │   │   ├── fArrGeo1.hap1.hap2.cur.busco_figure.png
    │   │   │   ├── fArrGeo1.hap2.cur.busco_full_table.tsv
    │   │   │   ├── fArrGeo1.hap2.cur.busco_short_summary.txt
    │   │   │   └── fArrGeo1.hap2.cur.missing_busco_list.tsv
    │   │   ├── genomescope
    │   │   │   ├── fArrGeo1_genomescope_linear_plot.png
    │   │   │   ├── fArrGeo1_genomescope_log_plot.png
    │   │   │   ├── fArrGeo1_genomescope_model.txt
    │   │   │   ├── fArrGeo1_genomescope_progress.txt
    │   │   │   ├── fArrGeo1_genomescope_summary.txt
    │   │   │   ├── fArrGeo1_genomescope_transformed_linear_plot.png
    │   │   │   └── fArrGeo1_genomescope_transformed_log_plot.png
    │   │   ├── hap1
    │   │   │   ├── busco
    │   │   │   │   ├── fArrGeo1.hap1.hap2.s2.busco_figure.png
    │   │   │   │   ├── fArrGeo1.hap1.s2.busco_full_table.tsv
    │   │   │   │   ├── fArrGeo1.hap1.s2.busco_short_summary.txt
    │   │   │   │   └── fArrGeo1.hap1.s2.missing_busco_list.tsv
    │   │   │   ├── gfastats
    │   │   │   │   └── fArrGeo1.hap1.s2.gfastats.txt
    │   │   │   └── pretext
    │   │   │       ├── fArrGeo1.hap1.s2.map.pretext
    │   │   │       └── fArrGeo1.hap1.s2.pretext_snapshot.png
    │   │   ├── hap2
    │   │   │   ├── busco
    │   │   │   │   ├── fArrGeo1.hap2.s2.busco_full_table.tsv
    │   │   │   │   ├── fArrGeo1.hap2.s2.busco_short_summary.txt
    │   │   │   │   └── fArrGeo1.hap2.s2.missing_busco_list.tsv
    │   │   │   ├── gfastats
    │   │   │   │   └── fArrGeo1.hap2.s2.gfastats.txt
    │   │   │   └── pretext
    │   │   │       ├── fArrGeo1.hap2.s2.map.pretext
    │   │   │       └── fArrGeo1.hap2.s2.pretext_snapshot.png
    │   │   └── merqury
    │   │       ├── fArrGeo1_png
    │   │       │   ├── fArrGeo1.hap1.chr_level.spectra-cn.fl.png
    │   │       │   ├── fArrGeo1.hap1.chr_level.spectra-cn.ln.png
    │   │       │   ├── fArrGeo1.hap1.chr_level.spectra-cn.st.png
    │   │       │   ├── fArrGeo1.hap2.chr_level.spectra-cn.fl.png
    │   │       │   ├── fArrGeo1.hap2.chr_level.spectra-cn.ln.png
    │   │       │   ├── fArrGeo1.hap2.chr_level.spectra-cn.st.png
    │   │       │   ├── fArrGeo1.spectra-asm.fl.png
    │   │       │   ├── fArrGeo1.spectra-asm.ln.png
    │   │       │   ├── fArrGeo1.spectra-asm.st.png
    │   │       │   ├── fArrGeo1.spectra-cn.fl.png
    │   │       │   ├── fArrGeo1.spectra-cn.ln.png
    │   │       │   └── fArrGeo1.spectra-cn.st.png
    │   │       ├── fArrGeo1_qv
    │   │       │   ├── fArrGeo1.hap1.chr_level.qv
    │   │       │   ├── fArrGeo1.hap2.chr_level.qv
    │   │       │   └── fArrGeo1.summary.qv
    │   │       └── fArrGeo1_stats
    │   │           └── fArrGeo1.completeness.stats
    │   ├── fArrGeo1.hap1.hap2.s2.fasta.gz
    │   └── intermediates
    │       ├── fArrGeo1_hap1_contigs.fasta.gz
    │       ├── fArrGeo1_hap2_contigs.fasta.gz
    │       ├── hifiasm
    │       │   ├── fArrGeo1_hap1_contig_graph.gfa.gz
    │       │   └── fArrGeo1_hap2_contig_graph.gfa.gz
    │       └── meryl
    │           └── fArrGeo1_v230525_meryldb.tar.gz
    └── genomic_data
        ├── hic
        │   ├── OG95H_D_HICL_S4_L001_R1_001.fastq.gz
        │   ├── OG95H_D_HICL_S4_L001_R2_001.fastq.gz
        │   └── fArrGeo1_hic_files.md5
        └── pacbio_hifi
            ├── OG95H_m64497e_230528_010643.hifi_reads.bc2010--bc2010.bam
            ├── OG95H_m64497e_230528_010643.hifi_reads.bc2010--bc2010.filt.fastq.gz
            ├── OG95H_m64497e_230528_010643.unbarcoded.hifi_reads.bam
            ├── OG95H_m64497e_230528_010643.unbarcoded.hifi_reads.filt.fastq.gz
            ├── OG95H_m64497e_230529_102918.hifi_reads.bc2010--bc2010.bam
            ├── OG95H_m64497e_230529_102918.hifi_reads.bc2010--bc2010.filt.fastq.gz
            ├── OG95H_m64497e_230529_102918.unbarcoded.hifi_reads.bam
            ├── OG95H_m64497e_230529_102918.unbarcoded.hifi_reads.filt.fastq.gz
            └── fArrGeo1_hifi_files.md5

```

# OceanOmics eDNA

The OceanOmics eDNA data aims to learn how to use environmental DNA (eDNA) to assess ecosystem health. Its main products are metabarcoding and metagenomics raw reads, and processed metabarcoding outputs including ASVs and their assigned taxonomy.

The data is structured by OceanOmics project (often but not always a voyage), then by pipeline run, then by assay. The S3 bucket contains one folder per project, with each project folder containing a folder per pipeline run. The pipeline run folder name encodes the [`OceanOmics-amplicon-nf`](https://github.com/Minderoo-OceanOmics-Centre-UWA/OceanOmics-amplicon-nf) pipeline version, the curated reference database version, and the year/month the analysis was run. Each pipeline run is split into several folders, one folder per chosen assay (16SFishD, MiFishUE2, etc.):

```
{project}/{amp-vX.Y.Z_curdb-vA.B.C_YYYYMon}/{assay}/...
```

For example, `OcOm_2513/amp-v1.3.2_curdb-v1.0.1_2026JUN/16SFishD/...`.

Within each assay folder, numbered subfolders represent sequential stages of
the pipeline (demultiplexing/QC → denoising → curation → taxonomy →
filtering/reporting):

```
.
└── OcOm_2513
    └── amp-v1.3.2_curdb-v1.0.1_2026JUN
        ├── 16SFishD
        │   ├── 01-cutadapt
        │   │   ├── all-primers-trimmed
        │   │   │   └── *.R1/R2.fq.gz          # reads after primer trimming
        │   │   ├── assigned
        │   │   │   └── *.R1/R2.fq.gz          # demultiplexed reads, before primer trimming
        │   │   └── unknown
        │   │       └── Unknown
        │   │           └── *.R1/R2.fq.gz      # reads not assignable to a sample/barcode pair
        │   ├── 01-fastqc
        │   │   └── *_fastqc.html / *_fastqc.zip
        │   ├── 01-seqkit_stats
        │   │   └── raw/assigned/unknown/prefilter/final_seqkit_stats.txt
        │   ├── 02-dada2
        │   │   ├── plots/                     # QC plots + read-tracking stats
        │   │   ├── *_asv.fa                   # ASV sequences
        │   │   ├── *_asv_table.csv            # ASV count table
        │   │   ├── *_asv_final_table.tsv      # ASV count table (transposed)
        │   │   ├── *_lca_input.tsv            # ASV count table reformatted for the BLAST/LCA taxonomy step
        │   │   └── *_seq_tab.rds              # DADA2 sequence table (ASV x sample counts), pre-chimera-removal
        │   ├── 03-lulu
        │   │   ├── *_asv_db/                  # BLAST db built from ASVs
        │   │   ├── *_curated_asv.fa           # ASVs after LULU curation
        │   │   ├── *_asv_curated_table.tab    # ASV count table after LULU curation
        │   │   ├── *_asv_lulu_map.tab         # LULU stats: which ASVs were merged/collapsed into which
        │   │   └── *_asv_match_list.txt       # ASV self-vs-self BLAST matchlist used as LULU input
        │   ├── 04-blast
        │   │   └── *_{curateddb,nt}_blastn_results.txt   # BLAST vs curated DB and vs NCBI nt
        │   ├── 04-ocomnbc
        │   │   └── *_{curateddb,nt}[_lulucurated]_ocom_nbc_output.tsv   # Naive Bayes classifier output
        │   ├── 05-lca
        │   │   └── *_{curateddb,nt}[_lulucurated]_{lca_with_fishbase_output,taxa_final,taxa_raw}.tsv
        │   ├── 06-aquamap
        │   │   └── *_aquamaps_{curateddb,nt}[_lulucurated].csv   # species occurrence probability by location
        │   ├── 06-phyloseq
        │   │   └── *_{curateddb,nt}[_lulucurated]_{final_taxa.tsv,flagged_phyloseq.rds}
        │   ├── 07-faire
        │   │   └── *_{curateddb,nt}[_lulucurated]_final_faire_metadata.xlsx   # FAIRe-formatted results + metadata
        │   ├── 07-multiqc
        │   │   └── *_multiqc_report.html      # aggregate pipeline QC report
        │   ├── 07-pipeline_info
        │   │   ├── *_samplesheet.valid.csv
        │   │   ├── execution_report/execution_timeline/execution_trace/pipeline_dag (per run attempt)
        │   │   └── software_versions.yml
        │   └── 07-proportional_filter
        │       └── *_{curateddb,nt}[_lulucurated]_{OTU_filtered,faire_taxa_filtered,phyloseq_filtered,phyloseq_taxa_filtered,proportional_stats}.*
        ├── MarVer1
        │   └── ... (same structure as above)
        └── MiFishUE2
            └── ... (same structure as above)
```

Note: outputs from stage 04 onward are duplicated per taxonomy-assignment
path — `curateddb` (OceanOmics curated reference database) vs `nt` (NCBI
nt), each with and without a `_lulucurated` variant (before/after LULU
curation) — so collaborators can compare results across these approaches.


