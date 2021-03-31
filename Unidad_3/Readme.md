
#EnsAmble de genomas

### Crearemos un directorio y copiaremos ahí los datos de secuenciación

´mkdir -p $HOME/Ensamble/ && cd $_´


(https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0060204)[“Identification of Optimum Sequencing Depth Especially for De Novo Genome Assembly of Small Genomes Using Next Generation Sequencing Data”. Desai et al. PLOS One 2013]

Existen diferentes bases de datos que podemos utilizar para el análisis de genomas.

Links:
SRA: https://www.ncbi.nlm.nih.gov/sra/ERX008638
ENA: https://www.ebi.ac.uk/ena/data/view/ERX008638

#6.1 Tres escenarios de datos

Los cambios realizados en estos tres escenarios producen cambios en el nombre de los archivos por lo que se recomienda tener un respaldo de los archivos originales con los nombres originales.
Es recomendable documentar si se renombraron los archivos en un archivo README.txt, README.md en el folder del proyecto.

MiSeq, un archivo por muestra

sampleA_S1_R1_001.fastq.gz
sampleA_S1_R2_001.fastq.gz

Este script hace
find -maxdepth 1 -name "*fastq.gz" -type f | rename 's



NextSeq, cuatro archivos por muestra, L001-L004


sampleA_S1_L001_R1_001.fastq.gz
sampleA_S1_L001_R2_001.fastq.gz

sampleA_S1_L002_R1_001.fastq.gz
sampleA_S1_L002_R2_001.fastq.gz

sampleA_S1_L003_R1_001.fastq.gz
sampleA_S1_L003_R2_001.fastq.gz

sampleA_S1_L004_R1_001.fastq.gz
sampleA_S1_L004_R2_001.fastq.gz


