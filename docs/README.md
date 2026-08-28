# Brazilian Financial Ontology (BFO)

## Visão Geral

A **Brazilian Financial Ontology (BFO)** é uma ontologia formal do sistema financeiro brasileiro, desenvolvida em OWL/RDF com os seguintes objetivos:

1. **Interoperabilidade semântica** entre sistemas financeiros
2. **Alinhamento com padrões internacionais** (FIBO - Financial Industry Business Ontology)
3. **Integração com padrões brasileiros** (Open Finance Brasil, BCB)

## Estrutura de Arquivos

```
bfo/
├── ontology/
│   ├── bfo-core.owl                  # Ontologia principal
│   ├── bfo-openfinance-mapping.owl   # Mapeamento Open Finance Brasil
│   ├── bfo-bcb-mapping.owl           # Mapeamento BCB/SGS
│   ├── bfo-fibo-alignment.owl        # Módulo opt-in que importa a FIBO
│   └── catalog-v001.xml              # Catálogo XML (resolução local no Protégé)
├── examples/
│   └── bfo-examples.owl              # Exemplos de instâncias
├── queries/                          # Consultas SPARQL verificadas pelo CI
├── w3id/                             # Registro do IRI persistente w3id.org
├── legacy/                           # Ontologia anterior, fora da distribuição
└── docs/
    └── README.md                     # Esta documentação
```

## Namespaces

| Prefixo | URI | Descrição |
|---------|-----|-----------|
| `bfo` | `https://w3id.org/bfo-br#` | Brazilian Financial Ontology |
| `cmns-org` | `https://www.omg.org/spec/Commons/Organizations/` | OMG Commons - Organizations |
| `cmns-pts` | `https://www.omg.org/spec/Commons/PartiesAndSituations/` | OMG Commons - Parties |
| `cmns-rga` | `https://www.omg.org/spec/Commons/RegulatoryAgencies/` | OMG Commons - Regulatory Agencies |
| `fibo-fbc-fct-fse` | `…/fibo/ontology/FBC/FunctionalEntities/FinancialServicesEntities/` | FIBO FBC - Entidades |
| `fibo-fbc-fi-fi` | `…/fibo/ontology/FBC/FinancialInstruments/FinancialInstruments/` | FIBO FBC - Instrumentos |
| `fibo-fbc-pas-caa` | `…/fibo/ontology/FBC/ProductsAndServices/ClientsAndAccounts/` | FIBO FBC - Contas |
| `fibo-fbc-pas-fpas` | `…/fibo/ontology/FBC/ProductsAndServices/FinancialProductsAndServices/` | FIBO FBC - Produtos |
| `fibo-fnd-arr-id` | `…/fibo/ontology/FND/Arrangements/IdentifiersAndIndices/` | FIBO FND - Índices |
| `fibo-fnd-pas-pas` | `…/fibo/ontology/FND/ProductsAndServices/ProductsAndServices/` | FIBO FND - Produtos |
| `fibo-fnd-rel-rel` | `…/fibo/ontology/FND/Relations/Relations/` | FIBO FND - Relações |
| `fibo-be-le-lp` | `…/fibo/ontology/BE/LegalEntities/LegalPersons/` | FIBO BE - Pessoas |
| `fibo-sec-eq-eq` | `…/fibo/ontology/SEC/Equities/EquityInstruments/` | FIBO SEC - Ações |
| `fibo-sec-eq-dr` | `…/fibo/ontology/SEC/Equities/DepositaryReceipts/` | FIBO SEC - BDR |
| `fibo-sec-dbt-bnd` | `…/fibo/ontology/SEC/Debt/Bonds/` | FIBO SEC - Títulos de dívida |
| `fibo-sec-fund-fund` | `…/fibo/ontology/SEC/Funds/Funds/` | FIBO SEC - Fundos |
| `fibo-sec-sec-pls` | `…/fibo/ontology/SEC/Securities/Pools/` | FIBO SEC - Veículos de investimento coletivo |
| `fibo-der-drc-swp` | `…/fibo/ontology/DER/DerivativesContracts/Swaps/` | FIBO DER - Swaps |
| `fibo-loan-ln-ln` | `…/fibo/ontology/LOAN/LoansGeneral/Loans/` | FIBO LOAN - Empréstimos |
| `fibo-loan-spc-cra` | `…/fibo/ontology/LOAN/LoansSpecific/CardAccounts/` | FIBO LOAN - Cartões |
| `fibo-fnd-acc-cur` | `…/fibo/ontology/FND/Accounting/CurrencyAmount/` | FIBO FND - Moeda e valores |
| `fibo-fnd-agr-ctr` | `…/fibo/ontology/FND/Agreements/Contracts/` | FIBO FND - Contratos |
| `fibo-fnd-agr-agr` | `…/fibo/ontology/FND/Agreements/Agreements/` | FIBO FND - Acordos |
| `fibo-fbc-dae-gty` | `…/fibo/ontology/FBC/DebtAndEquities/Guaranty/` | FIBO FBC - Garantias e seguro |
| `fibo-fbc-dae-dbt` | `…/fibo/ontology/FBC/DebtAndEquities/Debt/` | FIBO FBC - Dívida |
| `fibo-fbc-dae-cr` | `…/fibo/ontology/FBC/DebtAndEquities/CreditRatings/` | FIBO FBC - Rating |
| `fibo-sec-sec-iss` | `…/fibo/ontology/SEC/Securities/SecuritiesIssuance/` | FIBO SEC - Emissão |
| `fibo-sec-dbt-tstd` | `…/fibo/ontology/SEC/Debt/TradedShortTermDebt/` | FIBO SEC - Dívida de curto prazo |
| `fibo-be-oac-cown` | `…/fibo/ontology/BE/OwnershipAndControl/CorporateOwnership/` | FIBO BE - Participação societária |
| `fibo-cae-cev-cac` | `…/fibo/ontology/CAE/CorporateEvents/CorporateActions/` | FIBO CAE - Eventos societários |
| `fibo-cae-cev-scac` | `…/fibo/ontology/CAE/CorporateEvents/SecurityRelatedCorporateActions/` | FIBO CAE - Eventos sobre valores mobiliários |
| `ofb` | `https://openfinancebrasil.org.br/schema/` | Open Finance Brasil |
| `bcb` | `https://dadosabertos.bcb.gov.br/schema/` | BCB Dados Abertos |

