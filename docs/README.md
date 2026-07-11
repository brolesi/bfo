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
│   └── catalog-v001.xml              # Catálogo XML (resolução local no Protégé)
├── examples/
│   └── bfo-examples.owl              # Exemplos de instâncias
└── docs/
    └── README.md                     # Esta documentação
```

## Namespaces

| Prefixo | URI | Descrição |
|---------|-----|-----------|
| `bfo` | `https://ontology.brasil.gov.br/bfo#` | Brazilian Financial Ontology |
| `fibo-fnd` | `https://spec.edmcouncil.org/fibo/ontology/FND/` | FIBO Foundations |
| `fibo-be` | `https://spec.edmcouncil.org/fibo/ontology/BE/` | FIBO Business Entities |
| `fibo-fbc` | `https://spec.edmcouncil.org/fibo/ontology/FBC/` | FIBO Financial Business & Commerce |
| `fibo-sec` | `https://spec.edmcouncil.org/fibo/ontology/SEC/` | FIBO Securities |
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
| `bfo:OrgaoReguladorBrasileiro` | `fibo-fbc:RegulatoryAgency` | BCB, CVM, SUSEP, PREVIC |
| `bfo:InstituicaoFinanceiraBrasileira` | `fibo-fbc:FinancialServiceProvider` | Bancos, cooperativas, fintechs |
| `bfo:BancoComercial` | `fibo-fbc:DepositoryInstitution` | Banco que capta depósitos à vista |
| `bfo:BancoMultiplo` | - | Opera com múltiplas carteiras |
| `bfo:InstituicaoDePagamento` | - | IPs reguladas pela Lei 12.865/2013 |
| `bfo:SociedadeDeCreditoDireto` | - | SCD - fintech de crédito (Res. CMN 4.656/2018) |
| `bfo:SociedadeDeEmprestimoEntrePessoas` | - | SEP - P2P lending (Res. CMN 4.656/2018) |
| `bfo:PrestadoraDeServicosDeAtivosVirtuais` | `fibo-fbc:FinancialServiceProvider` | PSAV/VASP (Lei 14.478/2022) |
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
| `bfo:TituloPublicoFederal` | `fibo-sec:GovernmentDebtSecurity` | Tesouro Direto |
| `bfo:TesouroSelic` | - | Pós-fixado, indexado à Selic |
| `bfo:TesouroIPCA` | - | Híbrido, IPCA + taxa |
| `bfo:CDB` | `fibo-sec:CertificateOfDeposit` | Cobertura FGC até R$ 250k |
| `bfo:LCI` / `bfo:LCA` | - | Isentos de IR para PF |
| `bfo:CRI` / `bfo:CRA` | - | Securitização, isentos de IR para PF |
| `bfo:Debenture` | `fibo-sec:CorporateBond` | Sem cobertura FGC |

#### Renda Variável
| Classe BFO | Superclasse FIBO | Características |
|------------|------------------|-----------------|
| `bfo:AcaoOrdinaria` | `fibo-sec:CommonShare` | ON - direito a voto |
| `bfo:AcaoPreferencial` | `fibo-sec:PreferredShare` | PN - preferência em dividendos |
| `bfo:FundoImobiliario` | - | FII - rendimentos isentos* |
| `bfo:ETF` | `fibo-sec:ExchangeTradedFund` | Replica índice |
| `bfo:BDR` | `fibo-sec:DepositaryReceipt` | Ações estrangeiras |

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
`bfo:Opcao`, `bfo:ContratoFuturo`, `bfo:Swap` — alinhados a `fibo-sec:Derivative`.

### 4. Contas

| Classe BFO | Equivalente Open Finance | Descrição |
|------------|-------------------------|-----------|
| `bfo:ContaCorrente` | `ofb:CheckingAccount` | Depósitos à vista |
| `bfo:ContaPoupanca` | `ofb:SavingsAccount` | Remuneração TR + 0,5% a.m. |
| `bfo:ContaPagamento` | `ofb:PaymentAccount` | Instituições de pagamento |

### 5. Operações

| Classe BFO | Superclasse FIBO | Exemplos |
|------------|------------------|----------|
| `bfo:OperacaoCredito` | `fibo-fbc:LoanTransaction` | Empréstimos, financiamentos |
| `bfo:OperacaoPagamento` | `fibo-fbc:PaymentTransaction` | PIX, TED, Boleto, Cartão de Crédito |
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
bfo:dataVencimento    → xsd:date
bfo:dataLiquidacao    → xsd:date
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
| **FND** (Foundations) | Party, Transaction, FinancialInstrument, Index |
| **BE** (Business Entities) | NaturalPerson, LegalEntity |
| **FBC** (Financial Business & Commerce) | RegulatoryAgency, FinancialServiceProvider, Account |
| **SEC** (Securities) | DebtInstrument, EquityInstrument, Derivative, Fund |

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

## Exemplos de Uso

### SPARQL - Buscar CDBs por emissor e taxa

```sparql
PREFIX bfo: <https://ontology.brasil.gov.br/bfo#>

SELECT ?cdb ?emissor ?taxa WHERE {
  ?cdb a bfo:CDB ;
       bfo:emitidoPor ?emissor ;
       bfo:taxaJuros ?taxa .
}
```

### SPARQL - Operações de um Cliente

```sparql
PREFIX bfo: <https://ontology.brasil.gov.br/bfo#>

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
PREFIX bfo: <https://ontology.brasil.gov.br/bfo#>

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

**Versão**: 1.1.0
**Data**: Julho 2026
