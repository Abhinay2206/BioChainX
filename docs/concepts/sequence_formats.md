**Sequence formats**  
---

You're spot on\! As we generate more and more biological data, especially with high-throughput technologies, having standardized formats to store and share this information becomes crucial. Let's explore the common biological data formats you mentioned and touch upon omics data in general:

### **Fundamental Sequence Formats: FASTA and FASTQ**

These are the workhorses for representing biological sequences.

* **FASTA (.fasta, .fa, .fna, .faa, etc.):** This is a simple text-based format for representing nucleotide or amino acid sequences.

  * Each sequence starts with a header line beginning with a "\>" symbol, followed by a description or identifier of the sequence.  
  * Subsequent lines contain the actual sequence, using single-letter codes for nucleotides (A, T, C, G, U) or amino acids.  
  * FASTA files can contain one or more sequences.  
  * It's primarily used for representing reference sequences, assembled genomes, gene sequences, and protein sequences.  
* Code snippet  
  \>gi|2765658|emb|Z78533.1|PF01551 Arabidopsis thaliana gene for photosystem I light harvesting complex protein (LHC I), mRNA  
  ATGCTTCGCAGTAGTTCGTCATCATCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCGTCAT  
*   
* **FASTQ (.fastq):** This format extends FASTA to also store quality information for each base in a sequencing read. This is essential for working with raw sequencing data from technologies like NGS.

  * Each read is represented by four lines:  
    1. A header line that starts with "@" and contains information about the read (e.g., sequencing instrument, flowcell coordinates).  
    2. The raw sequence of the read.  
    3. A separator line, which is usually just a "+" symbol, but can sometimes repeat the read identifier.  
    4. A quality string, where each character corresponds to a base in the sequence and represents its quality score (the probability of that base being called incorrectly). Different encoding schemes (like Sanger, Solexa, Illumina) exist for these quality scores.  
* Code snippet  
  @SEQ\_ID  
  GATGGAAAGAAAGGGAAACCCCAAGAAACCCAGTTT  
  \+  
  \!''\*((((\*\*\*+))%%%++)(%%%%).1\*\*\*-+\*''))\*\*55CCF\>\>\>\>\>\>CCCCCCC65  
* 

### **Variant Calling: VCF**

The **Variant Call Format (VCF) (.vcf)** is a standardized text file used to store information about genetic variations (variants) identified in one or more samples, relative to a reference genome. These variants can include single nucleotide polymorphisms (SNPs), insertions, deletions (indels), and structural variations.

* **Metadata:** The file usually starts with a header section (lines beginning with "\#\#") that describes the format version, the reference genome used, and provides definitions for the information fields in the data lines.

* **Data Lines:** Each data line represents a single variant and contains information across several columns, including:

  * **CHROM:** The chromosome on which the variant occurs.  
  * **POS:** The 1-based position of the variant on the chromosome.  
  * **ID:** An identifier for the variant (can be a database ID or a custom ID).  
  * **REF:** The reference allele (the base(s) in the reference genome at this position).  
  * **ALT:** The alternate allele(s) observed in the samples.  
  * **QUAL:** A Phred-scaled quality score for the assertion made in the ALT column.  
  * **FILTER:** A flag indicating whether the variant passed certain quality filters.  
  * **INFO:** Additional information about the variant, often encoded as key-value pairs (e.g., allele frequency, functional annotation).  
  * **FORMAT:** Specifies the format of the genotype information for each sample.  
  * **Sample Columns:** One or more columns, each representing a sample, containing genotype information as specified in the FORMAT column (e.g., the alleles present in the sample, genotype quality, read depth).

