# Guessing ENSEMBL gene ID from a gene symbol

Guess the most probable ENSEMBL gene ID given your human/mouse gene symbol.

## How to run

This script is useful when updating a list of **human or mouse** gene symbols into a common reference. In order to run it, you need your query list as a plain text file. Clone this repository, copy your list into the main directory, and run

```bash 
gzip -d human.ensgen_ref.tsv.gz
./symbol_to_ensg <your_symbol_list> human.ensgen_ref.tsv Gencode-v38
```

The target, *Gencode-v38*, can be replaced with any reference of your choosing. The reference collection has Ensembl 70-105 and Gencode v19-v39. Other references can be added to the text file.

About 2-4 Gb RAM is needed. Sorry, the script was not optimized in any meaningful way. It's wicked fast though ;)

## Output 

The plain-text, tab-separated output will be written to *STDOUT*; it will contain 5 columns: 

| Original symbol | Reliable ENSG | Other ENSG | Category | New symbol | 
|:-:|:-:|:-:|:-:|:-:|
| AP006222.2 | NONE | ENSG00000286448,ENSG00000228463 | Unresolved | NONE |
| RP4-669L17.10 | ENSG00000237094 | NONE | Unique | RP4-669L17.4 |
| RP11-206L10.9 | ENSG00000237491 | NONE | Unique | LINC01409 |
| LINC00115 | ENSG00000225880 | NONE | Unique | LINC00115 |
| RNU11 | NONE | ENSG00000270103,ENSG00000274978 | Multi | NONE |
| SCARNA2 | NONE | ENSG00000278249,ENSG00000270066 | Multi | NONE |

First column contains original symbol; second, reliably inferred Ensembl ID; third, other Ensembl IDs associated with this symbol; fifth, the gene symbol updated according to *Gencode v38* (or other reference of your choosing). 

Fourth column contains a category to which the gene symbol belongs: 

  - **Unknown** - never seen in any of the references;
  - **Multi** - gene name matching multiple ENSG entities in the most updated Gencode/Ensembl reference; 
  - **Removed** - gene name does not exist in the newest Ref, and associated ENSG are absent in the newest Ref; 
  - **Unresolved** - gene name does not exist in the newest Ref, and multiple associated ENSGs are present in the newest Ref; 
  - **Unique** - uniqly matching one and only one ENSG; 
  - **Updated** - gene name exists in the newest Ref, but associated ENSG has changed, and previous ENSGs are retired; 
  - **Renamed** - gene name does not exist in the newest Ref, but associated ENSG is present in one copy in the newest Ref; 
  - **Ensembl** - gene name is actually an Ensembl ID. 

Of these, first four lead to gene "loss" (no reliable ENSG could be inferred), while last four give you a reliable guess as to what ENSG matches your gene symbol best. 

## Author 

Alexander V. Predeus, Wellcome Sanger Institute, 2022 
