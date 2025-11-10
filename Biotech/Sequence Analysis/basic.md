abbr
KM: mean abundance

KC: total abundance

LN

GC: GC content

GCS

CPG

CWF

CE

tutorials
[https://www.biostars.org/t/tutorials/?order=votes](https://www.biostars.org/t/tutorials/?order=votes)

python packages
[https://edinburgh-genome-foundry.github.io](https://edinburgh-genome-foundry.github.io)

[https://www.bioinformatics.recipes](https://www.bioinformatics.recipes)

[https://opensourcelibs.com/libs/biology](https://opensourcelibs.com/libs/biology)

[https://opensourcelibs.com/libs/evolution](https://opensourcelibs.com/libs/evolution)

**FASTQ+**
[https://github.com/shiquan/PISA?tab=readme-ov-file](https://github.com/shiquan/PISA?tab=readme-ov-file)
[https://academic.oup.com/bioinformatics/article/38/19/4639/6673134](https://academic.oup.com/bioinformatics/article/38/19/4639/6673134)
The FASTQ+ format is designed for single-cell experiments. It extends various optional tags, including cell barcodes and unique molecular identifiers, to the sequence identifier and is fully compatible with the FASTQ format. In addition, PISA implements various utilities for processing sequences in the FASTQ format and alignments in the SAM/BAM/CRAM format from single-cell experiments, such as converting FASTQ format to FASTQ+, annotating alignments, PCR deduplication, feature counting and barcodes correction.
The FASTQ files store the barcodes in the sequence field, but they are not included in the sequence alignment or assembly.
FASTQ+ format to extend the barcode and other features for FASTQ, but the FASTQ+ format also inherits all natures of the FASTQ file. 

**FASTQ**
[https://thesequencingcenter.com/knowledge-base/fastq-files/](https://thesequencingcenter.com/knowledge-base/fastq-files/)

**GFF/GFT**
Documentation [https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md
Format 9 columns:

NC_000001.11        BestRefSeq        mRNA        65419        71585        .        +        .        ID=rna-NM_001005484.2;Parent=gene-OR4F5;Dbxref=Ensembl:ENST00000641515.2,GeneID:79501,Genbank:NM_001005484.2,HGNC:HGNC:14825;Name=NM_001005484.2;gbkey=mRNA;gene=OR4F5;product=olfactory receptor family 4 subfamily F member 5;tag=MANE Select;transcript_id=NM_001005484.2

1. Sequence ID
2. Soaurce
3. Type
4. Start
5. End
6. Score
7. Strand: The strand of the feature. + for positive strand (relative to the landmark), - for minus strand, and . for features that are not stranded. In addition, ? can be used for features whose strandedness is relevant, but unknown.
8. Phase: For features of type "CDS", the phase indicates where the next codon begins relative to the 5' end (where the 5' end of the CDS is relative to the strand of the CDS feature) of the current CDS feature. For clarification the 5' end for CDS features on the plus strand is the feature's start and and the 5' end for CDS features on the minus strand is the feature's end. The phase is one of the integers 0, 1, or 2, indicating the number of bases forward from the start of the current CDS feature the next codon begins. A phase of "0" indicates that a codon begins on the first nucleotide of the CDS feature (i.e. 0 bases forward), a phase of "1" indicates that the codon begins at the second nucleotide of this CDS feature and a phase of "2" indicates that the codon begins at the third nucleotide of this region. Note that ‘Phase’ in the context of a GFF3 CDS feature should not be confused with the similar concept of frame that is also a common concept in bioinformatics. Frame is generally calculated as a value for a given base relative to the start of the complete open reading frame (ORF) or the codon (e.g. modulo 3) while CDS phase describes the start of the next codon relative to a given CDS feature.
9. Attributes: A list of feature attributes in the format tag=value. Multiple tag=value pairs are separated by semicolons.

Data in RefSeq Annotation (GFF)
region
pseudogene
transcript
exon
gene
primary_transcript
miRNA
lnc_RNA
mRNA
CDS
snRNA
silencer
biological_region
enhancer
transcriptional_cis_regulatory_region
meiotic_recombination_region
repeat_instability_region
nucleotide_motif
tandem_repeat
snoRNA
CAGE_cluster
DNaseI_hypersensitive_site
epigenetically_modified_region
ncRNA
antisense_RNA
non_allelic_homologous_recombination_region
tRNA
promoter
conserved_region
enhancer_blocking_element
mobile_genetic_element
minisatellite
protein_binding_site
locus_control_region
centromere
V_gene_segment
response_element
TATA_box
sequence_comparison
rRNA
sequence_feature
sequence_alteration
cDNA_match
insulator
matrix_attachment_site
repeat_region
regulatory_region
sequence_secondary_structure
C_gene_segment
J_gene_segment
origin_of_replication
mitotic_recombination_region
telomerase_RNA
CAAT_signal
dispersed_repeat
recombination_feature
vault_RNA
GC_rich_promoter_region
D_gene_segment
Y_RNA
replication_regulatory_region
RNase_MRP_RNA
sequence_alteration_artifact
microsatellite
imprinting_control_region
direct_repeat
chromosome_breakpoint
scRNA
RNase_P_RNA
replication_start_site
nucleotide_cleavage_site
D_loop


[https://ucdavis-bioinformatics-training.github.io/2019_March_UCSF_mRNAseq_Workshop/data_reduction/filetypes.html](https://ucdavis-bioinformatics-training.github.io/2019_March_UCSF_mRNAseq_Workshop/data_reduction/filetypes.html)

The primary file types you’ll see related to DNA sequence analysis are:

- FASTA: raw readings
- FASTQ: raw readings with quality control values which is a propability
- FASTQ+: format is designed for single-cell experiments.

- @DP8400023247TLL1C001R00400170541/2
- GTAACAGCCAGCCCAACTGTGGCAGGGAAACAAGTAGAGGCTTAGGATCAAATAAAATAGGGACTTATTAGGAATTCTTACCACTGTTTCTACTGTACAC
- +
- @@@@<6>E?;E;ACCE-ECDFD8F>6DD;E=@AD>ED;=DF'CECADECG;F:DFACDED5E85;DF?A=EEA6:B:AE;@GFA=E-=E>;F>=FBG=F7

- BFQ: is a binary version of the FASTQ file format
- GTF/GFF2/GFF3 - хранение и отображение аннотации геномов. Позволяют аннотацию в строгом виде и с иерархией - с указанием, какие экзоны образуют какой транскрипт, а какие транскрипты - какой ген.
- GTF: (General Transfer Format) is identical to GFF2 version 2. файл с аннотацией генома. К сожалению, большинство современных аннотаций не придерживается заданных там стандартов.
- GFF2: ()
- GFF3: (General Feature Format) отличия, другая организация уровней иерархии. Является наиболее строгим форматом аннотации
- SAM: (Sequence Alignment Map) Формат для хранения выравниваний

- I have a SAM file that I created by aligning raw illumina reads of one species to a closely related reference assembly.

- @HD - header, is_sorted
- @SQ - референсные последовательности(хромосомы)
- @RG - read group
- @PG - history of program and command
- Flags:
- NH

- BAM: (Binary Alignment Map) Бинарная версия SAM, нечитаема.
- CRAM: практически не используется, поддерживается samtools/picard
- BED: (Browser Extensible Data) 3 колонки + 9 опционально. Аннотация транскриптов

- MGF:
- GA4GH url
- VCF: Variant Call Format
- BedGraph: is a file format that allows display of continuous-valued data in a track in genome browsers that support the format.

Tools

BrumiR:  for de novo discovery of microRNAs from sRNA-seq data

miRDeep2: for identification of novel and known miRNAs in deep sequencing data

samtools

bedtools - работа с интревалами

bedops

TopHat

Cufflinks

HISAT

Ballgown

StringTie - Transcript assembly and quantification for RNA-Seq.

Main Outputs

- Stringtie's main output is a GTF file containing the assembled transcripts

- Gene abundances in tab-delimited format

- Fully covered transcripts that match the reference annotation, in GTF format

- Files (tables) required as input to Ballgown, which uses them to estimate differential expression

- In merge mode, a merged GTF file from a set of GTF files

Bowtie2

igvtools

IGV genome browser

IGVtools

FastaQC: Контроль сырых прочтений, прочтения для которых не было сделано выравниваний и сборки.

GATK Genome Analysis Toolkit

RTG

WGBS (SNP calls from WGBS performed on the same sample.)

Picard is a set of command line tools for manipulating high-throughput sequencing (HTS) data and formats such as SAM/BAM/CRAM and VCF.

Pmatch - Pmatch is a fast, exact matching program for aligning protein sequences with either protein or DNA sequence.

HISAT2 aligns RNA-seq reads to a genome and discovers transcript splice sites, while running far faster than TopHat2 and requiring much less computer memory than other methods.

StringTie8 assembles the alignments into full and partial tran- scripts, creating multiple isoforms as necessary and estimating the expression levels of all genes and transcripts.

Ballgown9 takes the transcripts and expression levels from StringTie and applies rigorous statistical methods to determine which transcripts are differentially expressed between two or more experiments.

HISAT (hierarchical indexing for spliced alignment of transcripts)

HISAT2 is a fast and sensitive alignment program for mapping next-generation sequencing reads (whole-genome, transcriptome, and exome sequencing data)

UCSC

UseGalaxy

Ensembl

GRASP

GTEx

HaploReg

David

HumanMine

BamToBfq

BamToBfq

[http://book.bionumbers.org](http://book.bionumbers.org)

[https://en.wikibooks.org/wiki/An_Introduction_to_Molecular_Biology](https://en.wikibooks.org/wiki/An_Introduction_to_Molecular_Biology)

Align reads to a genome, assemble transcripts including novel splice variants, compute the abundance of these transcripts in each sample and compare experiments to identify differentially expressed genes and transcripts.

This protocol describes all the steps necessary to process a large set of raw sequencing reads and create lists of gene transcripts, expression levels, and differentially expressed genes and transcripts.

hisat2

hisat2-build

hisat2-inspect

hisat2, hisat2-align-s, hisat2-align-l, hisat2-build, hisat2-build-s, hisat2-build-l, hisat2-inspect, hisat2-inspect-s and hisat2-inspect-l.

I have several BED files with junctions extracted from BAM files via samtools, awk and bamToBed

I have a GTF file from which I load all exons and genes along with their genomic coordinates.

I am now attempting to extract the sequences of any genes (i.e. just the exons joined together) present in my sam file with the indels fixed. A GFF for the reference assembly is available that I can use.

such that the sequence AGAT would have four monomers (A, G, A, and T), three 2-mers (AG, GA, AT), two 3-mers (AGA and GAT) and one 4-mer (AGAT)

Sequence types:

секвенирование синтезом (sequencing-by-synthesis),

полупроводниковое секвенирование,

одномолекулярное секвенирование в реальном времени,

секвенирование при помощи нанопор,

NGS 2-nd generation - прочтения короткие (200-400bp) Shotgun Sequencing

NGS 3-nd generation - прочтения длинные (1000-200000bp)

NGS 2-nd generation:

Полупроводниковые секвенаторы - IonTorrent

sequencing-by-synthesis - Illumina (доминирует на рынке), HiSeq

Полупроводниковые секвенаторы:

- Микроскопический pH-метр. При синтезе нуклеотида в среду выходят ионы водорода и меняется pH. Мы регистрируем ток

- при добавлении нового нуклеотида измеряется ток

- NTP, добавляются по очереди.

Еще раз - нуклеотиды добавляются попеременно, мы знаем какой нуклеотид в данный момент добавлен, если pH не изменился значит синтез не произошел и наоборот.

Проблема этого метода при синтезе более 2 одинаковых нуклеотидов трудно определить какое количество нуклеотидов синтезировалось (AAAAAAAAAAA - такое событие тяжело определить pH метру)

sequencing-by-synthesis:

4 меченых нуклеотида добавляются одновременно

Добавление F-NTP останавливает синтез, тк к OH группе присоединен флуоресцентный белок

Делаем Imaging

Добавлеям реактив, который освобождает OH группу для последующего синтеза

Sequence Protocol:

Подготовить библиотеку(выделить ДНК/РНК, нарезка/фрагментация, лигация адаптеров, ПЦР)

1. Выделить ДНК/РНК:

- Клетки лизируются (разрушается мембрана, при помощи мыла т.к. Мембрана состоит из жиров(липофильна))
- Центрифугирование для отделения твердой фазы
- Экстракция - осаждение этанолом/хлороформ/Тризол/мини-колонка(Qiagen DNeasy Blood & Tissue Kit)
- Выделение РНК контролируется RNA Integrity Number

1. нарезка/фрагментация

- ДНК - ультразвук, ДНКаза или другой рестриктазой, транспозазой(дробит и одновременно пришивает нужные адаптеры)
- РНК - нагревание, соли цинка и магния
- Проблемы - легко перебросить ДНК, сделать слишком короткие фрагменты
- Bioanalyzer - прибор для определения размеров фрагментов.

Приготовление NGS-библиотеки включает в себя четыре основные стадии: 1) выделение ДНК или РНК; 2) фрагментацию ДНК или РНК; 3) лигирование адаптеров; 4) амплификацию при помощи ПЦР.

Фрагментация РНК и ДНК делается разными способами. В случае с РНК это соли двухвалентных металлов (цинк, магний) и нагревание. Для фрагментации ДНК чаще всего используют соникацию или специально разработанные ферменты на основе транспозаз. Последние одноверменно с фрагментацией также присоединяют адаптеры.

РНК намного менее стабильна, чем ДНК. Работа с РНК требует особого контроля качества исходного материала. Для оценки степени разложения РНК используют RIN - RNA Integrity Number, который позволяет универсально оценить степерь разложения РНК по соотношению пиков рибосомных РНК - 18S и 28S.

Для последующего секвенирования к каждому фрагменту из NGS-библиотеки необходимо добавить несколько "служебных" последовательностей: бар-код для локализации фрагмента на подложке, универсальный адаптер для ПЦР, а также индексную последовательность (в случае, когда одновременно производится секвенирование смеси нескольких образцов).

Y-образные адаптеры используются Illumina для получения фрагментов, пригодных для секвенирования фрагмента с обеих сторон (парно-концевые прочтения).

Аннотация геномов бывает структурная и функциональная;

Для структурной аннотации геномов используют предикторы генов, а также информацию по экспрессии (РНК-сек и другую), и информацию по протеиновым последовательностям.

Аннотация модельных организмов сделана довольно тщательно; обычно, существует несколько консорциумов или институтов, занимающихся аннотацией такого генома. Существует 4 основные аннотации генома человека (RefSeq, Ensembl, UCSC, Gencode), из которых самой современной является Gencode.

Аннотация может быть представлена в нескольких форматах данных - таких, как длинный 12-колоночный BED, а также GTF/GFF2, и GFF3.

Аннотации генома человека - CCDS/USCS/Ensembl/RefSeq/Gencode/AceView

1. Исходные данные – результаты секвенирования, каждому результату соответствует файл BAM.

2. Для дальнейшего анализа требуется для каждого файла BAM создать файл индекса (BAI).

3. При анализе данных требуется файл с референсным геномом (общий для всех образцов). Для того, чтобы можно было работать с ним, требуется создать файл с индексом референса (FAIDX) и словарем contig (DICT).

4. Для файла с данными генерируется файл с генетическими вариантами. Для этого используются программы, которые называются variant callers. В рамках одного пайплайна может использоваться несколько variant caller'ов.

5. После получения файлов с генетическими вариантами необходимо сравнить между собой варианты, полученные с использованием разных variant caller'ов. Процесс сравнения выполняется программой vcf-isec, результат не зависит от порядка, в котором были взяты файлы, файлы сравниваются попарно. сравнивать между собой нужно не все VCF, а только полученные из одного BAM.

6. variant caller принимает на вход только один файл с данными, сравнение variant caller'ов нужно провести для каждого из файлов с результатами секвенирования

SNP genotyping

SNV calling from NGS data refers to a range of methods for identifying the existence of single nucleotide variants (SNVs) from the results of next generation sequencing (NGS) experiments

Comments:

На сайте molbiol.ru есть несколько протоколов выделения ДНК  ([http://molbiol.ru/protocol/14_04.html](http://molbiol.ru/protocol/14_04.html)﻿)

Квиадженовские колонки это конечно хорошо, но уверен, что их с успехом можно заменить битым стеклом. Ну в 21 веке бить стекло не обязательно, а купить у Сигмы мелкодисперсный SiO2. Одной банки хватит лет на 100.

Тризол- гуанидин тиоционат/фенол/хроформ  - ацкая штука сама по себе разрушающая клеточные мембраны.

у нас та же история с Qiagen RNAeasy blood and Tissue Kit - из дрозофил выделяется меньше РНК, и присутствует загрязнение на 230нМ, по сравнению с Тризолом. Но для РНК-секвенирования почему-то упорно рекомендуют именно колоночные методы, и особенно Qiagen...

Но я уверен, что, допустим, набор Qiagen DNeasy Blood & Tissue Kit ﻿должен прекрасно подойти для Ваших целей. Тем более, что есть оптимизированная инструкция - ﻿ ﻿[https://www.qiagen.com/us/resources/download.aspx?id=cabd47a4-cb5a-4327-b10d-d90b8542421e&lang=e](https://www.qiagen.com/us/resources/download.aspx?id=cabd47a4-cb5a-4327-b10d-d90b8542421e&lang=e)...﻿

﻿Да, возможно выход будет меньше, чем при использовании метода фенол-хлороформной экстракции, зато сам препарат будет почище, и Вам не придётся работать с вредными веществами. В любом случае, если Вы будете что-то секвенировать или амплифицировать, то Вам всего-то и надо, что несколько нг ДНК.

Bridge Amplification(парные прочтения) - позволяет сделать прочтение фрагмента с двух сторон

Высокая точность 0.1%

Все прочтения получаются одинаковыми по длине - это особенность прибора - тогда секвенируется адаптер, лигированный с другого конца,  или запишется NNN. Все прочтения получаются одинаковыми по длине, т.к. делается абсолютно одинаковое (заданное оператором) количество циклов "добавление комплементарного нуклеотида - измерение флуоресценции - снятие защитной группы".

NGS 3-nd generation:

- Oxford Nanopore

- PacBio SMRT single-molecule real time

Nanopore

Природный протеин-нанопора - Alpha-Hemolysin

Motor protein - upload dna to nanopore

Ток через пору пропорционален размеру проходящего нуклеотида

PacBio SMRT single-molecule real time

Read 15k

14% ошибок

Всего 37 миллионов bp or 37 Gbp

Всего 3 типа применения NGS

1. Секвенирование генов (DeNovo, ресеквенирование(уточнение последовательности))

2. Секвенирование транскриптов (DeNova, аннотирование генов)

3. Количественные методы (определение конформации ДНК и позиции протеинов на ДНК)

4. Неизученные виды сборка De Novo

Coverage - 70% coverage at 5x depth - 70% генома покрыты хотя бы пятерным слоем прочтений

Количественные методы -

Транскриптом RNA-seq

Позиционный ChIP-seq, genome foot printing

По кол-ву выравниваний на референсный геном можно судить об изобилии транскриптов

Секвенирование можно проводить для организмов с неизвестным геномом или транскриптомом - в этом случае говорят о секвенировании генома/транскриптома de novo.

Секвенирование также можно проводить с целью уточнить последовательность уже собранного генома или транскриптома - в таком случае говорят о ресеквенировании.

Секвенирование можно производить с целью уточнить последовательность каких-либо фрагментов, а можно - с целью подсчитать количество прочтений, выравнивающихся на определенные участки генома или транскриптома.

Контиг - участок генома или транскрипта, покрытый чтениями без пробелов.

Скаффолд - набор упорядочных, ориентированных контигов

Полиплолидность - много копий генома. Свойственно растениям

Меры качества сборки denovo генома

- N50 - мера средней длины контига

- mis-assemblies - кол-во ошибок в сборке

- NG50 - ассчитывается так же, как N50, за исключением того, что суммируемые скаффолды или контиги должны покрывать более 50% реальной (или оценочной) длины генома.

N50 - Scaffold/contig length at which you have covered 50℅ of total assembly length

NG50 - Scaffold/contig length at which you have covered 50℅ of total genome length

N50/L50 - бессмысленны для транскриптома

критерием качества сборки транскриптома - Количество правильно собранных "типичных генов"

Гены домашнего хозяйства транскрибирующая в любой клетке

Программы - busco,

Какие из нижеприведенных причин могут подтолкнуть исследователя к сборке транскриптома без сборки генома? - Необходимость биологического сравнения нескольких тканей, Большой размер генома, Высокая плоидность генома

Геномы секвенируются методом "дробовика". Сборка геномов производится в последовательности "прочтения -> контиги -> скаффолды".

Контиг - участок генома или транскрипта, покрытый прочтениями без пробелов. Скаффолд - набор упорядоченных, ориентированных контигов.

Для уточнения положения контигов в скаффолдах широко используются библиотеки mate pair. В них при помощи лигации, фрагментации, и последующего секвенирования определяется взаимное расположение участков, отстоящих друг от друга на значительном расстоянии (1-10 тыс.п.н.)

Транскриптомы часто собирают de novo в тех случаях, когда геном слишком сложен для эффективной сборки: содержит очень много повторов, имеет высокую плоидность или очень большой размер.

Для сборки геномов применяются метрики N50 и L50. N50 - это длина контига, который, вместе со всеми контигами большей длины, присутствующими в сборке, покрывает 50% генома или более. L50 - это минимальное количество контигов из сборки, которые в сумме покрывают 50% генома или более. Метрики N50 и L50 бессмысленны для сборки транскриптома.

Оптимизация методов сборки с целью увеличения N50 часто приводит к увеличению количества ошибок в сборке (mis-assemblies). Последние представляют из себя значительную проблему при аннотации генома.

Как геномы, так и транскриптомы можно оценивать по количеству генов, которые заведомо должны присутствовать в геноме/транскриптоме. Проверочные наборы генов меняются в зависимости от типа исследуемого организма. Оценку можно проводить при помощи программ CEGMA и BUSCO.

/--------Мутации----------/

Silent/Nonsense/Missense - классификация мутация по значимости

Silent при изменении ДНК последовательности аминокислота осталась та же и транслируемый белок не поменялся

Nonsense получился стоп кодон, трансляция белка прерывается. Белок не функционален

Missense - при изменении ДНК последовательности меняется аминокислота, но неизвестно из какой группы будет транслировать новая аминокислота. полярная/неполярная и т.д. Это может нарушить структуру белка.

Indel - insert/deletaion

Рецессивное наследование - нужно две копии битого гена для развития заболевания

Доминантное наследование - нужна одна копия битого гена для развития заболевания

/--------Методы Ресеквенирование----------/

Ампликон - подбор праймеров к генам кандидатам и проводим ПЦР

16S секвенирование - помогает идентифицировать вид

Экзомы - биотин-стрептавидин обогащение, для выделения exon?

OhmyGut, Atlas, Knomics

10X Genomics

Вариации в геноме можно формально разделить на три большие группы - однонуклеотидные замены (single-nucleotide polymorphisms, SNP, снипы), инделы (вставки и делеции размером до 1000 п.н.), и структурные варианты (вставки и делеции размером более 1000 п.н., а также сбалансированные транслокации, инверсии, и т.д.)

Замены нуклеотидов в кодирующей последовательности ДНК могут иметь различные последствия для аминокислот в конечном протеине: не изменять их (синонимичная замена), изменять на другую аминокислоту (миссенс-замена), либо сигнализировать преждевременную остановку синтеза протеина (нонсенс-замена).

Методы ресеквенирования чаще всего применяют в медицинской генетике для исследования вариации генома человека. Часто эти методы применяются для диагностики наследственных моногенных (Менделевских) заболеваний.

Секвенирование ампликонов и таргетных панелей часто применяется для изучения последовательности нескольких генов, или небольших участков генома (например, рибосомной последовательности 16S у бактерий). Для этого библиотеки амплифицируются (обогащаются) нужными участками при помощи ПЦР с заранее подобранными праймерами, ограничивающими интересующие нас участки. Дальнейшее секвенирование позволяет получить высокое покрытие целевых регионов при относительно невысоком объеме секвенирования.

Секвенирование экзомов и больших генных панелей применяется для изучения вариации в большом наборе генов. Полный экзом позволяет найти варианты во всей протеин-кодирующей части генома человека. Для амплификации нужных участков генома используется биотин-стрептавидиновое обогащение.

Структурные варианты очень сложно находить при помощи коротких прочтений. Полногеномное секвенирование при помощи NGS 2-го поколения является минимальным экспериментом, позволяющим находить такие варианты с достаточной чувствительностью. Оптимальными для нахождения структурных вариантов являются методы NGS 3-го поколения и специальные методы - такие, как 10X Genomics.

/------------DNase-Seq, MNase-Seq, и ATAC-Seq-----------/

Помогают анализировать открытые участки ДНК

/-----NGS-------/

Вариант кодировки качества прочтения fastaq

Sanger = Phred + 33

Solexa = Solexa+64

Illumina = Phred + 64

GZ сжимает fastaq в 5-10 раз

SRA - Sequence Read Archive, формат данных для SRA DB. Декодирует командой

fastaq-dump --split-3 file.sra

Индекс Fasta

samtools faidx genome.fa # индексирует геном на хромосомы

5 Стобцы:

Название хромосомы | длина хромосомы | сдвиг(начало хромосомы) | длина строки | кол-во битов на последовательность

Геномные интервалы - экзоны, промоторы, энхансеры, промоторы, повторы

===Аннотация===

Структурная аннотация Чаще

- Поиск повторов

- Поиск протеин-кодирующих генов

- Идентификация кодирующих последовательностей

- Поиск псевдогенов и некодирующих РНК

Функциональная аннотация Редко

- Какова роль генов и регуляторах элементов

1. Идентификация повторов RepeatMasker softmask (AA -> aa), hardmask (AA -> NN)

FASTA + BED file with repeats -> mask repeats

Псевдогены - неработающие гены. Процессированные, непроцессированные, унитарные.

Гены могут располагаться на разных нитях ДНК

Известно, что разные организмы, как правило, генетически отличаются друг от друга (в том случае, если они не являются клонами). Генетические отличия между организмами традиционно назывались мутациями, сейчас более принятым является термин генетический вариант. Для того, чтобы эффективно оперировать отличиями, удобно приводить отличия от некоторого стандарта, в качестве такого стандарта используется референсный геном – последовательность ДНК наилучшим образом представляющая генотип некоторого вида. Например, результатом проекта "Геном человека" стал референсный геном человека.

Процесс генотипирования, представляющий собой поиск отличий между геномом анализируемого организма и референсного генома, лежит в основе генетической диагностики. Одним из наиболее перспективных методов генотипирования является высокопроизводительное секвенирование (MPS, также используется термин NGS). Отличительной чертой этой технологии являются большие объемы данных и сложные схемы анализа, что сделало MPS полигоном для отработки инструментария для создания пайплайнов анализа данных.

Другой особенностью MPS является использование большого количества инструментов для анализа, которые могут давать сильно отличающиеся результаты при работе на одном и том же наборе данных. Мы попробуем сравнить эффективность нескольких инструментов, предназначенных для поиска генетических вариантов (variant callers) и создать пайплайн, который будет автоматически проводить такое сравнение с использованием некоторого набора образцов.

Общая логика работы пайплайна такова:

Референсный геном подготавливается для анализа

С использованием референсного генома и файлов с данными проводится поиск генетических вариантов.  Для поиска генетических вариантов используется три различных инструмента: freebayes, samtools и HaplotypeCaller.

Производится подсчет генетических вариантов в файлах, полученных каждым из инструментов.

Производится поиск вариантов, общих для файлов, полученных каждым из инструментов.

Создается файл с отчетом (см. ниже)

Goals:

- Differential Expression

- Alternative Splicing

- Low level expression

- Absolute Quantitation

- Novel genes

- Isoform expression

/------Illumina------/

Illumina paired-end sequencing

/------PacBio SMRT DNA sequencing------/

Single Molecule Real Time

ZMV - Zero Mode Waveguide

DNA-polimerase and Phospholinked Nucleotides.

DNA-polimerase attached to the bottom of plate/wall.

During DNA replication DNA polymerase attach nucleotides with Phospholinked molecule.

When nucleotide attaches to chain Phospholinked molecule detaches and emmits a light.

ZMV catches emission.

Problems:

Production rate of DNA polymerase is 1000bp/s, machine can't register light emission so fast.

Solution: make DNA circle and make few circles of measurements we will have overlaps and we can combine all measurements.

/------RNA-Seq------/

High throughput sequencing tells us which genes are active, and how much they transcribed.

RNA-seq to measure gene expression in normal cells and mutated cells and compare the difference.

3 Main steps for RNA-seq

1. Prepare sequence library

2. Sequence (sequence-by-synthesis(Illumina, PacBio))

3. Data Analysis

/-Prepare sequence library(Illumina protocol)-/

1. Isolate RNA

2. Break RNA into small fragments(200-300bp)

3. Convert RNA fragments cDNA(double stranded)(because DNA more stable and easy to modify and amplify)

4. Add sequencing adaptors(A,B) to both ends(Allow seq.machine to recognize the fragments)

5. PCR amplify the library

6. Quality control(Verify library concentration, verify library fragment length)

/-Sequence-/

1. Glass slide has attached complimentary sequences to A and B adaptors(many-many oligos of 2 types)

2. cDNA attaches to sequence witch complimentary to adapter. DNA-polimerase create compliment sequence.

3. Denaturation. Original cDNA wash away.

4. New sequence strand able to fold and attach to another oligos. DNA-polimerase create double strand.

5. Denaturation again, but for amplification(bridge amplification). Process repeats from step 4 again many times

6. After amplification reverse strand wash away. Sequences attaches to one type of oligos

7. Sequence  process - fluorescent nucleotide attaches and emit a light.

Bulk RNA-Seq

Single Cell RNA-Seq

/-Data Analysis-/

Step#1

1. Plot data 20.000 genes

2. Apply PCA

3. Find witch data we can remove from analysis

Step#2

1. Identify differently expressed genes between the normal and mutant samples. Use (logFC and logCPM graph).

Wt and ko samples

/--CHIP-Seq(chromatin immunoprecipitation combined with high-throughput sequencing)------/

It identifies the locations(regions) in the genome bound by proteins.

Chromosomes - are made out of chromatin

chromatin - is made out DNA + histones(protein)

DNA wraps around the histones to package DNA, and packaging can regulate gene transcription.

Protocol:

1. Use formaldehyde to glue all of the proteins bound to the DNA together with the DNA.

2. Cut DNA to small seq(300 bp)

3. Isolate proteins we're interested in by using antibody.

4. Unglue and wash proteins away, only DNA remains.

5. Use RNA-seq protocol from step 5.

/------NGS------/

There are two major paradigms in next-generation sequencing (NGS) technology: short-read sequencing and long-read sequencing. Short-read sequencing approaches provide lower-cost, higher-accuracy data that are useful for population-level research and clinical variant discovery. By contrast, long-read approaches provide read lengths that are well suited for de novo genome assembly applications and full-length isoform sequencing.

Short-read sequencing approaches fall under two broad categories: sequencing by ligation (SBL) and sequencing by synthesis (SBS)

/------CLIP-seq------/

CLIP-seq (Cross-Linking Immunoprecipitation combined with high-throughput sequencing) is a transcriptomics method to study RNA directly bound to RNA-binding proteins across the entire transcriptome in cell or tissue samples. It is a powerful technique to analyze protein interactions with RNA, map RNA binding protein binding sites, or to identify RNA modifications sites of interest. CLIP-seq software tools are used for data pre-processing and processing, as well as data analysis and visualization.

/----------------/

RNA-seq

An overview of the ‘new Tuxedo’ protocol. In an experiment involving multiple RNA-seq data sets, reads are first mapped to the genome using HISAT (Steps 1 and 2). Annotation of reference genes and transcripts can be provided as input, but this is optional, as indicated by the dotted line. The alignments are then passed to StringTie (Step 3), which assembles and quantifies the transcripts in each sample. (In the alternative protocol, the alignments from Step 2 are passed directly to Step 6, skipping all assembly steps. Step 6 will then estimate abundance only for known, annotated transcripts.) After initial assembly, the assembled transcripts

are merged together (Step 4) by a special StringTie module, which creates

a uniform set of transcripts for all samples. StringTie can use annotation in both of these steps, as shown by the dotted lines. The gffcompare program then compares the genes and transcripts with the annotation and reports statistics on this comparison (Step 5). In Step 6, StringTie processes

the read alignments and either the merged transcripts or the reference annotation (through the diamond labeled ‘OR’). Using this input, StringTie re-estimates abundances where necessary and creates new transcript

tables for input to Ballgown. Ballgown then compares all transcripts across conditions and produces tables and plots of differentially expressed genes and transcripts (Steps 7–21). Black and curved blue lines in the figure represent input to and output from the programs, respectively. Optional inputs are represented by dotted lines.

NSFC

Companies

23Mofang

3D Medicines Corporation

Ablife Inc.

Accuragen Inc.

Adicon Clinical Laboratories

Adicon Clinical Laboratories, Inc.

Allwegene

Anchordx

Annoroad Gene Technology

Basecare

Basetra Shanghai

Benagen

Benegene

Berry Genomics Co. Ltd.

BGI

Biogenius

Biological Company Division, Beijing Computing Center

Biomarker

Bionovogene

Bioon

Biorefer

Biosan

Biotecan

Biotechnology Shanghai

Biotree

Boheng

Burning Rock

Capitalbio Technology

Capitalgenomics Co. Ltd.

Cloudhealth Medical Group Ltd.

Cyagen

Cygnus

Daan Gene

Darui Biotechnology Co. Ltd.

Dian Diagnostics

Direct Genomics

Emei Tongde

Ezlife

Find Bio-Tech

Forever Gen

Frasergen

Gemple

Gene Denovo

Genecast

Genedock

Genekang

Geneocean

Genergy Shanghai

Geneseeq

Genesky

Genetron

Genewiz

Gennlife

Gminix

Hangzhou G-Bio Biotech Co. Ltd.

Hangzhou Heyi Gene Technology Co. Ltd.

Hangzhou Lianchuan Bio Technologies Co. Ltd.

Hangzhou Mitian Gene Technology Co. Ltd.

Hangzhou Ptm Biolabs Co. Inc.

Hangzhou Ruichuang Biotechnology Co. Ltd.

Hangzhou Shengting Biotech Co. Ltd.

Hanyu

Haplox

Hikewell

HKGI

Honor Tech

Huaniu Biotechnology

Huasheng Hengye

Huayigenomics

Hyk Gene Technology Co. Ltd.

IGE

Igenetech

IMH Bio

Imuno Biotech Co. Ltd.

IPE

Jabrehoo

Jiayin

Kangchen Bio-Tech

Kindstar

Kingmed Diagnostics Center Co. Ltd.

Linked Biotech

Logic Informatics Co. Ltd.

Lucidus

Macrogen

Magigen

Majorbio

Microbiota

Microhelix Gene

Microread

Mygenostics

New Horizon Health

Nextomics

Novelbio

Novogene Bioinformatics Technology Co. Ltd.

Oebiotech

Offo

Origene China

Paihang

Quanmai

Quanti Health

Qy Node

Reader Bioinformatics

Realgene

Rhonin

Ribobio

Sangon Biotech

Sanvalley

Scisoon

Sensichip

Shanghai Bioasia Gene Co. Ltd.

Shanghai Personal Biotechnology Co. Ltd.

Shanghai Yunying Medical Technology Co. Ltd.

Sinogenomax

Sinomdgene

Sinopath

Solomen Brothers

South Gene

Superbio

Suzshou Biomedical Research And Development Centers

Tgs

Tinygene

Top Gene

Tri-I

Unimed Diagnostic Technology Ltd.

Vishuos

Wuxi Apptec

Xinpeijing

Xybiotech

Yikon Genomics Co. Ltd.

Yingbio

Yucebio Co. Ltd.

Zhejiang Tianke Hi-Technology Development Co. Ltd.

Zi Bio Tech

Zixin Pharmaceutical Industrial Co. Ltd.