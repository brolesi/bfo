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
│   ├── bfo-core.owl              # Ontologia principal
│   ├── bfo-openfinance-mapping.owl   # Mapeamento Open Finance Brasil
│   └── bfo-bcb-mapping.owl       # Mapeamento BCB/SGS
├── examples/
│   └── bfo-examples.owl          # Exemplos de instâncias
└── docs/
    └── README.md                 # Esta documentação
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

## Classes Principais

### 1. Agentes Financeiros

| Classe BFO | Superclasse FIBO | Descrição |
|------------|------------------|-----------|
| `bfo:OrgaoReguladorBrasileiro` | `fibo-fbc:RegulatoryAgency` | BCB, CVM, SUSEP, PREVIC |
| `bfo:InstituicaoFinanceiraBrasileira` | `fibo-fbc:FinancialServiceProvider` | Bancos, cooperativas, fintechs |
| `bfo:BancoComercial` | `fibo-fbc:DepositoryInstitution` | Banco que capta depósitos à vista |
| `bfo:BancoMultiplo` | - | Opera com múltiplas carteiras |
| `bfo:InstituicaoDePagamento` | - | IPs reguladas pela Lei 12.865/2013 |

### 2. Pessoas (Parties)

| Classe BFO | Equivalente Open Finance | Descrição |
|------------|-------------------------|-----------|
| `bfo:PessoaFisica` | `ofb:NaturalPerson` | Identificada por CPF |
| `bfo:PessoaJuridica` | `ofb:BusinessEntity` | Identificada por CNPJ |

### 3. Instrumentos Financeiros

#### Renda Fixa
| Classe BFO | Superclasse FIBO | Características |
|------------|------------------|-----------------|
| `bfo:TituloPublicoFederal` | `fibo-sec:GovernmentDebtSecurity` | Tesouro Direto |
| `bfo:TesouroSelic` | - | Pós-fixado, indexado à Selic |
| `bfo:TesouroIPCA` | - | Híbrido, IPCA + taxa |
| `bfo:CDB` | `fibo-sec:CertificateOfDeposit` | Cobertura FGC até R$ 250k |
| `bfo:LCI` / `bfo:LCA` | - | Isentos de IR para PF |
| `bfo:Debenture` | `fibo-sec:CorporateBond` | Sem cobertura FGC |

#### Renda Variável
| Classe BFO | Superclasse FIBO | Características |
|------------|------------------|-----------------|
| `bfo:AcaoOrdinaria` | `fibo-sec:CommonShare` | ON - direito a voto |
| `bfo:AcaoPreferencial` | `fibo-sec:PreferredShare` | PN - preferência em dividendos |
| `bfo:FundoImobiliario` | - | FII - rendimentos isentos* |
| `bfo:ETF` | `fibo-sec:ExchangeTradedFund` | Replica índice |
| `bfo:BDR` | `fibo-sec:DepositaryReceipt` | Ações estrangeiras |

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
| `bfo:OperacaoPagamento` | `fibo-fbc:PaymentTransaction` | PIX, TED, Boleto |
| `bfo:OperacaoInvestimento` | - | Aplicação, Resgate |

## Propriedades Principais

### Object Properties (Relações)

```turtle
bfo:reguladoPor       → OrgaoRegulador
bfo:possuiConta       → Conta
bfo:realizadaPor      → Pessoa
bfo:emitidoPor        → PessoaJuridica
bfo:indexadoPor       → IndicadorEconomico
bfo:classificacaoRisco → ClassificacaoRiscoCredito
```

### Data Properties (Atributos)

```turtle
# Identificadores
bfo:cpf               → xsd:string (11 dígitos)
bfo:cnpj              → xsd:string (14 dígitos)
bfo:codigoISPB        → xsd:string (8 dígitos)
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

# Booleanos
bfo:coberturaFGC      → xsd:boolean
bfo:isentoIRPF        → xsd:boolean
```

## Alinhamento FIBO

A BFO está alinhada com os seguintes módulos do FIBO:

| Módulo FIBO | Classes Utilizadas |
|-------------|-------------------|
| **FND** (Foundations) | Party, Transaction, FinancialInstrument, Index |
| **BE** (Business Entities) | NaturalPerson, LegalEntity |
| **FBC** (Financial Business & Commerce) | RegulatoryAgency, FinancialServiceProvider, Account |
| **SEC** (Securities) | DebtInstrument, EquityInstrument, Derivative |

## Integração Open Finance Brasil

Mapeamentos diretos com as APIs do Open Finance:

| Fase | APIs | Classes BFO |
|------|------|-------------|
| 1 | Produtos, Canais | InstituicaoFinanceiraBrasileira |
| 2 | Contas, Clientes | Conta, PessoaFisica, PessoaJuridica |
| 3 | Pagamentos | PIX, OperacaoPagamento |
| 4 | Investimentos | InstrumentoRendaFixa, InstrumentoRendaVariavel |

## Integração BCB

Mapeamentos com dados abertos do Banco Central:

| Sistema BCB | Classes/Propriedades BFO |
|-------------|-------------------------|
| **SGS** (Séries Temporais) | IndicadorEconomico (Selic, CDI, IPCA) |
| **IF.data** | InstituicaoFinanceiraBrasileira, SegmentacaoPrudencial |
| **SCR** | OperacaoCredito, ClassificacaoRiscoCredito |
| **DICT** | chavePIX |

## Exemplos de Uso

### SPARQL - Buscar CDBs com FGC

```sparql
PREFIX bfo: <https://ontology.brasil.gov.br/bfo#>

SELECT ?cdb ?emissor ?taxa WHERE {
  ?cdb a bfo:CDB ;
       bfo:emitidoPor ?emissor ;
       bfo:taxaJuros ?taxa ;
       bfo:coberturaFGC true .
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

## Ferramentas Recomendadas

- **Protégé**: Editor de ontologias OWL
- **Apache Jena**: Framework RDF/SPARQL
- **RDFLib**: Biblioteca Python para RDF
- **GraphDB / Blazegraph**: Triple stores

## Referências

- [FIBO - Financial Industry Business Ontology](https://spec.edmcouncil.org/fibo/)
- [Open Finance Brasil](https://openfinancebrasil.org.br/)
- [BCB Dados Abertos](https://dadosabertos.bcb.gov.br/)
- [CVM Dados Abertos](https://dados.cvm.gov.br/)

## Licença

Creative Commons Attribution 4.0 International (CC BY 4.0)

## Contribuição

Contribuições são bem-vindas! Por favor, abra issues ou pull requests no repositório.

---

**Versão**: 1.0.0  
**Data**: Janeiro 2026
