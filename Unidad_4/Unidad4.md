# Unida 4. Calidad de Ensamble


La **calidad del Ensamble** se mide en covertura y profundidad, a que se refiere con ésto:

Covertura: A lo largo del genoma, es decir de 7.0 mpb ¿cuánto estoy cubriendo?

Colocar cada contig en una sola línea y contar líneas y la distribución de su longitud.


i) colocar el contig en una sola líne es decir:

>contig1 AGTCGCTGCTCGCTAGATAGATCGACTCGACAGCTATCACTGCAGTACTGCATGCATGCATGCTGCATGCATGCATGCTACG...

>contig2 AGTCGCTGCTCGCTAGATAGATCGACTCGACAGCTATCACTGCAGTACTGCATGCATGCATGCTGCATGCATGCATGCTACG...

>contig3 AGTCGCTGCTCGCTAGATAGATCGACTCGACAGCTATCACTGCAGTACTGCATGCATGCATGCTGCATGCATGCATGCTACG...

>contig4 AGTCGCTGCTCGCTAGATAGATCGACTCGACAGCTATCACTGCAGTACTGCATGCATGCATGCTGCATGCATGCATGCTACG...

ii) conteo de contigs con número de nucléotidos
```
67289
58778
43899
39898
```
utilizando los siguientes comandos

Para denovo_velvet

```
one_liner 02_assembly/denovo_velvet/salbi_61/contigs.fa

grep -v \> 02_assembly/denovo_velvet/salbi_61/contigs.fa | awk '{ print length }' | sort -nr | uniq
```
velvet_assembly
Para denovo_spades

```
one_liner 02_assembly/denovo_spades/Salbidoflavus/scaffolds.fasta

grep -v \> 02_assembly/denovo_spades/Salbidoflavus/scaffolds.fasta | awk '{ print length }' | sort -nr | uniq
```


## Estadísticos del Ensamble

1º verificamos que assembly_stats esté instalado

`assembly-stats`


2º instalar el programa assembly_stats con **mamba**

`mamba install -n qc -yc bioconda assembly_stats`

### Un vistazo general a los archivos de ensamble:
¿cuál es el mejor ensamble?

a lo largo
>atcatgctagtagctacgtagctagcatgctagcatgcatgcatgctagctagcta

a lo profundo
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac
atcatgctagtagctac


```
source activate qc
assembly-stats 02_assembly/denovo_spades/Salbidoflavus/scaffolds.fasta
assembly-stats 02_assembly/denovo_velvet/salbi_61/contigs.fa
conda deactivate
```

Observamos el número de contigs
promedio de largo:
Sum= tamaño del genoma esperado.
n= número de contigs.
ave= promedio de longitud de los contigs.
largest= el contig más largo.
¿cuál es la longitud de mis contings que me representa x%?
n50= al menos el 50% de mis contigs tendrán esa longitud.
n90= al menos 90% de mis contigs tendrán esa longitud.
etc..



# 5 Amplitud y profundidad de cobertura

¿cuál es el rendimiento de ese genoma ensamblado?
¿qué pasaría si mapeo las lecturas con el ensamble?

Depth
Breadth

conda deactivate

```
mkdir -p 03_assembly_qc

Spades

cd $HOME/Ensamble

source activate qcBreath
conda deactivate
```
* tomamos como referencia nuestro propio Ensamble y mapeamos nuestras lecturas trimeadas vs el ensamble
El mapeo de secuencias se lleva a cabo en 3 pasos

+ indexar
+ mapear
+ filtrar

esta en un sólo script
bwa es un "algoritmo Burrows-Wheeler Aligner", nos permite ubicar nuestras lecturas sobre el genoma de referencia.
Utilizaremos un formato sam

conda deactivate
```
bwa index -p 03_assembly_qc/spades_assembly 02_assembly/denovo_spades/Salbidoflavus/scaffolds.fasta

bwa mem -t 20 03_assembly_qc/spades_assembly \
    01_qc/Salbidoflavus_S01_R1.trim.fastq.gz 01_qc/Salbidoflavus_S01_R2.trim.fastq.gz | \
    samtools view -Sb - | sreathenamtools sort - -o 03_assembly_qc/spades_assembly.sorted.bam

conda deactivateBreath
conda deactivate
```
samtool view: lee un formato y convierte a otro
samtools sort: ordena

## SAM/BAM