* Code snippet  
  \#\#fileformat=VCFv4.2  
  \#\#reference=GRCh38  
  \#\#contig=\<ID=chr1,length=248956422\>  
  \#\#INFO=\<ID=AC,Number=A,Type=Integer,Description="Total number of alternate alleles in called genotypes"\>  
  \#\#FORMAT=\<ID=GT,Number=1,Type=String,Description="Genotype"\>  
  \#CHROM POS ID REF ALT QUAL FILTER INFO FORMAT Sample1 Sample2  
  chr1 100 rs123 A G 30 PASS AC=2 GT 1/1 0/1  
  chr1 105 . T GA 40 PASS AC=1 GT 0/1 0/0  
* 

### **Aligned Reads: BAM/SAM**

When sequencing reads are aligned to a reference genome, the resulting alignments are often stored in **SAM/BAM format**.

* **SAM (Sequence Alignment/Map) (.sam):** This is a human-readable, tab-delimited text format for storing the alignment of sequencing reads to a reference sequence. Each line represents a single alignment. It contains information about the read, its alignment position, mapping quality, and the alignment details (CIGAR string). The file also has a header section with metadata about the reference sequences and alignment.

* **BAM (Binary Alignment Map) (.bam):** This is the binary, compressed version of the SAM format. It stores the same information as SAM but in a more space-efficient and computer-readable way, making it faster for downstream analysis. BAM files are typically indexed (.bai file) to allow for efficient random access to specific genomic regions.

   Both SAM and BAM files contain similar information for each aligned read, including:

  * **QNAME:** Query template name (read identifier).  
  * **FLAG:** Bitwise flags indicating various properties of the alignment (e.g., paired-end status, mapping quality).  
  * **RNAME:** Reference sequence name (chromosome).  
  * **POS:** 1-based leftmost mapping position.  
  * **MAPQ:** Mapping quality score.  
  * **CIGAR:** Concise Idiosyncratic Gapped Alignment Report string, describing the alignment (matches, mismatches, insertions, deletions).  
  * **RNEXT, PNEXT, TLEN:** Information about the mate pair in paired-end sequencing.  
  * **SEQ:** The read sequence.  
  * **QUAL:** The base quality scores of the read.  
  * **Optional Fields:** Additional information in the form of tags (e.g., alignment score, edit distance).

### **Omics Data**

The term "**omics**" refers to fields of biological study that end with "-omics," such as genomics, transcriptomics, proteomics, and metabolomics. These fields involve the large-scale analysis of biological molecules. Each "omic" generates its own types of data and often uses specific file formats.

* **Genomics:** Deals with the study of genomes (DNA). Formats include FASTA (for sequences), FASTQ (for raw reads), VCF (for variants), and BAM/SAM (for aligned reads). Specialized formats also exist for genome assemblies (e.g., AGP).  
* **Transcriptomics:** Studies the transcriptome (RNA). Raw data is often in FASTQ format (RNA-Seq). Processed data includes tables of gene expression counts, often in simple tab-delimited text files or more structured formats like **TSV (Tab-Separated Values)** or **CSV (Comma-Separated Values)**. Specialized formats might exist for specific analysis tools.  
* **Proteomics:** Focuses on proteins. Data is often generated by mass spectrometry and can be stored in formats like **mzML**, **mzXML**, and **mzID**. Processed data can be in tabular formats with protein identifications, abundances, and modifications.  
* **Metabolomics:** Studies small molecules (metabolites). Similar to proteomics, mass spectrometry data formats like **mzML** and **mzXML** are common. Processed data often consists of tables with metabolite identifications and their concentrations.

**Key Considerations for Omics Data:**

* **Metadata:** Crucial for interpreting omics data. This includes experimental design, sample information, and processing steps, often stored in separate files or databases. Standardized metadata formats and reporting guidelines are actively being developed.  
* **Tabular Formats:** Many processed omics datasets end up in tabular formats (TSV, CSV) for easy analysis and integration with statistical software.  
* **Specialized Formats:** Depending on the specific technology and analysis pipeline, other specialized file formats might be encountered.

Understanding these fundamental biological data formats is essential for anyone working with genomic and other high-throughput biological data. They provide a common language for storing, sharing, and analyzing the vast amounts of information generated by modern biological research.