## Nota de Modelagem (OWL 2 DL)

Características de classe (ex.: "CDB tem cobertura FGC") são expressas como
restrições `owl:Restriction` com `owl:hasValue`, e não como asserções de
propriedades de dados diretamente na classe — o que violaria o perfil OWL DL.
Termos descontinuados no mercado (ex.: `bfo:DOC`) são mantidos com
`owl:deprecated true` para representação de dados históricos.

## Classes Principais

### 1. Agentes Financeiros

| Classe BFO | Superclasse FIBO | Descrição |
|------------|------------------|-----------|
| `bfo:OrgaoReguladorBrasileiro` | `cmns-rga:RegulatoryAgency` | BCB, CVM, SUSEP, PREVIC |
| `bfo:InstituicaoFinanceiraBrasileira` | `fibo-fbc-pas-fpas:FinancialServiceProvider` | Bancos, cooperativas, fintechs |
| `bfo:BancoComercial` | `fibo-fbc-fct-fse:DepositoryInstitution` | Banco que capta depósitos à vista |
| `bfo:BancoMultiplo` | - | Opera com múltiplas carteiras |
| `bfo:InstituicaoDePagamento` | - | IPs reguladas pela Lei 12.865/2013 |
| `bfo:SociedadeDeCreditoDireto` | - | SCD - fintech de crédito (Res. CMN 4.656/2018) |
| `bfo:SociedadeDeEmprestimoEntrePessoas` | - | SEP - P2P lending (Res. CMN 4.656/2018) |
| `bfo:PrestadoraDeServicosDeAtivosVirtuais` | `fibo-fbc-pas-fpas:FinancialServiceProvider` | PSAV/VASP (Lei 14.478/2022) |
| `bfo:InfraestruturaDoMercadoFinanceiro` | - | FMIs, alinhada aos PFMI (CPMI-IOSCO) |

**Indivíduos de infraestrutura e suporte**: `bfo:B3`, `bfo:TesouroNacional`,
`bfo:FGC`, `bfo:ANBIMA`.

