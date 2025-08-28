# SwissLipidsReact

Archived repository, the active version can be found at [SIB github](https://github.com/sib-swiss/swisslipidsreact)

Expands Rhea reaction patterns into complete lipid reactions, resolving structures and assigning RInChIs.

[![License](https://img.shields.io/github/license/sib-swiss/pyrheadb)](LICENSE)
![OS Linux](https://img.shields.io/badge/OS-Linux-green)
![OS Windows](https://img.shields.io/badge/OS-Windows-blue)
![OS macOS](https://img.shields.io/badge/OS-macOS-lightgrey)

## Description
This code combines [Rhea](https://www.rhea-db.org) database of biochemical reactiona and [SwissLipids](https://www.swisslipids.org/#/) database of lipid structures to enumerate the hypothetically possible space of biochemical reactions with lipid structures fully resolved.

The subset of Rhea reactions that define the lipid reaction mechanisms are represented using [ChEBI](https://www.ebi.ac.uk/chebi/) idenitfiers of reacting lipid classes in the Rhea database.

SwissLipids provides connection between the lipid class - hypothetical entity aiming to represent many lipids present in nature that share a particular substructure - and all of the hypothetically possible lipid structures with isomeric subspecies level of compound structure definition, i.e. 2.5D structure definition, allowing to recognise precisely atom composition and bond order, as well as stereochemical tags of the atoms of every molecule.

This code transforms each Rhea reaction defined in terms of lipid classes into a set of reactions with each reactant and product having their 2.5D structures defined, and checks the correspondance between reactant and products to ensure that the resulting reactions are atomically balanced and biochemically feasible.

## Data
It is necessary to download lipids.tsv from [SwissLipids](https://www.swisslipids.org/#/downloads).

lipids.tsv (~700MB) should be located in src/swisslipidsreact/package_data before starting the execution.

## Installation
```bash
pip install .
```

## pyrheadb dependency
This package is dependent on [pyrheadb](https://github.com/sib-swiss/pyrheadb/tree/main).

To avoid downloading and preprocessing full Rhea reaction data for every potential new execution, follow [instructions](https://github.com/sib-swiss/pyrheadb/wiki) on how to set up the RHEADB_LOC environment variable.

## Run
```bash
# Run enumeration
swisslipidsreact run

# Export .ttl (turtle) format for integration into the RDF knowledge graph.
swisslipidsreact export-ttl

# rhea reaction template usage analysis
swisslipidsreact master-id-analysis
```
## Options

* Fatty acid options
  
Option | Meaning | Time | Usage |
--- | --- | --- | --- |
Curated FA | Filter SwissLipids based on allowed FA per position | hours | considered full enumerated set |
Palmitate only | Only palmitate allowed as a fatty acid in any position | minutes | default, C16, test set, control execution and output |
All FA  | all SwissLipids considered | ∞ | not recommended for all Rhea IDs, can be used for individual Rhea ID |

```bash
# run options

"--output-dir",
type=str,
default=None,
help="Output directory (default: current working directory)"

"--curated-fa",
action="store_true",
help="Use curated fatty acid list (default: False for C16)"

"--all-fa",
action="store_true",
default=False,
help="No restrictions of FA per position"

"--rheaid",
type=int,
default=None,
help="run pipeline for only one rhea id"

# ttl export options

"--curated-fa",
action="store_true",
help="Use curated fatty acid list for TTL export (default: False for C16)"

"--input",
type=str,
default=None,
help="Input TSV file (default: inferred from mode)"

```

## Usage
By default, the pipeline will generate results for palmitate only.
To generate results for the whole list of fatty acids in human and enumerated classes, set --curated-fa option.

To learn more about the options, check swisslipidsreact --help.

* run pipeline for C16:
```bash
swisslipidsreact run --output-dir results_C16/
```

* run pipeline for curated list of fatty acids (execution time: several hours):
```bash
swisslipidsreact run --curated-fa --output-dir results_curated_fatty_acids/
```

* run pipeline for all fatty acids (execution time: ∞):
```bash
swisslipidsreact run --all-fa --output-dir results_all_fatty_acids/
```

* run pipeline for all fatty acids per rhea id (execution time: ∞):
```bash
swisslipidsreact run --all-fa --output-dir results_78071/ --rheaid 78071
```

* turtle (RDF type) export for palmitate:
```bash
swisslipidsreact export-ttl --output-dir results_C16/
```
* turtle (RDF type) export for curated list of fatty acids (execution time: several hours):
```bash
swisslipidsreact export-ttl --curated-fa --output-dir results_curated_fatty_acids/
```

* analyse the rhea reaction master id usage
```bash
swisslipidsreact master-id-analysis --input "results_merged/merged_enumerated_reactions.tsv" --all-fa
```
