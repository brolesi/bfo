# Changelog

Todas as mudanças relevantes deste projeto são documentadas neste arquivo.

O formato segue o [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/)
e o projeto adota [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [1.1.0] - 2026-07-11

### Adicionado

- **Fundos de investimento (Resolução CVM 175/2022)**: `FundoDeInvestimento`,
  `FundoRendaFixa`, `FundoDeAcoes`, `FundoMultimercado`, `FundoCambial`,
  `FIDC`, `FIP`; `ETF` e `FundoImobiliario` reclassificados também como
  fundos de investimento.
- **Previdência complementar**: `PlanoPrevidenciaComplementar`, `PGBL`, `VGBL`.
- **Ativos virtuais e moeda digital**: `AtivoVirtual` (Lei 14.478/2022),
  `PrestadoraDeServicosDeAtivosVirtuais` (PSAV/VASP),
  `MoedaDigitalDeBancoCentral` (CBDC) e o indivíduo `Drex`.
- **Novas instituições**: `SociedadeDeCreditoDireto` (SCD) e
  `SociedadeDeEmprestimoEntrePessoas` (SEP), conforme Resolução CMN 4.656/2018.
- **Infraestrutura de mercado**: classe `InfraestruturaDoMercadoFinanceiro`
  (alinhada aos PFMI do CPMI-IOSCO) e indivíduos `B3`, `TesouroNacional`,
  `FGC` e `ANBIMA`.
- **Operações**: `OperacaoCambio` (Lei 14.286/2021), `OperacaoCartaoCredito`
  e `DOC` (marcado como `owl:deprecated` — descontinuado em 2024).
- **Garantias**: `Garantia`, `GarantiaReal`, `GarantiaFidejussoria`,
  `AlienacaoFiduciaria`, `Hipoteca`, `Aval`, `Fianca` e a propriedade
  `garantidaPor`.
- **Consentimento Open Finance/LGPD**: `Consentimento`, `StatusConsentimento`
  (com indivíduos alinhados aos status das APIs do Open Finance Brasil) e
  propriedades `consentimentoConcedidoPor`, `consentimentoConcedidoA`,
  `statusConsentimento`, `dataConsentimento`, `dataExpiracaoConsentimento`.
- **Indicadores econômicos**: `PTAX` (SGS 1), `INPC` (SGS 188) e `Ibovespa`.
- **Identificadores**: `codigoLEI` (ISO 17442) e `codigoCOMPE`.
- **Propriedades de fundos**: `administradoPor` e `geridoPor`.
- Metadados de ontologia: `owl:versionIRI`, `dcterms:modified`,
  `vann:preferredNamespacePrefix`, `vann:preferredNamespaceUri`.
- Mapeamentos Open Finance adicionais: `Consent`, `VehicleLoan`,
  `CreditCardTransaction`, `InvestmentFund`, `BankFixedIncome`,
  `CreditFixedIncome`.
- Séries BCB/SGS adicionais no mapeamento BCB: PTAX (1) e INPC (188).
- Novos exemplos: financiamento veicular com alienação fiduciária, fundo
  multimercado (CVM 175) e consentimento Open Finance.
- Arquivos de governança do repositório: `LICENSE`, `CITATION.cff`,
  `CHANGELOG.md`, `CONTRIBUTING.md`, `.gitignore`.

### Corrigido

- IRI inválida `bfo:custodiado Por` (continha espaço) renomeada para
  `bfo:custodiadoPor`.
- Propriedade `bfo:envolveinstrumento` renomeada para
  `bfo:envolveInstrumento` (padrão camelCase), com atualização dos exemplos.
- Asserções de propriedades de dados feitas diretamente em classes
  (inválidas em OWL DL) convertidas em restrições
  `owl:Restriction`/`owl:hasValue` (CDB, LCI, LCA, Debênture, CRI, CRA,
  ações, títulos públicos, FII).
- Declaração de todas as propriedades usadas sem declaração
  (`tipoRemuneracao`, `tipoCarteiraBCB`, `direitoVoto`,
  `preferenciaDistribuicao`, `provisaoMinima`, `codigoBCB`, entre outras).
- Indivíduo `TesouroNacional`, antes referenciado sem declaração,
  formalmente declarado.
- Mapeamento BCB: propriedades de dados não são mais declaradas como
  subpropriedades de propriedades de objeto (`segmentoPrudencial`,
  `classificacaoRiscoSCR`) — substituídas por `rdfs:seeAlso`.
- `catalog-v001.xml` corrigido: o import do FIBO apontava, por engano,
  para o arquivo de exemplos; agora resolve os módulos locais da BFO.

## [1.0.0] - 2026-01-15

### Adicionado

- Ontologia núcleo (`bfo-core.owl`) com órgãos reguladores, instituições
  financeiras, pessoas, instrumentos financeiros, contas, operações,
  indicadores econômicos e tributação.
- Módulo de mapeamento Open Finance Brasil
  (`bfo-openfinance-mapping.owl`).
- Módulo de mapeamento BCB/SGS (`bfo-bcb-mapping.owl`).
- Arquivo de exemplos (`bfo-examples.owl`).
- Documentação em `docs/README.md`.

[1.1.0]: https://github.com/brolesi/bfo/releases/tag/v1.1.0
[1.0.0]: https://github.com/brolesi/bfo/releases/tag/v1.0.0