SAM son las siglas para Sequence Alignment Map, un formato de texto delimitado por tabuladores; consiste de un header, el cual es opcional, y una sección de alineamiento. En caso de estar presente el header empiezan con el símbolo @. Cada alineamiento tiene 11 campos obligatorios para información esencial del alineamiento, como posición de mapeo y otros campos que hacen que formato SAM flexible y específico.

![figura_1.png](figura_1.png)

ejemplo

![bam.png](bam.png)

header
tipo de alineamiento
secuencias

vamos a obtener: el identificador de cada una de las lecturas y la identificación de donde mapeo en el Ensamble
cuanto nucleotidos alinearon, si alineó R1 ó R2.

samtools (disponible en el entorno de qc)
samtools es una herramienta básica en la bioinformática, básicamente se utiliza para alineamientos


```
cd $HOME/Ensamble

source activate qc

samtools view -b -f 4 \
    03_assembly_qc/spades_assembly.sorted.bam\
    > 03_assembly_qc/spades_assembly.unmapped.02_assembly/denovo_spades/Salbidoflavus/scaffolds.fastasorted.bam

samtools view -b -F 4 \
    03_assembly_qc/spades_assembly.sorted.bam \
    > 03_assembly_qc/spades_assembly.mapped.sorted.bam

samtools index 03_assembly_qc/spades_assembly.sorted.bam
samtools index 03_assembly_qc/spades_assembly.mapped.sorted.bam

conda deactivate
```
-b bam
-F 4 mapped
-f 4 unmapped


depth coverage: por cada posición

```
source activate qc
samtools depth 03_assembly_qc/spades_assembly.mapped.sorted.bam \
            > 03_assembly_qc/spades_assembly_depth.txt
```


#### Bases mapped and sum depth: 6961535 240579162, total de bases mapeadas
awk '{ c++; s+=$3 } END {print c " " s}' 03_assembly_qc/spades_assembly_depth.txt

#### Sum depth (profundidad): 34.5583
awk '{ c++; s+=$3 } END { print s/c }' 03_assembly_qc/spades_assembly_depth.txt

#### Breadth of coverage (with >= 5X coverage depth; Amplitud-longitud): 99.40
awk '{c++; if($3>5) total+=1}END{print (total/02_assembly/denovo_spades/Salbidoflavus/scaffolds.fastac)*100}' 03_assembly_qc/spades_assembly_depth.txt

#### Ahora del archivo mapeado lo voy a convertir en fastq para poder contar

samtools fastq -c 9 \
        -1 03_assembly_qc/Salbidoflavus_S01_R1.fastq.gz \
        -2 03_assembly_qc/Salbidoflavus_S01_R2.fastq.gz \
        -s 03_assembly_qc/Salbidoflavus_S.fastq.gz \
        03_assembly_qc/spades_assembly.mapped.sorted.bam

# Contamos líneas, por enésima vez:

zcat 03_assembly_qc/Salbidoflavus_S01_R1.fastq.gz | awk 'END{ print NR/4 }'
zcat 03_assembly_qc/Salbidoflavus_S01_R2.fastq.gz | awk 'END{ print NR/4 }'
zcat 03_assembly_qc/Salbidoflavus_S.fastq.gz | awk 'END{ print NR/4 }'



# Ensamble con referencia

Mapeo de reads vs referencia
cd $HOME/Ensamble

mkdir -p 04_assembly_ref/
Descargamos la referencia:

wget -P 04_assembly_ref/ https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.fna.gz

Mapeamos las lecturas de 01_qc versus el genoma de referencia.

cd $HOME/Ensamble

source activate qc

bwa index -p 04_assembly_ref/salbi_61 \
     04_assembly_ref/GCF_000359525.1_ASM35952v1_genomic.fna.gz

bwa mem -t 20 04_assembly_ref/salbi_61 \
   01_qc/Salbidoflavus_S01_R1.trim.fastq.gz 01_qc/Salbidoflavus_S01_R2.trim.fastq.gz | \
   samtools view -Sb - | samtools sort - -o 04_assembly_ref/salbi_61_vs_reads.sorted.bam

samtools index 04_assembly_ref/salbi_61_vs_reads.sorted.bam

conda deactivate

#### Se filtra las lecturas en mapeadas y no mapeadas versus la referencia.

cd $HOME/Ensamble

source activate qc

