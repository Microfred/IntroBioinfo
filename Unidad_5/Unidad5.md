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

## ![5.2 RAST](http://rast.nmpdr.org/])

+ Otra manera, muy popular por cierto, es realizar una anotación automatizada en el servidor web RAST, 
accesible desde la página.
![rast01.png](rast01.png)

+ Es necesario registrarse.
![rast02.png](rast02.png)

+ Puedes lanzar varias anotaciones simultaneas, el servidor indicará cuál es la carga de trabajo actual.
![rast03.png](rast03.png)

+ Vamos a la pestaña **Your jobs** > **Upload new job**
![rast04.png](rast04.png)

+ Seleccionamos el archivo de los contigs, A5 en este caso.
![rast05.png](rast05.png)

+ Nos da un resumen del genoma.
![rast06.png](rast06.png)

+ En caso de ser un genoma resecuenciado podemos llenar los campos con la información de la referencia.
![rast07.png](rast07.png)

+ En este caso sólo selecciono el código 11 para codones de bacteria y lleno los campos de género y especie.
![rast08.png](rast08.png)

Seleccionamos el algoritmo para anotación: GLIMMER, RAST
![rast09.png](rast09.png)
![rast10.png](rast10.png)

Enviamos nuestra secuencia y el sistema nos notificará vía correo electrónico cuando nuestra anotación esté lista.
![rast11.png](rast11.png)