### 2. Pessoas (Parties)

| Classe BFO | Equivalente Open Finance | Descrição |
|------------|-------------------------|-----------|
| `bfo:PessoaFisica` | `ofb:NaturalPerson` | Identificada por CPF |
| `bfo:PessoaJuridica` | `ofb:BusinessEntity` | Identificada por CNPJ (e opcionalmente LEI) |

### 3. Instrumentos Financeiros

#### Renda Fixa
| Classe BFO | Superclasse FIBO | Características |
|------------|------------------|-----------------|
| `bfo:TituloPublicoFederal` | `fibo-sec-dbt-bnd:SovereignDebtInstrument` | Tesouro Direto |
| `bfo:TesouroSelic` | - | Pós-fixado, indexado à Selic |
| `bfo:TesouroIPCA` | - | Híbrido, IPCA + taxa |
| `bfo:CDB` | `fibo-fbc-pas-caa:CertificateOfDeposit` | Cobertura FGC até R$ 250k |
| `bfo:LCI` / `bfo:LCA` | - | Isentos de IR para PF |
| `bfo:CRI` / `bfo:CRA` | - | Securitização, isentos de IR para PF |
| `bfo:Debenture` | `fibo-sec-dbt-bnd:CorporateBond` | Sem cobertura FGC |

#### Renda Variável
| Classe BFO | Superclasse FIBO | Características |
|------------|------------------|-----------------|
| `bfo:AcaoOrdinaria` | `fibo-sec-eq-eq:CommonShare` | ON - direito a voto |
| `bfo:AcaoPreferencial` | `fibo-sec-eq-eq:PreferredShare` | PN - preferência em dividendos |
| `bfo:FundoImobiliario` | - | FII - rendimentos isentos* |
| `bfo:ETF` | `fibo-sec-fund-fund:ExchangeTradedFund` | Replica índice |
| `bfo:BDR` | `fibo-sec-eq-dr:DepositaryReceipt` | Ações estrangeiras |

#### Fundos de Investimento (Resolução CVM 175/2022)
| Classe BFO | Descrição |
|------------|-----------|
| `bfo:FundoDeInvestimento` | Classe geral (superclasse de ETF e FII) |
| `bfo:FundoRendaFixa` | ≥ 80% em ativos de taxa de juros/índice de preços |
| `bfo:FundoDeAcoes` | ≥ 67% em ações |
| `bfo:FundoMultimercado` | Múltiplos fatores de risco |
| `bfo:FundoCambial` | ≥ 80% em variação cambial |
| `bfo:FIDC` | Direitos creditórios |
| `bfo:FIP` | Participações (private equity / venture capital) |

Propriedades associadas: `bfo:administradoPor` (administrador fiduciário) e
`bfo:geridoPor` (gestor de carteira).

#### Previdência Complementar
| Classe BFO | Descrição |
|------------|-----------|
| `bfo:PGBL` | Contribuições dedutíveis (até 12% da renda); IR sobre o total |
| `bfo:VGBL` | Sem dedução; IR apenas sobre rendimentos |

#### Ativos Virtuais e Moeda Digital
| Classe/Indivíduo BFO | Descrição |
|----------------------|-----------|
| `bfo:AtivoVirtual` | Lei 14.478/2022 (Marco Legal dos Criptoativos) |
| `bfo:MoedaDigitalDeBancoCentral` | CBDC |
| `bfo:Drex` | Real Digital - piloto do BCB (indivíduo) |

#### Derivativos
`bfo:Opcao`, `bfo:ContratoFuturo`, `bfo:Swap` — alinhados a `fibo-fbc-fi-fi:DerivativeInstrument`.

### 4. Contas

| Classe BFO | Equivalente Open Finance | Descrição |
|------------|-------------------------|-----------|
| `bfo:ContaCorrente` | `ofb:CheckingAccount` | Depósitos à vista |
| `bfo:ContaPoupanca` | `ofb:SavingsAccount` | Remuneração TR + 0,5% a.m. |
| `bfo:ContaPagamento` | `ofb:PaymentAccount` | Instituições de pagamento |

### 5. Operações