samtools view -b -f 4 \
   04_assembly_ref/salbi_61_vs_reads.sorted.bam \
   > 04_assembly_ref/salbi_61_vs_reads.unmapped.sorted.bam

samtools view -b -F 4 \
   04_assembly_ref/salbi_61_vs_reads.sorted.bam \
   > 04_assembly_ref/salbi_61_vs_reads.mapped.sorted.bam

samtools index 04_assembly_ref/salbi_61_vs_reads.mapped.sorted.bam

conda deactivate

#### Tomamos el archivo BAM de las lecturas mapeadas:

cd $HOME/Ensamble

source activate qc

samtools depth 04_assembly_ref/salbi_61_vs_reads.mapped.sorted.bam \
           > 04_assembly_ref/salbi_61_vs_reads_depth.txt

conda deactivate

# Bases mapped and sum depth: 6423733 220023401
awk '{ c++; s+=$3 } END {print c " " s}' 04_assembly_ref/salbi_61_vs_reads_depth.txt

# Sum depth: 34.2516
awk '{ c++; s+=$3 } END { print s/c }' 04_assembly_ref/salbi_61_vs_reads_depth.txt

# Breadth of coverage (with >= 5X coverage depth): 99.3396
awk '{c++; if($3>5) total+=1}END{print (total/c)*100}' 04_assembly_ref/salbi_61_vs_reads_depth.txt


cd $HOME/Ensamble

source activate qc

samtools fastq -c 9 \
       -1 04_assembly_ref/salbi_61_vs_reads_R1.fastq.gz \
       -2 04_assembly_ref/salbi_61_vs_reads_R2.fastq.gz \
       -s 04_assembly_ref/salbi_61_vs_reads_S.fastq.gz \
       04_assembly_ref/salbi_61_vs_reads.mapped.sorted.bam

conda deactivate

#### Contamos líneas, por enésima vez:

##### Lecturas
zcat 01_qc/Salbidoflavus_S01_R1.trim.fastq.gz | awk 'END{ print NR/4 }'
zcat 01_qc/Salbidoflavus_S01_R2.trim.fastq.gz | awk 'END{ print NR/4 }'
zcat 01_qc/Salbidoflavus_S01_1U.trim.fq.gz | awk 'END{ print NR/4 }'
zcat 01_qc/Salbidoflavus_S01_2U.trim.fq.gz | awk 'END{ print NR/4 }'

# Lecturas que mapearon con la referencia
zcat 04_assembly_ref/salbi_61_vs_reads_R1.fastq.gz | awk 'END{ print NR/4 }'
zcat 04_assembly_ref/salbi_61_vs_reads_R2.fastq.gz | awk 'END{ print NR/4 }'
zcat 04_assembly_ref/salbi_61_vs_reads_S.fastq.gz | awk 'END{ print NR/4 }'




9.2 Ensamble con referencia
cd $HOME/Ensamble

mkdir -p 04_assembly_ref/velvet/

velveth 04_assembly_ref/velvet/ec_31 31 -fasta \
     -reference 04_assembly_ref/GCF_000005845.2_ASM584v2_genomic.fna.gz \
     -fastq -shortPaired -separate \
     01_qc/ecoli_S01_R1.trim.fastq.gz 01_qc/ecoli_S01_R2.trim.fastq.gz

# Este paso necesita que Velvet se compila con la opción LONGSEQUENCES
velvetg 04_assembly_ref/velvet/ec_31 -read_trkg yes -ins_length 500 \
         -cov_cutoff 40 -exp_cov 80 -min_contig_lgth 500 -amos_file yes
Contamos líneas:

grep -c \> 04_assembly_ref/velvet/ec_31/contigs.fa

one_liner 04_assembly_ref/velvet/ec_31/contigs.fa

grep -v \> 04_assembly_ref/velvet/ec_31/contigs.fa | awk '{ print length }' | sort -nr | uniq

source activate qc
assembly-stats 04_assembly_ref/velvet/ec_31/contigs.fa
conda deactivate
Si los comparamos con el ensamble de novo


grep -c \> 02_assembly/denovo_velvet/ec_31/contigs.fa

grep -v \> 04_assembly_ref/velvet/ec_31/contigs.fa | awk '{ print length }' | sort -nr | uniq

source activate qc
assembly-stats 04_assembly_ref/velvet/ec_31/contigs.fa
conda deactivate
