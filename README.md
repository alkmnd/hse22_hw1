# hse22_hw1
# Homework 1
# Belova Natalya

##### Создадим ссылки в папке:
```
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {} 
```

##### Выбор случайных чтений:
```
seqtk sample -s612 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s612 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s612 oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
seqtk sample -s612 oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
```

##### Оцененим чтения, используя FastQC:
```
mkdir fastqc
ls sub* matep* | xargs -tI{} fastqc -o fastqc {}
```

##### Создадим отчет через MultiQC:
```
mkdir multiqc
multiqc -o multiqc fastqc
```
##### Выбираем случaйные чтения как в пункте 2.
##### Обрежем чтения:
```
platanus_trim sub*
platanus_internal_trim matep*
```

##### Удаляем исходные файлы:
```
rm sub1.fastq
rm sub2.fastq
rm matep1.fastq
rm matep2.fastq
```
##### Oцениваем качество обрезанных чтений через FastQC и Multiqc:
```
mkdir fastqc_trimmed
ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
```
```
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```

##### Cбор контиг:
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```

##### Сбор скаффолдов:
```
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> scaffold.log
```

##### Уменьшение числа промежутков:
```
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> gapclose.log
```

##### Удаляем подрезанные чтения:
```
rm sub*.trimmed matep*.int_trimmed
```
---
## Полученные отчеты
### Для необрезанных чтений:
![Image alt](https://github.com/alkmnd/hse22_hw1/raw/main/images/m1.png)
![Image alt](https://github.com/alkmnd/hse22_hw1/raw/main/images/m2.png)
### Для обрезанных чтений: 
![Image alt](https://github.com/alkmnd/hse22_hw1/raw/main/images/m1_trimmed.png)
![Image alt](https://github.com/alkmnd/hse22_hw1/raw/main/images/m2_trimmed.png)
---
##### Анализ контигов, скаффолдов, подсчет кол-ва гэпов доступны по ссылке на Google Collab:
```
https://colab.research.google.com/drive/1Krq_ukieMcxLf1Euqzu5w2BriM539OpN?usp=sharing
```
