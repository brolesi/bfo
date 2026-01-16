# Brazilian Financial Ontology (BFO)

A formal ontology for the Brazilian financial system, developed in OWL/RDF, aiming to provide semantic interoperability, alignment with international standards (FIBO), and integration with Brazilian standards (Open Finance Brasil, BCB).

## Features
- Semantic interoperability for financial systems
- Alignment with FIBO (Financial Industry Business Ontology)
- Integration with Open Finance Brasil and Banco Central do Brasil (BCB)
- Core ontology and mapping modules
- Example instances for demonstration

## Repository Structure
```
bfo/
├── ontology/
│   ├── bfo-core.owl                # Core ontology
│   ├── bfo-openfinance-mapping.owl # Open Finance Brasil mapping
│   └── bfo-bcb-mapping.owl         # BCB/SGS mapping
├── examples/
│   └── bfo-examples.owl            # Example instances
└── docs/
    └── README.md                   # Documentation (detailed)
```

## Documentation
See [docs/README.md](docs/README.md) for a detailed description of classes, properties, mappings, and usage examples.

## Citation
If you use BFO in your work, please cite as follows:

```bibtex
@software{bfo2026,
  author       = {Brolesi, F. F.},
  title        = {{Brazilian Financial Ontology (BFO): Uma Ontologia Formal do Sistema Financeiro Brasileiro}},
  year         = {2026},
  publisher    = {GitHub},
  url          = {https://github.com/brolesi/bfo},
  version      = {1.0.0},
  note         = {Ontologia OWL/RDF alinhada com FIBO e integrada ao Open Finance Brasil e BCB}
}
```

## License
Creative Commons Attribution 4.0 International (CC BY 4.0)

## Contributing
Contributions are welcome! Please open issues or pull requests.