| Classe BFO | Superclasse FIBO | Exemplos |
|------------|------------------|----------|
| `bfo:OperacaoCredito` | `fibo-loan-ln-ln:Loan` | Empréstimos, financiamentos |
| `bfo:OperacaoPagamento` | `fibo-fbc-pas-caa:IndividualTransaction` | PIX, TED, Boleto, Cartão de Crédito |
| `bfo:OperacaoInvestimento` | - | Aplicação, Resgate |
| `bfo:OperacaoCambio` | - | Novo marco cambial (Lei 14.286/2021) |

Observações:
- A mensageria do arranjo **Pix** segue o padrão **ISO 20022** (pacs.008, pacs.002).
- `bfo:DOC` está marcado como `owl:deprecated` (descontinuado em janeiro de 2024).

### 6. Garantias

| Classe BFO | Tipo | Descrição |
|------------|------|-----------|
| `bfo:AlienacaoFiduciaria` | Real | Propriedade resolúvel transferida ao credor |
| `bfo:Hipoteca` | Real | Direito real sobre imóvel |
| `bfo:Aval` | Fidejussória | Garantia autônoma em títulos de crédito |
| `bfo:Fianca` | Fidejussória | Garantia acessória |

Vinculadas à operação por `bfo:garantidaPor`.

### 7. Consentimento (Open Finance / LGPD)

A classe `bfo:Consentimento` (equivalente a `ofb:Consent`) modela o ciclo de
vida do consentimento conforme a LGPD (Lei 13.709/2018) e a Resolução
Conjunta CMN/BCB 1/2020:

- `bfo:consentimentoConcedidoPor` → cliente (Pessoa)
- `bfo:consentimentoConcedidoA` → instituição receptora/iniciadora
- `bfo:statusConsentimento` → `AWAITING_AUTHORISATION`, `AUTHORISED`,
  `REJECTED`, `REVOKED`
- `bfo:dataConsentimento` / `bfo:dataExpiracaoConsentimento`

## Propriedades Principais

### Object Properties (Relações)

```turtle
bfo:reguladoPor        → OrgaoRegulador
bfo:possuiConta        → Conta
bfo:realizadaPor       → Pessoa
bfo:emitidoPor         → PessoaJuridica
bfo:envolveInstrumento → InstrumentoFinanceiro
bfo:custodiadoPor      → InstituicaoFinanceiraBrasileira
bfo:indexadoPor        → IndicadorEconomico
bfo:classificacaoRisco → ClassificacaoRiscoCredito
bfo:garantidaPor       → Garantia
bfo:administradoPor    → InstituicaoFinanceiraBrasileira
bfo:geridoPor          → PessoaJuridica
bfo:statusConsentimento → StatusConsentimento
```

### Data Properties (Atributos)

```turtle
# Identificadores
bfo:cpf               → xsd:string (11 dígitos)
bfo:cnpj              → xsd:string (14 dígitos)
bfo:codigoISPB        → xsd:string (8 dígitos)
bfo:codigoCOMPE       → xsd:string (3 dígitos)
bfo:codigoLEI         → xsd:string (20 caracteres, ISO 17442)
bfo:codigoISIN        → xsd:string (12 caracteres)
bfo:ticker            → xsd:string
bfo:chavePIX          → xsd:string

# Valores
bfo:valorBRL          → xsd:decimal
bfo:taxaJuros         → xsd:decimal

# Datas
bfo:dataOperacao      → xsd:dateTime
bfo:dataVencimento    → xsd:dateTime
bfo:dataLiquidacao    → xsd:dateTime
bfo:dataConsentimento → xsd:dateTime

# Booleanos
bfo:coberturaFGC      → xsd:boolean
bfo:isentoIRPF        → xsd:boolean
bfo:direitoVoto       → xsd:boolean
```

## Alinhamento FIBO

A BFO está alinhada com os seguintes módulos do FIBO:

