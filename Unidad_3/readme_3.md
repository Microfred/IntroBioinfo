
# Unidad 3

## Filtering and Trimming Reads (Trimeado)
### Eliminación de artefactos

  Luego del análisis de las lecturas, es conveniente filtrar lecturas de mala calidad y
procesar lecturas que puedan contener adaptadores u otros sesgos. Un procesamiento
adecuado de las lecturas es clave para mejorar la calidad promedio de las lecturas y disminuir
la probabilidad de errores en el ensamblaje.

  Cualquier problema como una alta duplicación, secuencias de baja calidad, inclusión de adaptadores
secuencias adaptadoras, etc., puede introducir un **sesgo** en los análisis posteriores. Las secuencias deben ser
tratadas para reducir ese sesgo. Ambos pasos pueden completarse con Trimmomatic.

Shit in => Shit out [SiSo](https://www.urbandictionary.com/define.php?term=siso)

¿cuándo hacemos un Trimming?

+ Necesitamos incrementar la calidad de nuestras lecturas
+ Los picos en el "contenido de GC por secuencia" suelen indicar contaminación por adaptadores
+ Secuencias sobrerepresentadas
+ Contenido de "Kmer" podría identificar ciertas contaminaciones del adaptador que necesita ser recortado o eliminado

Existen varios softwares para realizar estos pasos

* [Cutadapt](https://cutadapt.readthedocs.io/en/stable/index.html)
  Cutadapt encuentra y elimina secuencias adaptadoras, cebadores, colas poli-A y otros tipos de secuencias no deseadas de sus lecturas de secuenciación de alto rendimiento.

* [trim_galore](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md)
  es un script de cobertura para automatizar la calidad y el recorte de adaptadores, así como el control de calidad, con alguna funcionalidad añadida para eliminar las posiciones de metilación sesgadas para los archivos de secuencias (paired-end, directional, non-directional)

* [FASTX-T](http://hannonlab.cshl.edu/fastx_toolkit/)
  El FASTX-Toolkit es una colección de herramientas de línea de comandos para el preprocesamiento de archivos FASTA/FASTQ de Short-Reads.



###[Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)
#### Instalación

Trimmomatic puede usarse tanto con lecturas single-end como paired-end. En el caso
de las lecturas paired-end, el programa realiza otras tareas como verificar que cada par de
lecturas esté completo, es decir, que esté presente la lectura forward y reverse del par.

Las lecturas que no tienen su par son separadas en un archivo diferente (Forward y
Reverse unpaired) y posteriormente podrán ser utilizadas como lecturas single-end. También
se incluyen en esos archivos las lecturas a las que su par fue eliminado por no cumplir alguno
de los criterios especificados.

```
cd Downloads/
◦ wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
◦ unzip Trimmomatic-0.39.zip
◦ cd Trimomatic-0.39
```
Corre con el siguiente script con parámetros por default
```
java -jar trimmomatic-0.39.jar PE -threads {number_threads} -phred33 {forward_read.fq.gz}
{reverse_read.fq.gz} –baseout {examplename} ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3
TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

+ ILLUMINACLIP: Corta el adaptador y otras secuencias específicas de illumina de las lecturaa.
+ LEADING: Cortar las bases del inicio de una lectura, si está por debajo de un umbral de calidad
+ TRAILING:cortar bases del final de una lectura, si está por debajo de un umbral de calidad
+ MINLEN: Dejar la lectura si está por debajo de una longitud especificada
+ SLIDINGWINDOW: Realiza un recorte de la ventana deslizante, cortando una vez que la calidad media dentro de la ventana cae por debajo de un umbral.
+ HEADCROP: Corta el número especificado de bases desde el inicio de la lectura

**NOTA** El recorte se produce en el orden en que se especifican los pasos en la línea de comandos. En la mayoría de los casos se recomienda que el recorte del adaptador, si es necesario, se realice lo antes posible.

```
java -jar trimmomatic-0.39.jar PE -phred33 sample_R1.fastq.gz sample_R2.fastq.gz sampleT_R1.fastq.gz sampleU_R1.fastq.gz sampleT_R2.fastq.gz sampleU_R2.fastq.gz SLIDINGWINDOW:4:28 MINLEN:36
```



### trim_galore
#### [Instalación](https://anaconda.org/bioconda/trim-galore)

```
trim_galore --paired --three_prime_clip_R1 15 --three_prime_clip_R2 15 *.clock_UMI.R1.fq.gz *.clock_UMI.R2.fq.gz
```

este script ya está probado:

```
trim_galore \
 --paired --illumina --retain_unpaired --fastqc \
 -o . *1.fq.gz *2.fq.gz
```

Finalmente debemos evaluar nuestras secuencias después del control de calidad, lo realizaremos 
en FASTCQ, comparamos el antes y el después.


# ahora vamos a ensamblar.



