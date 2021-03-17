
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

*[FASTX-T][http://hannonlab.cshl.edu/fastx_toolkit/]
  El FASTX-Toolkit es una colección de herramientas de línea de comandos para el preprocesamiento de archivos FASTA/FASTQ de Short-Reads.

