# CODX
--

codx is a python package that allow retrieval of exons data from NCBI RefSeqGene database.

## Installation

```bash
pip install codx
```

## Usage

```python
# Import the create_db function to create a sqlite3 database with gene and exon data from NCBI
from codx.components import create_db


# 120892 is the gene id for LRRK2 gene
db = create_db(["120892"])

# From the database object, you can retrieve a gene object using its gene name
gene = db.get_gene("LRRK2")

# From the gene objects you can retrieve exons data from the blocks attribute each exon object has its start and end location as well as the associated sequence
for exon in gene.blocks:
    print(exon.start, exon.end, exon.sequence)

# Using the gene object it is also possible to create all possible ordered combinations of exons
# This will be a generator object that yield a SeqRecord object for each combination
# There however may be a lot of combinations so depending on the gene, you may not want to use this with a very large gene unless there are no other options
for exon_combination in gene.shuffle_blocks():
    print(exon_combination)

# To create six frame translation of any sequence, you can use the three_frame_translation function twice, one with and one without the reverse complement option enable
# Each output is a dictionary with the translatable sequence as value and the frame as key
from codx.components import three_frame_translation
for exon_combination in gene.shuffle_blocks():
    three_frame = three_frame_translation(exon_combination.seq, only_start_at_atg=True)
    three_frame_complement = three_frame_translation(exon_combination.seq, only_start_at_atg=True, reverse_complement=True)

```
