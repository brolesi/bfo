# Brazilian Financial Ontology (BFO)

[![Licença: CC BY 4.0](https://img.shields.io/badge/Licen%C3%A7a-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Versão](https://img.shields.io/badge/vers%C3%A3o-2.0.0-blue.svg)](CHANGELOG.md)
[![OWL 2](https://img.shields.io/badge/OWL-2_DL-orange.svg)](https://www.w3.org/TR/owl2-overview/)

Ontologia formal do sistema financeiro brasileiro, em OWL/RDF, para
interoperabilidade semântica, alinhamento com padrões internacionais
(FIBO — Financial Industry Business Ontology) e integração com os padrões
brasileiros (Open Finance Brasil, Banco Central do Brasil).

> **Nota**: não confundir com a *Basic Formal Ontology*, que compartilha a
> sigla BFO. É por isso que o namespace desta ontologia é `bfo-br`.

## O que tem aqui

- **Interoperabilidade semântica** para sistemas financeiros brasileiros
- **Alinhamento FIBO** — `rdfs:subClassOf` / `rdfs:subPropertyOf` para FND,
  BE, FBC, SEC, DER e LOAN, mais o OMG Commons. Cada IRI alvo é conferida
  contra um download real da FIBO no CI (ver [Validação](#validação)); os
  imports ficam no [módulo opt-in](#módulo-de-alinhamento-fibo)
- **Integração Open Finance Brasil** — clientes, contas, crédito, iniciação
  de pagamento, investimentos e **ciclo de vida do consentimento**
  (LGPD / Resolução Conjunta CMN/BCB 1/2020)
- **Integração com os dados abertos do BCB** — séries do SGS (Selic, CDI,
  IPCA, PTAX, INPC…), IF.data, SCR, DICT
- **Cobertura de mercado atualizada** — fundos sob a Resolução CVM 175/2022
  (FIDC, FIP, multimercado…), ativos virtuais (Lei 14.478/2022), Drex,
  fintechs de crédito direto (SCD/SEP), garantias, câmbio (Lei 14.286/2021)
- **Anotações bilíngues** (rótulos e definições em pt-BR; en onde aplicável)
- Instâncias de exemplo e consultas SPARQL de demonstração

## Estrutura do repositório

```
bfo/
├── ontology/
│   ├── bfo-core.owl                  # Núcleo (classes, propriedades, indivíduos)
│   ├── bfo-openfinance-mapping.owl   # Mapeamento Open Finance Brasil
│   ├── bfo-bcb-mapping.owl           # Mapeamento dados abertos BCB/SGS
│   ├── bfo-fibo-alignment.owl        # Módulo opt-in que importa a FIBO
│   └── catalog-v001.xml              # Catálogo XML para o Protégé
├── examples/
│   └── bfo-examples.owl              # Instâncias de exemplo (A-Box)
├── queries/                          # Consultas SPARQL verificadas pelo CI
├── w3id/                             # Registro do IRI persistente w3id.org
├── legacy/                           # Ontologia anterior, fora da distribuição
├── docs/
│   └── README.md                     # Documentação detalhada
├── CHANGELOG.md                      # Histórico de versões (Keep a Changelog)
├── CITATION.cff                      # Metadados de citação
├── CONTRIBUTING.md                   # Convenções de modelagem e fluxo de trabalho
└── LICENSE                           # CC BY 4.0
```

## Como começar

### Protégé

Abra `ontology/bfo-core.owl` no [Protégé](https://protege.stanford.edu/).
O `catalog-v001.xml` resolve os módulos localmente.

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

### Exemplo SPARQL — CDBs com cobertura do FGC

```sparql
PREFIX bfo: <https://w3id.org/bfo-br#>

SELECT ?cdb ?emissor ?taxa WHERE {
  ?cdb a bfo:CDB ;
       bfo:emitidoPor ?emissor ;
       bfo:taxaJuros ?taxa .
}
```

## Módulo de alinhamento FIBO

O `bfo-core.owl` **não** faz `owl:imports` da FIBO. São cerca de dois milhões
de triplas: importá-las deixa o núcleo lento de abrir no Protégé e
impraticável de raciocinar, para todo mundo que só quer as classes
brasileiras. Os axiomas de alinhamento em si (`rdfs:subClassOf` para IRIs da
FIBO) ficam no núcleo — são OWL válido sem o import.

Se você precisa que um reasoner enxergue a hierarquia FIBO por trás desses
alinhamentos, carregue `ontology/bfo-fibo-alignment.owl` no lugar do núcleo.
Ele importa os dois.

## Validação

Todo push e pull request roda
[`.github/workflows/validate.yml`](.github/workflows/validate.yml):

| Verificação | Ferramenta |
|---|---|
| XML bem-formado, módulos mesclam | `robot merge` |
| Conformidade com o perfil OWL 2 DL | `robot validate-profile --profile DL` |
| Consistência, sem classes insatisfazíveis | `robot reason --reasoner HermiT` |
| Toda entidade tem `rdfs:label` e `skos:definition` | [`queries/anotacoes-obrigatorias.rq`](queries/anotacoes-obrigatorias.rq) |
| Nenhum rótulo duplicado entre entidades distintas | [`queries/rotulos-duplicados.rq`](queries/rotulos-duplicados.rq) |
| Relatório de modelagem (informativo, não bloqueia) | `robot report` |
| Os imports da FIBO resolvem, e cada IRI alinhada existe lá e não está deprecada | [`queries/alinhamentos-fantasma.rq`](queries/alinhamentos-fantasma.rq) |

O job da FIBO também roda semanalmente, porque a FIBO é publicada por
terceiros e muda sem aviso.

As serializações em Turtle são geradas pelo CI como artefato, em vez de
versionadas, para não haver duas fontes da verdade.

## Namespace

O namespace da ontologia é `https://w3id.org/bfo-br#`, prefixo `bfo`. A
tabela completa de prefixos (FIBO, OMG Commons, Open Finance, BCB) está em
[docs/README.md](docs/README.md#namespaces).

`https://w3id.org/bfo-br` é um IRI persistente com registro pendente junto ao
W3C Permanent Identifier Community Group — ver [w3id/README.md](w3id/README.md).
Enquanto o registro não sai, os IRIs não dereferenciam; nada depende disso,
porque o `catalog-v001.xml` resolve tudo localmente.

## Documentação

[docs/README.md](docs/README.md) descreve em detalhe as classes,
propriedades, mapeamentos, classificações regulatórias e exemplos de uso.

## Versionamento

O projeto segue o [Versionamento Semântico](https://semver.org/lang/pt-BR/).
Termos publicados nunca são removidos; os superados recebem
`owl:deprecated true` (ex.: `bfo:DOC`, descontinuado em 2024). Ver
[CHANGELOG.md](CHANGELOG.md).

## Citação

Se você usar a BFO, por favor cite (ver [CITATION.cff](CITATION.cff)):

```bibtex
@software{bfo2026,
  author       = {Brolesi, F. F.},
  title        = {{Brazilian Financial Ontology (BFO): Uma Ontologia Formal do Sistema Financeiro Brasileiro}},
  year         = {2026},
  publisher    = {GitHub},
  url          = {https://github.com/brolesi/bfo},
  version      = {2.0.0},
  note         = {Ontologia OWL/RDF alinhada com FIBO e integrada ao Open Finance Brasil e BCB}
}
```

## Contribuindo

Contribuições são bem-vindas. Leia o [CONTRIBUTING.md](CONTRIBUTING.md) para
as convenções de modelagem (nomenclatura de IRIs, anotações obrigatórias,
restrições de OWL DL) e abra uma issue ou pull request.

## Licença

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)