Podemos concatenar los archivos unos por uno :(

cat sampleA_S1_L00?_R1* > sampleA_S1_L001_R1_001.fastq.gz
cat sampleA_S1_L00?_R2* > sampleA_S1_L001_R2_001.fastq.gz

O podemos usar iteraciones para concatenar los archivos de las 4 líneas, L001-L004, de Illumina al formato sampleA_S1_R1.fastq.gz
while read i
do
    cat ${i}*R1*.fastq.gz > ${i}_R1.fastq.gz
    cat ${i}*R2*.fastq.gz > ${i}_R2.fastq.gz
done < <(ls *R1*gz | cut -d\_ -f1,2 | sort | uniq )

NextSeq, un archivo por muestra, L001

sampleA_S1_L001_R1_001.fastq.gz
sampleA_S1_L001_R2_001.fastq.gz


Arrancamos...


Comparar lecturas, antes y después del control de calidad

mkdir -p $HOME/Ensamble/00_raw && mv *fastq.gz* 00_raw/

cd 00_raw

zcat Q1_CSFP200001976-1a_H57HLDSXY_L1_1.fq.gz | seqkit sample -p 0.15 -o Salbidoflavus_S01_R1.fastq.gz
zcat Q1_CSFP200001976-1a_H57HLDSXY_L1_2.fq.gz | seqkit sample -p 0.15 -o Salbidoflavus_S01_R2.fastq.gz



cd $HOME/Ensamble

# Número de secuencias en el archivo ecoli_S01_R1.fastq.gz
zcat 00_raw/Salbidoflavus_S01_R1.fastq.gz  | awk 'END{ print NR/4 }'

#Cuento cuántas secuencias tengo y de qué longitud en el archivo ecoli_S01_R1.fastq.gz
zcat 00_raw/Salbidoflavus_S01_R1.fastq.gz  | \
      awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c

# Número de secuencias en el archivo ecoli_S01_R2.fastq.gz
zcat 00_raw/Salbidoflavus_S01_R2.fastq.gz | awk 'END{ print NR/4 }'

#Cuento cuántas secuencias tengo y de qué longitud en el archivo ecoli_S01_R2.fastq.gz
zcat 00_raw/Salbidoflavus_S01_R2.fastq.gz | \
      awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c

    #  7.1 FastQC
El formato FASTQ es una simple extensión del formato FASTA: permite la habilidad de almacenar información alfanumérica de la calidad asociada a cada nucleótido en una secuencia.

Cada archivo fastq tiene cuatro lineas:

* 1.- Nombre de la secuencia (header - id del secuenciador, coordenadas del spot, flow-cell, adaptador, etc.)
* 2.- Secuencia
* 3.- Espaciador (+)
* 4.- Valores de calidad: (Q Score)[https://en.wikipedia.org/wiki/FASTQ_format] - alfanumérico.

Imagen



Puntuación de calidad y precisión en el base calling:


tabla phred




## Dentro del folder $HOME/Ensamble/00_raw/ se encuentran los archivos que utilizaremos para el ensamble.

Creamos una carpeta para el control de calidad de las lecturas

mkdir -p $HOME/Ensamble/01_qc


Utilizamos FastQC para obtener la gráficas de calidad de las lecturas. ES NECESARIO ACTIVAR EL ENTORNO QC

cd $HOME/Ensamble
source activate qc
fastqc --nogroup -f fastq 00_raw/Salbidoflavus*fastq.gz -o 01_qc
conda deactivate

# En este caso en particular, las secuencias no tienen adaptador ¿o sí? y tienen la misma longitud.

#Para paired-end
cd $HOME/Ensamble
git clone https://github.com/ATGenomics/adapters.git $HOME/bin/adapters
adapters="$HOME/bin/adapters/NexteraPE-PE.fa"

source activate qc

trimmomatic PE -phred33 -threads 16 \
    00_raw/Salbidoflavus_S01_R1.fastq.gz  00_raw/Salbidoflavus_S01_R2.fastq.gz  \
    01_qc/Salbidoflavus_S01_R1.trim.fastq.gz 01_qc/Salbidoflavus_S01_1U.trim.fq.gz \
    01_qc/Salbidoflavus_S01_R2.trim.fastq.gz 01_qc/Salbidoflavus_S01_2U.trim.fq.gz \
    ILLUMINACLIP:${adapters}:2:30:10 SLIDINGWINDOW:4:20 MINLEN:130 CROP:100

  conda deactivate qc


  Comparar lecturas, antes y después del recorte:

  #Cuento cuántas secuencias tengo en el archivo `01_qc/Salbidoflavus_S01_R1.trim.fastq.gz`
zcat 01_qc/Salbidoflavus_S01_R1.trim.fastq.gz | awk 'END{ print NR/4 }'

#Cuento cuántas secuencias tengo y de qué longitud en el archivo `01_qc/Salbidoflavus_S01_R1.trim.fastq.gz`
zcat 01_qc/Salbidoflavus_S01_R1.trim.fastq.gz | \
    awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c

#Cuento cuántas secuencias tengo en el archivo `01_qc/Salbidoflavus_S01_R1.trim.fastq.gz`
zcat 01_qc/Salbidoflavus_S01_R1.trim.fastq.gz | awk 'END{ print NR/4 }'


# Los mismo pero para R2:
#Cuento cuántas secuencias tengo en el archivo `01_qc/Salbidoflavus_S01_R2.trim.fastq.gz`
zcat 01_qc/ecoli_S01_R2.trim.fastq.gz | awk 'END{ print NR/4 }'

#Cuento cuántas secuencias tengo y de qué longitud en el archivo `01_qc/ecoli_S01_R2.trim.fastq.gz`
zcat 01_qc/ecoli_S01_R2.trim.fastq.gz | \
    awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c

#Cuento cuántas secuencias tengo en el archivo `01_qc/ecoli_S01_2U.trim.fq.gz`
zcat 01_qc/ecoli_S01_2U.trim.fq.gz | awk 'END{ print NR/4 }'


# Evaluación de PhiX

Crear index de la referencia:

cd $HOME/Ensamble

source activate qc

tar -zxvf PhiX_Illumina_RTA.tar.gz && rm PhiX_Illumina_RTA.tar.gz

bwa index PhiX/Illumina/RTA/Sequence/Chromosomes/phix.fa -p 01_qc/phix

conda deactivate
Mapear lecturas versus la referencia:

cd $HOME/Ensamble

source activate qc
bwa mem -t 8 01_qc/phix \
    01_qc/ecoli_S01_R1.trim.fastq.gz \
    01_qc/ecoli_S01_R2.trim.fastq.gz | \
    samtools view -bS -f4 - | \
    samtools sort -@ 4 - -o 01_qc/ecoli.phix.sorted.bam

samtools fastq \
    -1 01_qc/ecoli_S01_R1.trim.clean.fastq.gz \
    -2 01_qc/ecoli_S01_R2.trim.clean.fastq.gz \
    -s 01_qc/ecoli_S01_S.trim.clean.fastq.gz \
    01_qc/ecoli.phix.sorted.bam
Contar, DE NUEVO.


ls -ltrh 01_qc/

zcat 01_qc/ecoli_S01_R1.trim.clean.fastq.gz | awk 'END{ print NR/4 }'


zcat 01_qc/ecoli_S01_R1.trim.clean.fastq.gz | awk 'END{ print NR/4 }'
