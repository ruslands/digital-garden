FASTA & FASTQ analysis pipeline

[https://amplicon-sequencing-pipeline.readthedocs.io/en/latest/preparing-dataset.html](https://amplicon-sequencing-pipeline.readthedocs.io/en/latest/preparing-dataset.html)

[https://omicstutorials.com/mastering-bioinformatics-analysis-with-fasta-sequences-a-biologists-guide-to-unix-and-linux/](https://omicstutorials.com/mastering-bioinformatics-analysis-with-fasta-sequences-a-biologists-guide-to-unix-and-linux/)

Statistics & Overview Step

Count sequences in the file

awk '/^>/{count++} END{print "Number of sequences: " count}' your_sequence_file.fasta

Calculate average sequence length

awk '/^>/{total += length(sequence); count++} {sequence = $0} END{print "Average sequence length: " total / count}' your_sequence_file.fasta

Minimum and maximum sequence length

GC content

Quality score distribution

Filtering by sequence length

Filtering by GC content