| Módulo FIBO | Classes Utilizadas |
|-------------|-------------------|
| **FND** (Foundations) | Index, TransactionEvent, isIssuedBy |
| **BE** (Business Entities) | LegallyCompetentNaturalPerson |
| **FBC** (Financial Business & Commerce) | FinancialServiceProvider, DepositoryInstitution, InvestmentBank, CreditUnion, BrokerDealer, Account, DemandDepositAccount, DepositAccount, CertificateOfDeposit, IndividualTransaction, FinancialInstrument, DebtInstrument, EquityInstrument, DerivativeInstrument, Option, Future |
| **SEC** (Securities) | Share, CommonShare, PreferredShare, DepositaryReceipt, CorporateBond, SovereignDebtInstrument, ExchangeTradedFund, CollectiveInvestmentVehicle |
| **DER** (Derivatives) | Swap |
| **LOAN** (Loans) | Loan |
| **OMG Commons** | Party, LegalEntity, RegulatoryAgency |

O core **não** importa a FIBO: são ~2 milhões de triplas, e importá-las torna
o carregamento no Protégé e o raciocínio impraticáveis para quem só quer as
classes brasileiras. Os alinhamentos (`rdfs:subClassOf` para IRIs da FIBO)
ficam no core e são OWL válido sem o import.

Para que um reasoner enxergue a hierarquia FIBO por trás deles, carregue
`ontology/bfo-fibo-alignment.owl` no lugar do core — ele importa os dois.

> **Atenção ao alinhar novos termos**: as IRIs da FIBO são hierárquicas
> (`…/SEC/Equities/EquityInstruments/Share`), não achatadas
> (`…/SEC/Share`), e vários termos de topo migraram para o OMG Commons.
> Até a v1.1.0 os 35 alinhamentos deste arquivo apontavam para IRIs que não
> existem; o job `fibo-alignment` do CI existe para impedir a repetição.

## Integração Open Finance Brasil

Mapeamentos diretos com as APIs do Open Finance:

| Fase | APIs | Classes BFO |
|------|------|-------------|
| 1 | Produtos, Canais | InstituicaoFinanceiraBrasileira |
| 2 | Contas, Clientes, Cartões | Conta, PessoaFisica, PessoaJuridica, OperacaoCartaoCredito |
| 3 | Pagamentos | PIX, OperacaoPagamento |
| 4 | Investimentos | InstrumentoRendaFixa, InstrumentoRendaVariavel, FundoDeInvestimento |
| Transversal | Consentimento | Consentimento, StatusConsentimento |

## Integração BCB

Mapeamentos com dados abertos do Banco Central:

| Sistema BCB | Classes/Propriedades BFO |
|-------------|-------------------------|
| **SGS** (Séries Temporais) | IndicadorEconomico (Selic, CDI, IPCA, IGP-M, TR, PTAX, INPC) |
| **IF.data** | InstituicaoFinanceiraBrasileira, SegmentacaoPrudencial |
| **SCR** | OperacaoCredito, ClassificacaoRiscoCredito |
| **DICT** | chavePIX |

### Séries SGS mapeadas

| Série | Indicador |
|-------|-----------|
| 1 | PTAX (dólar, venda) |
| 188 | INPC |
| 189 | IGP-M |
| 226 | TR |
| 433 | IPCA |
| 4189 | Taxa Selic (meta) |
| 4391 | CDI |

## Domínios acrescentados na v2.1.0

### Fundamentos monetários, contratuais e temporais

Base estrutural exigida pelos demais domínios: câmbio precisa de moeda,
apólice e consórcio precisam de contrato, classificação regulatória precisa
de vigência.

| Termo BFO | Superclasse FIBO | Observação |
|---|---|---|
| `bfo:Moeda` | `fibo-fnd-acc-cur:Currency` | Indivíduos `bfo:BRL`, `bfo:USD`, `bfo:EUR`, com `bfo:codigoISO4217` |
| `bfo:Contrato` | `fibo-fnd-agr-ctr:Contract` | Instrumento jurídico que formaliza a operação |
| `bfo:CalendarioBancario` | - | Indivíduo `bfo:CalendarioANBIMA` |

Propriedades: `bfo:valor` e `bfo:moeda` (substituem `bfo:valorBRL`),
`bfo:formalizadaPor`, `bfo:parteDoContrato`, `bfo:dataInicioVigencia`,
`bfo:dataFimVigencia`, `bfo:prazoLiquidacaoDiasUteis`, `bfo:dataFeriado`.

> **`bfo:valorBRL` está depreciada.** A denominação estava embutida no nome
> da propriedade, o que impede representar câmbio e conta em moeda
> estrangeira. Use `bfo:valor` com `bfo:moeda`. Conforme a política de
> versionamento, o termo antigo permanece publicado.

