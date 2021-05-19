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
Breath
conda deactivate
```
mkdir -p 03_assembly_qc

Velvet

cd $HOME/Ensamble

source activate qc
conda deactivate
```
* tomamos como referencia nuestro propio Ensamble y mapeamos nuestras lecturas trimeadas vs el ensamble
El mapeo de secuencias se lleva a cabop en 3 pasos
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
    samtools view -Sb - | samtools sort - -o 03_assembly_qc/spades_assembly

conda deactivate
conda deactivate
```
samtool view: lee un formato y convierte a otro
samtools sort: ordena

SAM/BAM

SAM son las siglas para Sequence Alignment Map, un formato de texto delimitado por tabuladores; consiste de un header, el cual es opcional, y una sección de alineamiento. En caso de estar presente el header empiezan con el símbolo @. Cada alineamiento tiene 11 campos obligatorios para información esencial del alineamiento, como posición de mapeo y otros campos que hacen que formato SAM flexible y específico.

figura

ejemplo
header
tipo de alineamiento
secuencias

vamos a obtener: el identificador de cada una de las lecturas y la identificación de donde mapeo en el Ensamble
cuanto nucleotidos alinearon, si alineó R1 ó R2.
