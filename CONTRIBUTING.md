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

- O arquivo deve ser XML bem-formado e carregável no
  [Protégé](https://protege.stanford.edu/) sem erros.
- Verifique a consistência lógica com um reasoner (HermiT ou ELK).
- Atualize `CHANGELOG.md` e, se houver mudança de conteúdo, os exemplos
  em `examples/` e a documentação em `docs/`.

## Versionamento

O projeto segue [SemVer](https://semver.org/lang/pt-BR/):

- **MAJOR**: mudanças incompatíveis (remoção/renomeação de IRIs);
- **MINOR**: novos termos e mapeamentos retrocompatíveis;
- **PATCH**: correções de anotações, definições e documentação.

## Licença

Ao contribuir, você concorda que sua contribuição será licenciada sob
[CC BY 4.0](LICENSE).