### Seguros, resseguro e capitalização (SUSEP)

| Classe BFO | Superclasse FIBO | Referência normativa |
|---|---|---|
| `bfo:Seguradora` | `fibo-fbc-fct-fse:InsuranceCompany` | Decreto-Lei 73/1966 |
| `bfo:Resseguradora` | `fibo-fbc-fct-fse:InsuranceCompany` | LC 126/2007 |
| `bfo:CorretoraDeSeguros` | - | Lei 4.594/1964 |
| `bfo:SociedadeDeCapitalizacao` | - | Decreto-Lei 261/1967 |
| `bfo:EntidadeAbertaDePrevidenciaComplementar` | - | LC 109/2001 |
| `bfo:ApoliceDeSeguro` | `fibo-fbc-dae-gty:InsurancePolicy` | CC, arts. 757 a 802 |
| `bfo:Sinistro`, `bfo:PremioDeSeguro`, `bfo:TituloDeCapitalizacao` | - | - |

`bfo:planoOperadoPor` liga PGBL e VGBL à EAPC que os opera, fechando a
lacuna em que ambos existiam sem entidade operadora.

### Previdência complementar fechada (PREVIC)

`bfo:EntidadeFechadaDePrevidenciaComplementar` (alinhada a
`fibo-sec-fund-fund:PensionFund`), `bfo:PlanoDeBeneficios` com as
modalidades BD, CD e CV, e os papéis `bfo:Patrocinador`,
`bfo:InstituidorDePlano`, `bfo:Participante` e `bfo:Assistido`.
Identificação por `bfo:codigoCNPB`.

### Infraestruturas do mercado financeiro

| Indivíduo | Classe | Papel |
|---|---|---|
| `bfo:SPB` | `InfraestruturaDoMercadoFinanceiro` | Sistema de Pagamentos Brasileiro |
| `bfo:STR` | `SistemaDePagamentos` | Liquida a TED em tempo real |
| `bfo:SistemaSelic` | `DepositariaCentral` | Custódia e liquidação de títulos públicos |
| `bfo:SPI` | `SistemaDePagamentos` | Liquida o PIX |
| `bfo:DICT` | `SistemaDeRegistro` | Diretório de chaves PIX |
| `bfo:Nuclea` | `CamaraDeCompensacao` | Antiga CIP |
| `bfo:SCR` | `SistemaDeRegistro` | Sistema de Informações de Créditos |

> `bfo:SistemaSelic` é o sistema; `bfo:TaxaSelic` é a taxa. A taxa recebe
> esse nome por ser apurada nas compromissadas registradas no sistema.

### Arranjos de pagamento, cartões e PIX

Cadeia da Lei 12.865/2013: `bfo:ArranjoDePagamento`,
`bfo:InstituidorDeArranjo`, `bfo:Credenciadora`, `bfo:Subcredenciadora`,
`bfo:EmissorDeInstrumentoDePagamento`, `bfo:BandeiraDeCartao`.

Cartões alinhados ao módulo LOAN da FIBO: `bfo:CartaoDePagamento`
(`fibo-loan-spc-cra:PaymentCard`), `bfo:CartaoDeCredito`
(`fibo-loan-spc-cra:CreditCard`), `bfo:CartaoDeDebito`, `bfo:CartaoPrePago`,
`bfo:ContaDeCartao` (`fibo-loan-spc-cra:CardAccount`).

PIX: modalidades `bfo:PIXAutomatico`, `bfo:PIXCobranca`, `bfo:PIXSaque`,
`bfo:PIXTroco`, e `bfo:TipoChavePIX` como enumeração fechada dos cinco tipos
de chave registráveis no DICT.

### Consórcio (Lei 11.795/2008)

`bfo:AdministradoraDeConsorcio`, `bfo:GrupoDeConsorcio`,
`bfo:CotaDeConsorcio`, `bfo:Contemplacao`, `bfo:Lance`, com
`bfo:taxaAdministracao` e `bfo:prazoGrupoMeses`.

### Títulos de crédito

