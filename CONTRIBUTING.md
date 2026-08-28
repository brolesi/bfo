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
- **Datas**: use sempre `xsd:dateTime`, nunca `xsd:date`. O mapa de
  datatypes do OWL 2 inclui `xsd:dateTime` e `xsd:dateTimeStamp`, mas **não**
  `xsd:date` — usá-lo tira a ontologia do perfil DL e o job `core` do CI
  falha em `validate-profile`. Vale para o `rdfs:range` e para o
  `rdf:datatype` de cada literal. A exceção são as anotações do cabeçalho
  (`dcterms:created`, `dcterms:modified`), que o perfil DL não submete ao
  mapa de datatypes.
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

### Sobre os ERRORs do `robot report`

O `robot report` roda como **informativo** e não bloqueia o merge. Ele segue
as convenções OBO, e três classes de violação que ele acusa aqui são
decisões deliberadas deste projeto — não corrija:

| Regra | Por que fica assim |
|---|---|
| `multiple_labels` | Rótulo em pt-BR **e** en é característica declarada do projeto. |
| `duplicate_label` | Um stub `&ofb;`/`&bcb;` declarado `owl:equivalentClass` de um termo BFO denota a mesma classe; compartilhar rótulo é correto. A regra de verdade está em `queries/rotulos-duplicados.rq`, que exclui esses pares. |
| `deprecated_class_reference` | Termos depreciados (ex.: `bfo:DOC`) **mantêm** seus `rdfs:subClassOf`. A convenção OBO os desliga da hierarquia; aqui eles precisam continuar classificando dados históricos. |

Qualquer outra regra em nível ERROR é para corrigir. Em especial
`label_whitespace` e `label_formatting`: escreva `rdfs:label` e
`skos:definition` **em uma única linha**, sem quebra nem indentação — em XML
essa formatação entra no literal e vaza para quem consulta.

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
