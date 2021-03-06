## Output

### Interactive HTML report

An interactive and tier-structured HTML report that shows the most relevant findings in the query cancer genome is provided with the following naming convention:

__sample_id__.pcgr_acmg.__genome_assembly__.html

 - The __sample_id__ is provided as input by the user, and reflects a unique identifier of the tumor-normal sample pair to be analyzed.

The report is structured in seven main sections, described in more detail below:

  1. __Report and query settings__
	 * Lists key configurations provided by user
  2. __Main results__
  	 * Eight value boxes that highlight the main properties of the tumor sample:
	     1. Tumor purity - as provided by the user
		2. Tumor ploidy - as provided by the user
	 	3. Mutational signatures - two most prevalent signatures (other than aging)
		4. Tier 1 variants (top four)
		5. Tier 2 variants (top four)
		6. Tumor mutational burden (TMB)
		7. Microsatellite instability (MSI) status
		8. Somatic copy number aberrations of clinical significance

  2. __Somatic SNVs/InDels__
	 * _Tumor mutational burden (TMB)_
		 * given a coding target region size specified by the user (ideally the __callable target size__), an estimate of the mutational burden is provided
		 - is presently only computed for tumor-normal input (e.g. *vcf_tumor_only = false*)
		 - The estimated mutational burden is assigned a descriptive *tertile* based on thresholds defined by the user (these should reflect thresholds of clinical significance, and may vary for different tumor types)
		 - The estimated TMB is shown in the context of TMB distributions from different primary sites in TCGA
	 * _Tier & variant statistics_
		 - indicate total variant numbers across variant types, coding types and tiers
      * _Global distribution - allelic support_
		 - distribution (histogram) of variant allelic support for somatic variants (will only be present in the report if specific fields in input VCF is defined and specified by the user)
      * _Global variant browser_
		 - permits exploration of the whole SNV/InDel dataset by filtering along several dimensions (call confidence, variant sequencing depth/support, variant consequence etc.)
      * _Tier tables_
	      - Variants are organized into five (tier 1-4 + noncoding) interactive datatables) according to clinical utility
	      - Contents of the tier tables are outlined below

  4. __Somatic CNAs__
      * _Segments - amplifications and homozygous deletions_
         -  Based on user-defined/default log-ratio thresholds of gains/losses, the whole CNA dataset can be navigated further through filters:
	       - cytoband
		  - type of CNA event - *focal* (less than 25% of chromosome arm affected) or *broad*
		  - log ratio
      * _Proto-oncogenes subject to copy number amplifications_
         - Datatable listing known proto-oncogenes covered by user-defined/default amplifications and potential targeted therapies
      * _Tumor suppressor genes subject to homozygous deletions_
         - Datatable listing known tumor suppressor genes covered by user-defined/default losses and potential targeted therapies
      * _Copy number aberrations as biomarkers for prognosis, diagnosis, and drug response_
         - Interactive data table where the user can navigate aberrations acting as biomarkers across therapeutic contexts, tumor subtypes, evidence levels etc.
  5. __MSI status__
      * Indicates predicted microsatellite stability from the somatic mutation profile and supporting evidence (details of the underlying MSI statistical classifier can be found [here](http://rpubs.com/sigven/msi_classification_v3))
      * The MSI classifier was trained on TCGA exome samples.
      * Will only be present in the report if specified by the user in the configuration file (i.e. _msi = true_) and if the input is tumor-normal (i.e. _vcf_tumor_only = false_)
  6. __Mutational signatures__
      * Estimation of relative contribution of [30 known mutational signatures](http://cancer.sanger.ac.uk/cosmic/signatures) in tumor sample (using [deconstructSigs](https://github.com/raerose01/deconstructSigs) as the underlying framework)
      * Datatable with signatures and proposed underlying etiologies
      * Will only be present in the report if specified by the user in the configuration file (i.e. _mutsignatures = true_) and if the input is tumor-normal (i.e. _vcf_tumor_only = false_)
      * [Trimer (i.e. DNA 3-mer) normalization](https://github.com/raerose01/deconstructSigs) can be configured according to sequencing approach used (WES, WXS etc.) using the 'mutsignatures_normalization' option, as can the
		 * minimum number of mutations required for analysis (option 'mutsignatures_mutation_limit')
		 * the maximum number of mutational signatures in the search space (option 'mutsignatures_signature_limit')
		 * possibility to discard any signature contributions with a weight less than a given cutoff (option 'mutsignatures_cutoff')
  7. __Documentation__
      * Annotation resources - software, databases and tools with version information
      * References - supporting scientific literature (key report elements)

#### Interactive datatables

The interactive datatables contain a number of hyperlinked annotations similar to those defined for the annotated VCF file, including the following:

* SYMBOL - Gene symbol (Entrez/NCBI)
* PROTEIN_CHANGE - Amino acid change (VEP)
* CANCER_TYPE - Biomarker (tier 1/2): associated cancer type
* EVIDENCE_LEVEL - Biomarker (tier 1/2): evidence level (A,B,C,D,E)
* CLINICAL_SIGNIFICANCE - Biomarker (tier 1/2): drug sensitivity, poor/better outcome etc
* EVIDENCE_TYPE - Biomarker (tier 1/2): predictive/diagnostic/therapeutic
* DISEASE_ONTOLOGY_ID - Biomarker (tier 1/2): associated cancer type (Disease Ontology)
* EVIDENCE_DIRECTION - Biomarker (tier 1/2): supports/does not support
* DESCRIPTION - Biomarker (tier 1/2): description
* VARIANT_ORIGIN - Biomarker (tier 1/2): variant origin (germline/somatic)
* BIOMARKER_MAPPING - Biomarker (tier 1/2): accuracy of genomic mapping (exact,codon,exon)
* CITATION - Biomarker (tier 1/2): supporting literature
* THERAPEUTIC_CONTEXT - Biomarker (tier 1/2): associated drugs
* RATING - Biomarker (tier 1/2): trust rating from 1 to 5 (CIVIC)
* GENE_NAME - gene name description (Entrez/NCBI)
* PROTEIN_DOMAIN - PFAM protein domain
* PROTEIN_FEATURE - UniProt feature overlapping variant site
* CDS_CHANGE - Coding sequence change
* MUTATION_HOTSPOT - Known cancer mutation hotspot
* MUTATION_HOTSPOT_CANCERTYPE - Hotspot-associated cancer types
* TCGA_FREQUENCY - Frequency of variant in TCGA cohorts
* ICGC_PCAWG_OCCURRENCE - Frequency of variant in ICGC-PCAWG cohorts
* DOCM_LITERATURE - Literature links - DoCM
* DOCM_DISEASE - Associated diseases - DoCM
* OPENTARGETS_RANK - Strength of gene-phenotype associatino according to the Open Targets Platform
* OPENTARGETS_ASSOCIATIONS - Phenotype associations with the gene retrieved from the Open Targets Platform
* INTOGEN_DRIVER_MUT - predicted driver mutation - IntOGen
* CONSEQUENCE - VEP consequence (primary transcript)
* HGVSc - from VEP
* HGVSp - from VEP
* NCBI_REFSEQ - Transcript accession ID(s) (NCBI RefSeq)
* ONCOGENE - Predicted as proto-oncogene from literature mining
* TUMOR_SUPPRESSOR - Predicted as tumor suppressor gene from literature mining
* ONCOSCORE - Literature-derived score for oncogenic potential (gene level)
* PREDICTED_EFFECT - Effect predictions from dbNSFP
* VEP_ALL_CSQ - All VEP transcript block consequences
* DBSNP - dbSNP rsID
* COSMIC - Cosmic mutation IDs
* CLINVAR - ClinVar variant origin and associated phenotypes
* CANCER_ASSOCIATIONS - Gene-associated cancer types from DisGenet
* TARGETED_DRUGS - Targeted drugs from Open Targets Platform /ChEMBL
* KEGG_PATHWAY - Gene-associated pathways from KEGG
* CALL_CONFIDENCE - Variant confidence (as set by user in input VCF)
* DP_TUMOR - Variant sequencing depth in tumor (as set by user in input VCF)
* AF_TUMOR - Variant allelic fraction in tumor (as set by user in input VCF)
* DP_CONTROL - Variant sequencing depth in control sample (as set by user in input VCF)
* AF_CONTROL - Variant allelic fraction in control sample (as set by user in input VCF)
* GENOMIC_CHANGE - Variant ID
* GENOME_VERSION - Genome assembly


Example reports:

* [View an example report for a breast tumor sample (TCGA)](http://folk.uio.no/sigven/tumor_sample.BRCA.pcgr_acmg.grch37.0.8.2.html)
* [View an example report for a colon adenocarcinoma sample (TCGA)](http://folk.uio.no/sigven/tumor_sample.COAD.pcgr_acmg.grch37.0.8.2.html)

The HTML reports have been tested using the following browsers:

* Safari (Version 12.1 (14607.1.40.1.4))
* Mozilla Firefox (52.0.2)
* Google Chrome (Version 74.0.3729.131 )

### JSON (beta)

A JSON file that stores the HTML report content is provided. This file will easen the process of extracting particular parts of the report for further analysis. The JSON contains two main objects, *metadata* and *content*, where the former contains information about the settings, data versions, and the latter contains the various sections of the report.

### Output files - somatic SNVs/InDels

#### Variant call format - VCF

A VCF file containing annotated, somatic calls (single nucleotide variants and insertion/deletions) is generated with the following naming convention:

__sample_id__.pcgr_acmg.__genome_assembly__.vcf.gz

Here, the __sample_id__ is provided as input by the user, and reflects a unique identifier of the tumor-normal sample pair to be analyzed. Following common standards, the annotated VCF file is compressed with [bgzip](http://www.htslib.org/doc/tabix.html) and indexed with [tabix](http://www.htslib.org/doc/tabix.html). Below follows a description of all annotations/tags present in the VCF INFO column after processing with the PCGR annotation pipeline:

##### _VEP consequence annotations_
  - CSQ - Complete consequence annotations from VEP. Format: Allele|Consequence|IMPACT|SYMBOL|Gene|Feature_type|Feature|BIOTYPE|EXON|
  INTRON|HGVSc|HGVSp|cDNA_position|CDS_position|Protein_position|Amino_acids|
  Codons|Existing_variation|ALLELE_NUM|DISTANCE|STRAND|FLAGS|PICK|VARIANT_CLASS|
  SYMBOL_SOURCE|HGNC_ID|CANONICAL|APPRIS|CCDS|ENSP|SWISSPROT|TREMBL|UNIPARC|
  RefSeq|DOMAINS|HGVS_OFFSET|AF|AFR_AF|AMR_AF|EAS_AF|EUR_AF|SAS_AF|gnomAD_AF|
  gnomAD_AFR_AF|gnomAD_AMR_AF|gnomAD_ASJ_AF|gnomAD_EAS_AF|gnomAD_FIN_AF|
  gnomAD_NFE_AF|gnomAD_OTH_AF|gnomAD_SAS_AF|CLIN_SIG|SOMATIC|PHENO|
  MOTIF_NAME|MOTIF_POS|HIGH_INF_POS|MOTIF_SCORE_CHANGE
  - Consequence - Impact modifier for the consequence type (picked by VEP's --flag\_pick\_allele option)
  - Gene - Ensembl stable ID of affected gene (picked by VEP's --flag\_pick\_allele option)
  - Feature_type - Type of feature. Currently one of Transcript, RegulatoryFeature, MotifFeature (picked by VEP's --flag\_pick\_allele option)
  - Feature - Ensembl stable ID of feature (picked by VEP's --flag\_pick\_allele option)
  - cDNA_position - Relative position of base pair in cDNA sequence (picked by VEP's --flag\_pick\_allele option)
  - CDS_position - Relative position of base pair in coding sequence (picked by VEP's --flag\_pick\_allele option)
  - CDS\_CHANGE - Coding, transcript-specific sequence annotation (picked by VEP's --flag\_pick\_allele option)
  - AMINO_ACID_START - Protein position indicating absolute start of amino acid altered (fetched from Protein_position)
  - AMINO_ACID_END -  Protein position indicating absolute end of amino acid altered (fetched from Protein_position)
  - Protein_position - Relative position of amino acid in protein (picked by VEP's --flag\_pick\_allele option)
  - Amino_acids - Only given if the variant affects the protein-coding sequence (picked by VEP's --flag\_pick\_allele option)
  - Codons - The alternative codons with the variant base in upper case (picked by VEP's --flag\_pick\_allele option)
  - IMPACT - Impact modifier for the consequence type (picked by VEP's --flag\_pick\_allele option)
  - VARIANT_CLASS - Sequence Ontology variant class (picked by VEP's --flag\_pick\_allele option)
  - SYMBOL - Gene symbol (picked by VEP's --flag\_pick\_allele option)
  - SYMBOL_ENTREZ - Official gene symbol as provided by NCBI's Entrez gene
  - SYMBOL_SOURCE - The source of the gene symbol (picked by VEP's --flag\_pick\_allele option)
  - STRAND - The DNA strand (1 or -1) on which the transcript/feature lies (picked by VEP's --flag\_pick\_allele option)
  - ENSP - The Ensembl protein identifier of the affected transcript (picked by VEP's --flag\_pick\_allele option)
  - FLAGS - Transcript quality flags: cds\_start\_NF: CDS 5', incomplete cds\_end\_NF: CDS 3' incomplete (picked by VEP's --flag\_pick\_allele option)
  - SWISSPROT - Best match UniProtKB/Swiss-Prot accession of protein product (picked by VEP's --flag\_pick\_allele option)
  - TREMBL - Best match UniProtKB/TrEMBL accession of protein product (picked by VEP's --flag\_pick\_allele option)
  - UNIPARC - Best match UniParc accession of protein product (picked by VEP's --flag\_pick\_allele option)
  - HGVSc - The HGVS coding sequence name (picked by VEP's --flag\_pick\_allele option)
  - HGVSp - The HGVS protein sequence name (picked by VEP's --flag\_pick\_allele option)
  - HGVSp_short - The HGVS protein sequence name, short version (picked by VEP's --flag\_pick\_allele option)
  - HGVS_OFFSET - Indicates by how many bases the HGVS notations for this variant have been shifted (picked by VEP's --flag\_pick\_allele option)
  - MOTIF_NAME - The source and identifier of a transcription factor binding profile aligned at this position (picked by VEP's --flag\_pick\_allele option)
  - MOTIF_POS - The relative position of the variation in the aligned TFBP (picked by VEP's --flag\_pick\_allele option)
  - HIGH\_INF\_POS - A flag indicating if the variant falls in a high information position of a transcription factor binding profile (TFBP) (picked by VEP's --flag\_pick\_allele option)
  - MOTIF\_SCORE\_CHANGE - The difference in motif score of the reference and variant sequences for the TFBP (picked by VEP's --flag\_pick\_allele option)
  - CELL_TYPE - List of cell types and classifications for regulatory feature (picked by VEP's --flag\_pick\_allele option)
  - CANONICAL - A flag indicating if the transcript is denoted as the canonical transcript for this gene (picked by VEP's --flag\_pick\_allele option)
  - CCDS - The CCDS identifier for this transcript, where applicable (picked by VEP's --flag\_pick\_allele option)
  - INTRON - The intron number (out of total number) (picked by VEP's --flag\_pick\_allele option)
  - EXON - The exon number (out of total number) (picked by VEP's --flag\_pick\_allele option)
  - LAST_EXON - Logical indicator for last exon of transcript (picked by VEP's --flag\_pick\_allele option)
  - LAST_INTRON - Logical indicator for last intron of transcript (picked by VEP's --flag\_pick\_allele option)
  - DISTANCE - Shortest distance from variant to transcript (picked by VEP's --flag\_pick\_allele option)
  - BIOTYPE - Biotype of transcript or regulatory feature (picked by VEP's --flag\_pick\_allele option)
  - TSL - Transcript support level (picked by VEP's --flag\_pick\_allele option)>
  - PUBMED - PubMed ID(s) of publications that cite existing variant - VEP
  - PHENO - Indicates if existing variant is associated with a phenotype, disease or trait - VEP
  - GENE_PHENO - Indicates if overlapped gene is associated with a phenotype, disease or trait - VEP
  - ALLELE_NUM - Allele number from input; 0 is reference, 1 is first alternate etc - VEP
  - REFSEQ_MATCH - The RefSeq transcript match status; contains a number of flags indicating whether this RefSeq transcript matches the underlying reference sequence and/or an Ensembl transcript (picked by VEP's --flag\_pick\_allele option)
  - PICK - Indicates if this block of consequence data was picked by VEP's --flag\_pick\_allele option
  - VEP\_ALL\_CONSEQUENCE - All transcript consequences (Consequence:SYMBOL:Feature_type:Feature:BIOTYPE) - VEP
  - EXONIC_STATUS - Indicates if variant consequence type is 'exonic' or 'nonexonic'. We here define 'exonic' as any variant with either of the following consequence:
	  - stop_gained / stop_lost
	  - start_lost
	  - frameshift_variant
	  - missense_variant
	  - splice_donor_variant
	  - splice_acceptor_variant
	  - inframe_insertion / inframe_deletion
	  - synonymous_variant
	  - protein_altering
  - CODING_STATUS - Indicates if primary variant consequence type is 'coding' or 'noncoding' (wrt. protein-alteration). 'coding' variants are here defined as those with an 'exonic' status, with the exception of synonymous variants

##### _Gene information_
  - ENTREZ_ID - [Entrez](http://www.ncbi.nlm.nih.gov/gene) gene identifier
  - APPRIS - Principal isoform flags according to the [APPRIS principal isoform database](http://appris.bioinfo.cnio.es/#/downloads)
  - UNIPROT_ID - [UniProt](http://www.uniprot.org) identifier
  - UNIPROT_ACC - [UniProt](http://www.uniprot.org) accession(s)
  - ENSEMBL_GENE_ID - Ensembl gene identifier for VEP's picked transcript (*ENSGXXXXXXX*)
  - ENSEMBL_TRANSCRIPT_ID - Ensembl transcript identifier for VEP's picked transcript (*ENSTXXXXXX*)
  - REFSEQ_MRNA - Corresponding RefSeq transcript(s) identifier for VEP's picked transcript (*NM_XXXXX*)
  - CORUM_ID - Associated protein complexes (identifiers) from [CORUM](http://mips.helmholtz-muenchen.de/corum/)
  - DISGENET_CUI - Tumor types associated with gene, as found in DisGeNET. Tumor types are listed as unique [MedGen](https://www.ncbi.nlm.nih.gov/medgen/) concept IDs (_CUIs_)
  - TUMOR_SUPPRESSOR - Gene is predicted as tumor suppressor candidate according to ([CancerMine](https://zenodo.org/record/2662509#.XNM4VtMzaL5))
  - ONCOGENE - Gene is predicted as an oncogene according to ([CancerMine](https://zenodo.org/record/2662509#.XNM4VtMzaL5))
  - ONCOSCORE - Literature-derived score for cancer gene relevance [Bioconductor/OncoScore](http://bioconductor.org/packages/release/bioc/html/OncoScore.html), range from 0 (low oncogenic potential) to 1 (high oncogenic potential)
  - INTOGEN_DRIVER - Gene is predicted as a cancer driver in the [IntoGen Cancer Drivers Database](https://www.intogen.org/downloads)
  - TCGA_DRIVER - Gene is predicted as a cancer driver in the [TCGA pan-cancer analysis of cancer driver genes and mutations](https://www.ncbi.nlm.nih.gov/pubmed/29625053)

##### _Variant effect and protein-coding information_
  - MUTATION\_HOTSPOT - mutation hotspot codon in [cancerhotspots.org](http://cancerhotspots.org/). Format: gene_symbol | codon | q-value
  - MUTATION_HOTSPOT_TRANSCRIPT - hotspot-associated transcripts (Ensembl transcript ID)
  - MUTATION_HOTSPOT_CANCERTYPE - hotspot-associated cancer types (from cancerhotspots.org)
  - UNIPROT\_FEATURE - Overlapping protein annotations from [UniProt KB](http://www.uniprot.org)
  - PFAM_DOMAIN - Pfam domain identifier (from VEP)
  - INTOGEN\_DRIVER\_MUT - Indicates if existing variant is predicted as driver mutation from IntoGen Catalog of Driver Mutations
  - PUTATIVE_DRIVER_MUTATION - Variant is predicted as driver mutation in the [TCGA pan-cancer analysis of cancer driver genes and mutations](https://www.ncbi.nlm.nih.gov/pubmed/29625053)
  - EFFECT\_PREDICTIONS - All predictions of effect of variant on protein function and pre-mRNA splicing from [database of non-synonymous functional predictions - dbNSFP](https://sites.google.com/site/jpopgen/dbNSFP). Predicted effects are provided by different sources/algorithms (separated by '&'):

    1. [SIFT](https://sift.bii.a-star.edu.sg/)
    2. [SIFT4G](https://sift.bii.a-star.edu.sg/sift4g/)
    3. [LRT](http://www.genetics.wustl.edu/jflab/lrt_query.html) (2009)
    4. [MutationTaster](http://www.mutationtaster.org/) (data release Nov 2015)
    5. [MutationAssessor](http://mutationassessor.org/) (release 3)
    6. [FATHMM](http://fathmm.biocompute.org.uk) (v2.3)
    7. [PROVEAN](http://provean.jcvi.org/index.php) (v1.1 Jan 2015)
    8. [FATHMM_MKL](http://fathmm.biocompute.org.uk/fathmmMKL.htm)
    9. [PRIMATEAI](https://www.nature.com/articles/s41588-018-0167-z)
    10. [DEOGEN2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5570203/)
    11. [DBNSFP\_CONSENSUS\_SVM](https://www.ncbi.nlm.nih.gov/pubmed/25552646) (Ensembl/consensus prediction, based on support vector machines)
    12. [DBNSFP\_CONSENSUS\_LR](https://www.ncbi.nlm.nih.gov/pubmed/25552646) (Ensembl/consensus prediction, logistic regression based)
    13. [SPLICE\_SITE\_EFFECT_ADA](http://nar.oxfordjournals.org/content/42/22/13534) (Ensembl/consensus prediction of splice-altering SNVs, based on adaptive boosting)
    14. [SPLICE\_SITE\_EFFECT_RF](http://nar.oxfordjournals.org/content/42/22/13534) (Ensembl/consensus prediction of splice-altering SNVs, based on random forest)
    15. [M-CAP](http://bejerano.stanford.edu/MCAP)
    16. [MutPred](http://mutpred.mutdb.org)
    17. [GERP](http://mendel.stanford.edu/SidowLab/downloads/gerp/)

  - SIFT_DBNSFP - predicted effect from SIFT (dbNSFP)
  - SIFT4G_DBNSFP - predicted effect from SIFT4G (dbNSFP)
  - PROVEAN_DBNSFP - predicted effect from PROVEAN (dbNSFP)
  - MUTATIONTASTER_DBNSFP - predicted effect from MUTATIONTASTER (dbNSFP)
  - MUTATIONASSESSOR_DBNSFP - predicted effect from MUTATIONASSESSOR (dbNSFP)
  - M_CAP_DBNSFP - predicted effect from M-CAP (dbNSFP)
  - MUTPRED_DBNSFP - score from MUTPRED (dbNSFP)
  - FATHMM_DBNSFP - predicted effect from FATHMM (dbNSFP)
  - PRIMATEAI_DBNSFP - predicted effect from PRIMATEAI (dbNSFP)
  - DEOGEN2_DBNSFP - predicted effect from DEOGEN2 (dbNSFP)
  - FATHMM_MKL_DBNSFP - predicted effect from FATHMM-mkl (dbNSFP)
  - META_LR_DBNSFP - predicted effect from ensemble prediction (logistic regression - dbNSFP)
  - SPLICE_SITE_RF_DBNSFP - predicted effect of splice site disruption, using random forest (dbscSNV)
  - SPLICE_SITE_ADA_DBNSFP - predicted effect of splice site disruption, using boosting (dbscSNV)


##### _Variant frequencies/annotations in germline/somatic databases_
  - AFR\_AF\_GNOMAD - African/American germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - AMR\_AF\_GNOMAD - American germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - GLOBAL\_AF\_GNOMAD - Adjusted global germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - SAS\_AF\_GNOMAD - South Asian germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - EAS\_AF\_GNOMAD - East Asian germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - FIN\_AF\_GNOMAD - Finnish germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - NFE\_AF\_GNOMAD - Non-Finnish European germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - OTH\_AF\_GNOMAD - Other germline allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - ASJ\_AF\_GNOMAD - Ashkenazi Jewish allele frequency ([Genome Aggregation Database release 2](http://gnomad.broadinstitute.org/))
  - AFR\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for samples from AFR (African)
  - AMR\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for samples from AMR (Ad Mixed American)
  - EAS\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for samples from EAS (East Asian)
  - EUR\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for samples from EUR (European)
  - SAS\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for samples from SAS (South Asian)
  - GLOBAL\_AF\_1KG - [1000G Project - phase 3](http://www.1000genomes.org) germline allele frequency for all 1000G project samples (global)
  - DBSNPRSID - [dbSNP](http://www.ncbi.nlm.nih.gov/SNP/) reference ID, as provided by VEP
  - COSMIC\_MUTATION\_ID - Mutation identifier in [Catalog of somatic mutations in cancer](http://cancer.sanger.ac.uk/cancergenome/projects/cosmic/) database, as provided by VEP
  - TCGA_PANCANCER_COUNT - Raw variant count across all TCGA tumor types
  - TCGA_FREQUENCY - Frequency of variant across TCGA tumor types. Format: tumortype|
  percent affected|affected cases|total cases
  - ICGC_PCAWG_OCCURRENCE - Mutation occurrence in [ICGC-PCAWG](http://docs.icgc.org/pcawg/).
  By project: project_code|affected_donors|tested_donors|frequency)
  - ICGC_PCAWG_AFFECTED_DONORS - Number of donors with the current mutation in [ICGC-PCAWG](http://docs.icgc.org/pcawg/)

##### _Clinical associations_
  - CLINVAR_MSID - [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) Measure Set/Variant ID
  - CLINVAR_ALLELE_ID - [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) allele ID
  - CLINVAR_PMID - Associated Pubmed IDs for variant in [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) - germline state-of-origin
  - CLINVAR_HGVSP - Protein variant expression using HGVS nomenclature
  - CLINVAR_PMID_SOMATIC - Associated Pubmed IDs for variant in [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) - somatic state-of-origin
  - CLINVAR_CLNSIG - Clinical significance for variant in [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) - germline state-of-origin
  - CLINVAR_CLNSIG_SOMATIC - Clinical significance for variant in [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) - somatic state-of-origin
  - CLINVAR_MEDGEN_CUI - Associated [MedGen](https://www.ncbi.nlm.nih.gov/medgen/)  concept identifiers (_CUIs_) - germline state-of-origin
  - CLINVAR_MEDGEN_CUI_SOMATIC - Associated [MedGen](https://www.ncbi.nlm.nih.gov/medgen/)  concept identifiers (_CUIs_) - somatic state-of-origin
  - CLINVAR\_VARIANT\_ORIGIN - Origin of variant (somatic, germline, de novo etc.) for variant in [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar)
  - CLINVAR_REVIEW_STATUS_STARS - Rating of the [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar) variant (0-4 stars) with respect to level of review
  - DOCM_PMID - Associated Pubmed IDs for variant in [Database of Curated Mutations](http://docm.genome.wustl.edu)
  - OPENTARGETS_DISEASE_ASSOCS - Associations between protein targets and disease based on multiple lines of evidence (mutations,affected pathways,GWAS, literature etc). Format: CUI:EFO_ID:IS_DIRECT:OVERALL_SCORE
  - OPENTARGETS_TRACTABILITY_COMPOUND - Confidence for the existence of a modulator (small molecule) that interacts with the target to elicit a desired biological effect
  - OPENTARGETS_TRACTABILITY_ANTIBODY - Confidence for the existence of a modulator (antibody) that interacts with the target to elicit a desired biological effect

##### _Other_
  - CHEMBL_COMPOUND_ID - antineoplastic drugs targeting the encoded protein (from [Open Targets Platform](https://www.targetvalidation.org/), drugs are listed as [ChEMBL](https://www.ebi.ac.uk/chembl/) compound identifiers)
  - CIVIC\_ID, CIVIC\_ID_2 - Variant identifiers in the [CIViC database](http://civic.genome.wustl.edu), CIVIC_ID refers to markers mapped at variant level, CIVIC_ID_2 refers to region markers (codon, exon etc.)
  - CBMDB_ID - Variant identifier in the [Cancer Biomarkers database](https://www.cancergenomeinterpreter.org/biomarkers)

#### Tab-separated values (TSV)

##### Annotated List of all SNVs/InDels
We provide a tab-separated values file with most important annotations for SNVs/InDels. The file has the following naming convention:

__sample_id__.pcgr_acmg.__genome_assembly__.snvs\_indels.tiers.tsv

The SNVs/InDels are organized into different __tiers__ (as defined above for the HTML report)

The following variables are included in the tiered TSV file:

    1. CHROM - Chromosome
    2. POS - Position (VCF-based)
    3. REF - Reference allele
    4. ALT - Alternate allele
    5. GENOMIC_CHANGE - Identifier for variant at the genome (VCF) level, e.g. 1:g.152382569A>G
    	  Format: (<chrom>:g.<position><ref_allele>><alt_allele>)
    6. GENOME_VERSION - Assembly version, e.g. GRCh37
    7. VCF_SAMPLE_ID - Sample identifier
    8. VARIANT_CLASS - Variant type, e.g. SNV/insertion/deletion
    9. SYMBOL - Gene symbol
    10. GENE_NAME - Gene description
    11. CCDS - CCDS identifier
    12. CANONICAL - indication of canonical transcript
    13. ENTREZ_ID - Entrez gene identifier
    14. UNIPROT_ID - UniProt protein identifier
    15. ENSEMBL_TRANSCRIPT_ID - Ensembl transcript identifier
    16. ENSEMBL_GENE_ID - Ensembl gene identifier
    17. REFSEQ_MRNA - RefSeq mRNA identifier
    18. ONCOSCORE - Literature-derived score for cancer gene relevance
    19. ONCOGENE - Gene is predicted as an oncogene according to literature mining (CancerMine)
    20. TUMOR_SUPPRESSOR - Gene is predicted as tumor suppressor according to literature mining (CancerMine)
    21. DISGENET_CUI - Associated tumor types from DisGeNET (MedGen concept IDs)
    22. DISGENET_TERMS - Associated tumor types from DisGeNET (MedGen concept terms)
    23. CONSEQUENCE - Variant consequence (as defined above for VCF output:
        Consequence)
    24. PROTEIN_CHANGE - Protein change (HGVSp without reference accession)
    25. PROTEIN_DOMAIN - Protein domain
    26. CODING_STATUS - Coding variant status wrt. protein alteration ('coding' or 'noncoding')
    27. EXONIC_STATUS - Exonic variant status ('exonic' or 'nonexonic')
    28. CDS_CHANGE - composite VEP-based variable for coding change, format:
        Consequence:Feature:cDNA_position:EXON:HGVSp_short
    29. HGVSp
    30. HGVSc
    31. EFFECT_PREDICTIONS - as defined above for VCF
    32. MUTATION_HOTSPOT - mutation hotspot codon in
        cancerhotspots.org. Format: gene_symbol | codon | q-value
    33. MUTATION_HOTSPOT_TRANSCRIPT - hotspot-associated transcripts (Ensembl transcript ID)
    34. MUTATION_HOTSPOT_CANCERTYPE - hotspot-associated cancer types (from cancerhotspots.org)
    35. PUTATIVE_DRIVER_MUTATION - Indicates if variant is predicted as
        driver mutation from TCGA's PanCancer study of cancer driver mutation
    36. CHASMPLUS_DRIVER - Driver mutation predicted by CHASMplus algorithm
    37. CHASMPLUS_TTYPE - Tumor type for which mutation is predicted as driver by CHASMplus
    38. VEP_ALL_CSQ - all VEP transcript block consequences
    39. DBSNPRSID - dbSNP reference cluster ID
    40. COSMIC_MUTATION_ID - COSMIC mutation ID
    41. TCGA_PANCANCER_COUNT - Raw variant count across all TCGA tumor types
    42. TCGA_FREQUENCY - Frequency of variant across TCGA tumor types. Format: tumortype|
    percent affected|affected cases|total cases
    43. ICGC_PCAWG_OCCURRENCE - Mutation occurrence in ICGC-PCAWG by project:
    project_code|affected_donors|tested_donors|frequency
    44. CHEMBL_COMPOUND_ID - Compounds (as ChEMBL IDs) that target the encoded protein (from Open Targets Platform)
    45. CHEMBL_COMPOUND_TERMS - Compounds (as drug names) that target the encoded protein (from Open Targets Platform)
    46. SIMPLEREPEATS_HIT - Variant overlaps UCSC _simpleRepeat_ sequence repeat track
    47. WINMASKER_HIT - Variant overlaps UCSC _windowmaskerSdust_ sequence repeat track
    48. OPENTARGETS_RANK - OpenTargets association score (between 0 and 1) for gene (maximum across cancer phenotypes)
    49. CLINVAR - ClinVar association: variant origin and associated traits
    50. CLINVAR_CLNSIG - clinical significance of ClinVar variant
    51. GLOBAL_AF_GNOMAD - global germline allele frequency in gnomAD
    52. GLOBAL_AF_1KG - 1000G Project - phase 3, germline allele frequency
    53. CALL_CONFIDENCE - confidence indicator for somatic variant
    54. DP_TUMOR - sequencing depth at variant site (tumor sample)
    55. AF_TUMOR - allelic fraction of alternate allele (tumor sample)
    56. DP_CONTROL - sequencing depth at variant site (control sample)
    57. AF_CONTROL - allelic fraction of alternate allele (control sample)
    58. TIER
    59. TIER_DESCRIPTION


**NOTE**: The user has the possibility to append the TSV file with data from other tags in the input VCF of interest (i.e. using the *custom_tags* option in the TOML configuration file)

### Output files - somatic copy number aberrations

#### 1. Tab-separated values (TSV)

 Copy number segments are intersected with the genomic coordinates of all transcripts from [GENCODE's basic gene annotation](https://www.gencodegenes.org/releases/current.html). In addition, PCGR attaches cancer-relevant annotations for the affected transcripts. The naming convention of the compressed TSV file is as follows:

__sample_id__.pcgr_acmg.__genome_assembly__.cna_segments.tsv.gz

The format of the compressed TSV file is the following:

    1. chrom - chromosome
    2. segment_start - start of copy number segment
    3. segment_end - end of copy number segment
    4. segment_length_Mb - length of segment in Mb
    5. event_type - focal or broad (covering more than 25% of chromosome arm)
    6. cytoband
    7. LogR - Copy log-ratio
    8. sample_id - Sample identifier
    9. ensembl_gene_id
    10. symbol - gene symbol
    11. ensembl_transcript_id
    12. transcript_start
    13. transcript_end
    14. transcript_overlap_percent - percent of transcript length covered by CN segment
    15. name - gene name description
    16. biotype - type of gene
    17. disgenet_cui - tumor types associated with gene (from DisGeNET, tumor types
	   are listed as MedGen concept IDs (CUI)
    18. tsgene - tumor suppressor gene status (CancerMine literature database)
    19. p_oncogene - oncogene status (CancerMine literature database)
    20. intogen_drivers - predicted driver gene status (IntoGen Cancer Drivers Database)
    21. chembl_compound_id - antineoplastic drugs targeting the encoded protein
       (from Open Targets Platform, drugs are listed as ChEMBL compound identifiers)
    22. gencode_gene_biotype
    23. gencode_tag
    24. gencode_release