| Classe BFO | Referência normativa |
|---|---|
| `bfo:CCB` | Lei 10.931/2004 |
| `bfo:CCI` | Lei 10.931/2004 — lastro usual de CRI |
| `bfo:CPR` | Lei 8.929/1994, alterada pela Lei 13.986/2020 |
| `bfo:CDCA` | Lei 11.076/2004 |
| `bfo:WarrantAgropecuario` | Lei 11.076/2004 |
| `bfo:DuplicataEscritural` | Lei 13.775/2018 |
| `bfo:NotaComercial` | Lei 14.195/2021 |

### Ofertas públicas (Resolução CVM 160/2022)

`bfo:OfertaPublica` (`fibo-sec-sec-iss:PublicOffering`),
`bfo:OfertaPublicaInicial`, `bfo:OfertaSubsequente`, `bfo:Prospecto`,
`bfo:CoordenadorLider` e `bfo:RitoDeRegistro` como enumeração fechada
(automático ou ordinário).

### Eventos societários e governança

`bfo:EventoSocietario` (`fibo-cae-cev-cac:CorporateAction`) com
`bfo:Dividendo`, `bfo:JurosSobreCapitalProprio`, `bfo:Bonificacao`,
`bfo:Desdobramento` (`fibo-cae-cev-scac:StockSplit`), `bfo:Grupamento`,
`bfo:Subscricao` e `bfo:RecompraDeAcoes`, mais `bfo:dataCom` e `bfo:dataEx`.

`bfo:NivelGovernancaB3` cobre Novo Mercado, Nível 2, Nível 1 e Bovespa Mais.

> O JCP não tem contrapartida na FIBO: é instituto da Lei 9.249/1995 sem
> equivalente direto na maioria das jurisdições.

### PLD/FT (Circular BCB 3.978/2020)

`bfo:UnidadeDeInteligenciaFinanceira` com o indivíduo `bfo:COAF`,
`bfo:ComunicacaoDeOperacaoSuspeita`, `bfo:PessoaExpostaPoliticamente`,
`bfo:AvaliacaoKYC` e a propriedade `bfo:beneficiarioFinal`.

### Câmbio (Lei 14.286/2021)

`bfo:ContratoDeCambio` com as modalidades pronto e futuro, `bfo:ACC`,
`bfo:ACE` e `bfo:ContaEmMoedaEstrangeira`, cuja abertura ao público em geral
foi justamente o que o novo marco cambial autorizou.

### Crédito, rating e tributação

Novas operações: `bfo:ChequeEspecial`, `bfo:CapitalDeGiro`,
`bfo:AntecipacaoDeRecebiveis`, `bfo:CreditoRural`, `bfo:Microcredito`,
`bfo:CreditoEstudantil`.

`bfo:RatingDeCredito` e `bfo:AgenciaDeClassificacaoDeRisco` representam a
opinião de agência sobre o emissor — distinta de
`bfo:ClassificacaoRiscoCredito`, que é o enquadramento obrigatório da
operação na escala AA a H da Resolução CMN 2.682/1999.

Tributação ganhou `bfo:aliquota`, `bfo:prazoMinimoDias`,
`bfo:prazoMaximoDias`, `bfo:limiteIsencaoMensal` e os indivíduos
`bfo:ComeCotas`, `bfo:IOF_Cambio`, `bfo:IOF_Credito`, `bfo:IOF_Seguro`,
`bfo:IOF_Titulos`, `bfo:IR_GanhoCapital` e `bfo:IR_FonteRendaFixa`.

### Sustentabilidade e ativos virtuais

`bfo:TituloTematicoSustentavel` com `bfo:TituloVerde` e `bfo:TituloSocial`
— deliberadamente **não** disjuntos de título público nem privado, porque
existem emissões soberanas e corporativas. Em ativos virtuais,
`bfo:Stablecoin` e `bfo:TokenDeRecebivel`.

## Exemplos de Uso

### SPARQL - Buscar CDBs por emissor e taxa

```sparql
PREFIX bfo: <https://w3id.org/bfo-br#>

SELECT ?cdb ?emissor ?taxa WHERE {
  ?cdb a bfo:CDB ;
       bfo:emitidoPor ?emissor ;
       bfo:taxaJuros ?taxa .
}
```

### SPARQL - Operações de um Cliente

