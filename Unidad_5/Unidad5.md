
# 5.0 Anotación
## 5.1 Prokka

Existen 2 algoritmos para realizar la anotación: genemarker y Prodigal

```
cd $HOME/Ensamble

mkdir -p 05_annotation

cp 02_assembly/denovo_spades/ecoli/scaffolds.fasta 05_annotation/spades_assembly.fasta

cp 02_assembly/denovo_velvet/ec_31/contigs.fa 05_annotation/velvet_assembly.fasta

source activate annotation

prokka --outdir 05_annotation/ec_spades_prokka --gcode 11 --evalue 1e-6 \
    --prefix ec_spades 05_annotation/spades_assembly.fasta

prokka --outdir 05_annotation/ec_velvet_prokka --gcode 11 --evalue 1e-6 \
    --prefix ec_velvet 05_annotation/velvet_assembly.fasta

conda deactivate
```

#### Archivos de salida:

|Extension | Description |
| --- | --- |
|.gff	| This is the master annotation in GFF3 format, containing both sequences and annotations. It can be viewed directly in Artemis or IGV. |
|.gbk	| This is a standard Genbank file derived from the master .gff. If the input to prokka was a multi-FASTA, then this will be a multi-Genbank, with one record for each sequence. |
|.fna |	Nucleotide FASTA file of the input contig sequences. |
|.faa	| Protein FASTA file of the translated CDS sequences. |
|.ffn	| Nucleotide FASTA file of all the prediction transcripts (CDS, rRNA, tRNA, tmRNA, misc_RNA) |
|.sqn	| An ASN1 format “Sequin” file for submission to Genbank. It needs to be edited to set the correct taxonomy, authors, related publication etc. |
|.fsa	| Nucleotide FASTA file of the input contig sequences, used by “tbl2asn” to create the .sqn file. It is mostly the same as the .fna file, but with extra Sequin tags in the sequence description lines. |
|.tbl	| Feature Table file, used by “tbl2asn” to create the .sqn file. |
|.err	| Unacceptable annotations - the NCBI discrepancy report. |
|.log	| Contains all the output that Prokka produced during its run. This is a record of what settings you used, even if the –quiet option was enabled. | 
|.txt	| Statistics relating to the annotated features found. |
|.tsv	| Tab-separated file of all features: locus_tag,ftype,len_bp,gene,EC_number,COG,product |

## 5.2 RAST
