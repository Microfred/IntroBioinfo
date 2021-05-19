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
one_liner 02_assembly/denovo_velvet/ec_31/contigs.fa

grep -v \> 02_assembly/denovo_velvet/ec_31/contigs.fa | awk '{ print length }' | sort -nr | uniq
```

Para denovo_spades

```
one_liner 02_assembly/denovo_spades/ecoli/scaffolds.fasta

grep -v \> 02_assembly/denovo_spades/ecoli/scaffolds.fasta | awk '{ print length }' | sort -nr | uniq
```


## Estadísticos del Ensamble

1º verificamos que assembly_stats esté instalado
`assembly-stats`


2º instalar el programa assembly_stats con **mamba**

`mamba install -n qc -yc bioconda assembly_stats`

### Un vistazo general a los archivos de ensamble:

```
source activate qc
assembly-stats 02_assembly/denovo_spades/ecoli/scaffolds.fasta
assembly-stats 02_assembly/denovo_velvet/ec_31/contigs.fa
conda deactivate
```