```sparql
PREFIX bfo: <https://w3id.org/bfo-br#>

SELECT ?operacao ?tipo ?valor ?data WHERE {
  ?cliente bfo:cpf "12345678901" .
  ?operacao bfo:realizadaPor ?cliente ;
            a ?tipo ;
            bfo:valorBRL ?valor ;
            bfo:dataOperacao ?data .
}
ORDER BY DESC(?data)
```

### SPARQL - Consentimentos ativos de um cliente

```sparql
PREFIX bfo: <https://w3id.org/bfo-br#>

SELECT ?consent ?instituicao ?expira WHERE {
  ?consent a bfo:Consentimento ;
           bfo:consentimentoConcedidoPor ?cliente ;
           bfo:consentimentoConcedidoA ?instituicao ;
           bfo:statusConsentimento bfo:ConsentimentoAutorizado ;
           bfo:dataExpiracaoConsentimento ?expira .
  ?cliente bfo:cpf "12345678901" .
}
```

## Classificações Regulatórias

### Segmentação Prudencial BCB (Res. 4.553/2017)

| Segmento | Critério |
|----------|----------|
| S1 | ≥ 10% PIB ou atividade internacional relevante |
| S2 | 1% a 10% PIB |
| S3 | 0,1% a 1% PIB |
| S4 | < 0,1% PIB |
| S5 | < 0,1% PIB + perfil simplificado |

### Classificação de Investidor CVM (Res. 30/2021)

| Tipo | Critério |
|------|----------|
| Profissional | > R$ 10 milhões aplicados |
| Qualificado | > R$ 1 milhão aplicados |
| Varejo | Demais |

### Risco de Crédito BCB (Res. 2.682/1999)

| Rating | Provisão Mínima |
|--------|-----------------|
| AA | 0% |
| A | 0,5% |
| B | 1% |
| C | 3% |
| D | 10% |
| E | 30% |
| F | 50% |
| G | 70% |
| H | 100% |

## Marcos Regulatórios Referenciados

| Norma | Tema | Elementos BFO |
|-------|------|---------------|
| Lei 12.865/2013 | Instituições de pagamento | `InstituicaoDePagamento` |
| Res. CMN 4.656/2018 | SCD e SEP | `SociedadeDeCreditoDireto`, `SociedadeDeEmprestimoEntrePessoas` |
| Res. Conjunta CMN/BCB 1/2020 | Open Finance | `Consentimento` |
| Lei 14.286/2021 | Marco cambial | `OperacaoCambio` |
| Res. CVM 175/2022 | Fundos de investimento | `FundoDeInvestimento` e subclasses |
| Lei 14.478/2022 | Criptoativos | `AtivoVirtual`, `PrestadoraDeServicosDeAtivosVirtuais` |
| Lei 13.709/2018 (LGPD) | Proteção de dados | `Consentimento` |

## Ferramentas Recomendadas

- **Protégé**: Editor de ontologias OWL
- **Apache Jena**: Framework RDF/SPARQL
- **RDFLib**: Biblioteca Python para RDF
- **GraphDB / Blazegraph**: Triple stores
- **HermiT / ELK**: Reasoners para verificação de consistência
- **ROBOT**: Automação de validação — é o que o CI roda
  (`.github/workflows/validate.yml`); ver [CONTRIBUTING.md](../CONTRIBUTING.md)
  para os comandos do ciclo curto local

## Referências

- [FIBO - Financial Industry Business Ontology](https://spec.edmcouncil.org/fibo/)
- [Open Finance Brasil](https://openfinancebrasil.org.br/)
- [BCB Dados Abertos](https://dadosabertos.bcb.gov.br/)
- [CVM Dados Abertos](https://dados.cvm.gov.br/)
- [Drex - Real Digital](https://www.bcb.gov.br/estabilidadefinanceira/drex)
- [GLEIF - Legal Entity Identifier](https://www.gleif.org/)

## Licença

Creative Commons Attribution 4.0 International (CC BY 4.0) — ver [LICENSE](../LICENSE).

## Contribuição

Contribuições são bem-vindas! Consulte [CONTRIBUTING.md](../CONTRIBUTING.md)
e abra issues ou pull requests no repositório.

---

**Versão**: 2.1.0
**Data**: Agosto 2026
