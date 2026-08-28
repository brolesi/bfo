# Contribuindo com a Brazilian Financial Ontology (BFO)

Obrigado pelo interesse em contribuir! Este documento descreve as
convenções do projeto.

## Como contribuir

1. Abra uma **issue** descrevendo a proposta (nova classe, correção de
   definição, novo mapeamento etc.) antes de submeter mudanças grandes.
2. Faça um *fork* e crie um branch descritivo
   (`feat/fundos-cvm-175`, `fix/iri-invalida`).
3. Submeta um **pull request** referenciando a issue.

## Convenções de modelagem

- **Nomenclatura de IRIs**
  - Classes: `PascalCase` em português, sem acentos — `FundoDeInvestimento`.
  - Propriedades: `camelCase` — `emitidoPor`, `taxaJuros`.
  - Indivíduos: `PascalCase` ou sigla consagrada — `TaxaSelic`, `IPCA`.
- **Anotações obrigatórias** para toda nova entidade:
  - `rdfs:label` com `xml:lang="pt-BR"` (e `en` quando aplicável);
  - `skos:definition` em pt-BR;
  - referência normativa em `rdfs:comment` quando a entidade derivar de
    lei ou resolução (ex.: "Resolução CVM 175/2022").
- **Alinhamento externo**: use `rdfs:subClassOf` / `owl:equivalentClass`
  para FIBO e Open Finance Brasil sempre que houver correspondência.
- **OWL DL**: não faça asserções de propriedades de dados diretamente em
  classes; use restrições (`owl:Restriction` + `owl:hasValue`) ou mova a
  asserção para indivíduos.
- **Depreciação**: nunca remova termos publicados; marque-os com
  `owl:deprecated true` e documente a substituição.

## Validação antes do PR

O CI (`.github/workflows/validate.yml`) roda tudo isto automaticamente em
cada PR. Você não precisa reproduzir localmente — mas se quiser o ciclo
curto, instale o [ROBOT](https://robot.obolibrary.org/) e rode:

```bash
robot merge --input ontology/bfo-core.owl --input ontology/bfo-openfinance-mapping.owl --input ontology/bfo-bcb-mapping.owl --input examples/bfo-examples.owl --output merged.owl
robot validate-profile --profile DL --input merged.owl
robot reason --reasoner HermiT --input merged.owl --output reasoned.owl
robot verify --input merged.owl --queries queries/anotacoes-obrigatorias.rq queries/rotulos-duplicados.rq --output-dir .
```

O job `fibo-alignment` baixa a FIBO e verifica que toda IRI externa
referenciada existe de fato. **Não invente IRIs da FIBO**: elas são
hierárquicas (`…/SEC/Equities/EquityInstruments/Share`), não achatadas, e
vários termos de topo vivem no OMG Commons (`cmns-org`, `cmns-pts`,
`cmns-rga`). Confira em <https://spec.edmcouncil.org/fibo/> antes de alinhar.

Atualize `CHANGELOG.md` e, se houver mudança de conteúdo, os exemplos
em `examples/` e a documentação em `docs/`.

## Versionamento

O projeto segue [SemVer](https://semver.org/lang/pt-BR/):

- **MAJOR**: mudanças incompatíveis (remoção/renomeação de IRIs);
- **MINOR**: novos termos e mapeamentos retrocompatíveis;
- **PATCH**: correções de anotações, definições e documentação.

## Licença

Ao contribuir, você concorda que sua contribuição será licenciada sob
[CC BY 4.0](LICENSE).
