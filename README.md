# Brazilian Financial Ontology (BFO)

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](CHANGELOG.md)
[![OWL 2](https://img.shields.io/badge/OWL-2_DL-orange.svg)](https://www.w3.org/TR/owl2-overview/)

A formal ontology of the Brazilian financial system, developed in OWL/RDF,
providing semantic interoperability, alignment with international standards
(FIBO — Financial Industry Business Ontology), and integration with Brazilian
standards (Open Finance Brasil, Banco Central do Brasil).

> **Nota**: não confundir com a *Basic Formal Ontology*, que compartilha a
> sigla BFO.

## Features

- **Semantic interoperability** for Brazilian financial systems
- **FIBO alignment** (`rdfs:subClassOf` / `owl:equivalentClass` references to
  FND, BE, FBC, and SEC modules)
- **Open Finance Brasil integration** — customers, accounts, credit,
  payment initiation, investments, and **consent lifecycle** (LGPD /
  Resolução Conjunta CMN/BCB 1/2020)
- **BCB open data integration** — SGS time series (Selic, CDI, IPCA, PTAX,
  INPC…), IF.data, SCR, DICT
- **Up-to-date market coverage** — investment funds under Resolução CVM
  175/2022 (FIDC, FIP, multimercado…), virtual assets (Lei 14.478/2022),
  Drex (Brazilian CBDC), direct credit fintechs (SCD/SEP), guarantees,
  foreign exchange (Lei 14.286/2021)
- **Bilingual annotations** (pt-BR labels/definitions; en where applicable)
- Example instances and SPARQL queries for demonstration

## Repository Structure

```
bfo/
├── ontology/
│   ├── bfo-core.owl                  # Core ontology (classes, properties, individuals)
│   ├── bfo-openfinance-mapping.owl   # Open Finance Brasil mapping module
│   ├── bfo-bcb-mapping.owl           # BCB/SGS open data mapping module
│   └── catalog-v001.xml              # XML catalog for local resolution (Protégé)
├── examples/
│   └── bfo-examples.owl              # Example instances (A-Box)
├── docs/
│   └── README.md                     # Detailed documentation (pt-BR)
├── CHANGELOG.md                      # Version history (Keep a Changelog)
├── CITATION.cff                      # Citation metadata (GitHub-native)
├── CONTRIBUTING.md                   # Modeling conventions and workflow
└── LICENSE                           # CC BY 4.0
```

## Quick Start

### Protégé

Open `ontology/bfo-core.owl` in [Protégé](https://protege.stanford.edu/).
The `catalog-v001.xml` file resolves the BFO modules locally.

### Apache Jena (Java)

```java
Model model = RDFDataMgr.loadModel("ontology/bfo-core.owl");
```

### RDFLib (Python)

```python
from rdflib import Graph
g = Graph()
g.parse("ontology/bfo-core.owl", format="xml")
```

### SPARQL example — CDBs covered by FGC

```sparql
PREFIX bfo: <https://ontology.brasil.gov.br/bfo#>

SELECT ?cdb ?emissor ?taxa WHERE {
  ?cdb a bfo:CDB ;
       bfo:emitidoPor ?emissor ;
       bfo:taxaJuros ?taxa .
}
```

## Namespaces

| Prefix | URI |
|--------|-----|
| `bfo` | `https://ontology.brasil.gov.br/bfo#` |
| `ofb` | `https://openfinancebrasil.org.br/schema/` |
| `bcb` | `https://dadosabertos.bcb.gov.br/schema/` |
| `fibo-*` | `https://spec.edmcouncil.org/fibo/ontology/…` |

## Documentation

See [docs/README.md](docs/README.md) for a detailed description of classes,
properties, mappings, regulatory classifications, and usage examples.

## Versioning

The project follows [Semantic Versioning](https://semver.org/). Published
terms are never removed; superseded terms are marked with
`owl:deprecated true` (e.g., `bfo:DOC`, discontinued in 2024). See
[CHANGELOG.md](CHANGELOG.md).

## Citation

If you use BFO in your work, please cite it (see [CITATION.cff](CITATION.cff)):

```bibtex
@software{bfo2026,
  author       = {Brolesi, F. F.},
  title        = {{Brazilian Financial Ontology (BFO): Uma Ontologia Formal do Sistema Financeiro Brasileiro}},
  year         = {2026},
  publisher    = {GitHub},
  url          = {https://github.com/brolesi/bfo},
  version      = {1.1.0},
  note         = {Ontologia OWL/RDF alinhada com FIBO e integrada ao Open Finance Brasil e BCB}
}
```

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md)
for modeling conventions (IRI naming, mandatory annotations, OWL DL
constraints) and open an issue or pull request.

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)
