# Changelog

Todas as mudanças relevantes deste projeto são documentadas neste arquivo.

O formato segue o [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/)
e o projeto adota [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [2.1.0] - 2026-08-28

Expansão de cobertura: 89 para 184 classes, 23 para 64 object properties, 27
para 48 data properties, 41 para 71 indivíduos. Nenhum termo removido ou
renomeado; uma propriedade depreciada.

### Adicionado

- **Seguros, resseguro e capitalização** (SUSEP): `Seguradora`,
  `Resseguradora`, `CorretoraDeSeguros`, `SociedadeDeCapitalizacao`,
  `EntidadeAbertaDePrevidenciaComplementar`, `ApoliceDeSeguro`, `Sinistro`,
  `PremioDeSeguro`, `TituloDeCapitalizacao`. A SUSEP estava modelada como
  órgão regulador desde a v1.0.0 sem que nada do que ela regula existisse.
  `planoOperadoPor` liga PGBL e VGBL à EAPC operadora.
- **Previdência complementar fechada** (PREVIC), na mesma situação:
  `EntidadeFechadaDePrevidenciaComplementar`, `PlanoDeBeneficios` nas
  modalidades BD, CD e CV, `Patrocinador`, `InstituidorDePlano`,
  `Participante`, `Assistido`, `codigoCNPB`.
- **Infraestruturas do mercado**: `CamaraDeCompensacao`,
  `DepositariaCentral`, `SistemaDePagamentos`, `SistemaDeRegistro`, com os
  indivíduos `SPB`, `STR`, `SistemaSelic`, `SPI`, `DICT`, `Nuclea` e `SCR`.
  A classe `InfraestruturaDoMercadoFinanceiro` tinha uma única instância.
- **Arranjos de pagamento e cartões** (Lei 12.865/2013, já citada na
  documentação sem que nenhum papel da cadeia existisse):
  `ArranjoDePagamento`, `InstituidorDeArranjo`, `Credenciadora`,
  `Subcredenciadora`, `EmissorDeInstrumentoDePagamento`, `BandeiraDeCartao`,
  `CartaoDePagamento` e subtipos, `ContaDeCartao`.
- **PIX detalhado**: modalidades `PIXAutomatico`, `PIXCobranca`, `PIXSaque`
  e `PIXTroco`; `TipoChavePIX` como enumeração fechada dos cinco tipos;
  `codigoQRPIX` e `linhaDigitavel`.
- **Consórcio** (Lei 11.795/2008): `AdministradoraDeConsorcio`,
  `GrupoDeConsorcio`, `CotaDeConsorcio`, `Contemplacao`, `Lance`,
  `taxaAdministracao`, `prazoGrupoMeses`.
- **Títulos de crédito**: `CCB`, `CCI`, `CPR`, `CDCA`, `WarrantAgropecuario`,
  `DuplicataEscritural`, `NotaComercial`.
- **Ofertas públicas** (Resolução CVM 160/2022): `OfertaPublica`,
  `OfertaPublicaInicial`, `OfertaSubsequente`, `Prospecto`,
  `CoordenadorLider`, `RitoDeRegistro` enumerado.
- **Eventos societários**: `EventoSocietario`, `Dividendo`,
  `JurosSobreCapitalProprio`, `Bonificacao`, `Desdobramento`, `Grupamento`,
  `Subscricao`, `RecompraDeAcoes`, `AssembleiaGeral`, `dataCom`, `dataEx`, e
  `NivelGovernancaB3` com os quatro segmentos de listagem.
- **PLD/FT** (Circular BCB 3.978/2020): `UnidadeDeInteligenciaFinanceira`
  com o indivíduo `COAF`, `ComunicacaoDeOperacaoSuspeita`,
  `PessoaExpostaPoliticamente`, `AvaliacaoKYC`, `beneficiarioFinal`.
- **Câmbio** (Lei 14.286/2021): `ContratoDeCambio`, `CambioPronto`,
  `CambioFuturo`, `ACC`, `ACE`, `ContaEmMoedaEstrangeira`, `taxaCambio`.
- **Crédito adicional**: `ChequeEspecial`, `CapitalDeGiro`,
  `AntecipacaoDeRecebiveis`, `CreditoRural`, `Microcredito`,
  `CreditoEstudantil`.
- **Rating**: `AgenciaDeClassificacaoDeRisco` e `RatingDeCredito`, distintos
  de `ClassificacaoRiscoCredito`, que continua sendo o enquadramento
  obrigatório da operação na escala AA a H.
- **Tributação**: `aliquota`, `prazoMinimoDias`, `prazoMaximoDias`,
  `limiteIsencaoMensal`, e os indivíduos `ComeCotas`, `IOF_Cambio`,
  `IOF_Credito`, `IOF_Seguro`, `IOF_Titulos`, `IR_GanhoCapital`,
  `IR_FonteRendaFixa`. A classe `Tributo` tinha três instâncias e nenhuma
  alíquota.
- **Fundos e sustentabilidade**: `Fiagro` (Lei 14.130/2021),
  `TituloTematicoSustentavel` com `TituloVerde` e `TituloSocial`.
- **Ativos virtuais**: `Stablecoin`, `TokenDeRecebivel`.
- **Fundamentos estruturais**: `Moeda` com os indivíduos `BRL`, `USD` e
  `EUR`; `Contrato`; `CalendarioBancario` e `FeriadoBancario`;
  `dataInicioVigencia` e `dataFimVigencia`; `prazoLiquidacaoDiasUteis`.
- Alinhamentos a doze novos módulos FIBO, incluindo o domínio CAE, agora
  importado pelo módulo de alinhamento. Todos verificados contra um clone da
  FIBO antes do commit: existem, têm rótulo e não estão depreciados.

### Modificado

- `incideTributo` teve o domínio ampliado de `OperacaoFinanceira` para a
  união com `EventoSocietario`. Provento de evento societário também é
  tributado — o JCP é retido na fonte a quinze por cento — e, sem a união,
  afirmar o tributo sobre um JCP o inferiria como operação financeira.
- Nova `emitenteDoTitulo` (domínio `TituloDeCredito`, imagem `Pessoa`),
  distinta de `emitidoPor`, cuja imagem é `PessoaJuridica` por se destinar a
  valores mobiliários. Título de crédito é emitido também por pessoa física:
  a CCB de um consignado e a CPR de um produtor rural são os casos
  correntes. Usar `emitidoPor` nesses casos tornava a ontologia
  inconsistente, por colidir com a disjunção entre `PessoaFisica` e
  `PessoaJuridica`.

### Descontinuado

- `valorBRL` marcada `owl:deprecated`. A denominação estava embutida no nome
  da propriedade, o que torna impossível representar contrato de câmbio e
  conta em moeda estrangeira. Substituída por `valor` mais `moeda`, esta
  apontando para um indivíduo de `Moeda` identificado por ISO 4217. Conforme
  a política de versionamento, o termo permanece publicado.

## [2.0.0] - 2026-08-27

Versão MAJOR: o namespace mudou e 35 IRIs de alinhamento externo foram
repontadas. Nenhum termo `bfo:` foi removido ou renomeado.

### Corrigido

- **Os alinhamentos FIBO apontavam para IRIs inexistentes.** As 35
  referências usavam um padrão achatado (`…/fibo/ontology/SEC/Share`) que a
  FIBO não usa — as IRIs reais são hierárquicas
  (`…/fibo/ontology/SEC/Equities/EquityInstruments/Share`). Como OWL opera
  em mundo aberto, uma IRI inexistente é apenas uma classe não declarada:
  nenhum reasoner reclamava e o alinhamento nunca existiu de fato. Todas as
  35 foram resolvidas contra a FIBO real; 6 alinhamentos sem contrapartida
  na FIBO (`isRegulatedBy`, `holdsAccount`, `hasLegalEntityIdentifier`,
  `hasISIN`) foram removidos.
  - Vários termos mudaram de domínio: `FinancialInstrument` está em FBC (não
    FND), `Swap` em DER, `CertificateOfDeposit` em FBC.
  - `Party`, `LegalEntity` e `RegulatoryAgency` migraram da FIBO para o
    OMG Commons (`cmns-pts`, `cmns-org`, `cmns-rga`).
  - `FundoDeInvestimento` aponta para
    `SEC/Securities/Pools/CollectiveInvestmentVehicle`. A homônima em
    `SEC/Funds/Funds/` existe, mas é um alias `owl:deprecated` da primeira.
- **Os `owl:imports` da FIBO não resolviam.** Apontavam para URLs de
  diretório (`…/ontology/FND/`) em vez de documentos de ontologia. Foram
  movidos para o novo módulo opt-in `bfo-fibo-alignment.owl` e corrigidos
  para `…/FND/AllFND/` etc.
- `dcterms:creator` dizia "Brazilian Financial Ontology Consortium",
  divergindo do `CITATION.cff`. Agora consistente.

### Modificado

- **Namespace**: `https://ontology.brasil.gov.br/bfo#` →
  `https://w3id.org/bfo-br#`. O domínio anterior é do gov.br, não é
  controlado pelo projeto e nunca dereferenciou. `bfo-br` e não `bfo` porque
  a sigla BFO pertence à *Basic Formal Ontology*, e um IRI persistente não
  pode ser trocado depois. Ver [w3id/README.md](w3id/README.md).
  - Migração: substitua o prefixo antigo pelo novo. Os nomes locais dos
    termos são idênticos.

### Adicionado

- **CI** (`.github/workflows/validate.yml`): perfil OWL 2 DL, reasoner
  HermiT, `robot report`, cobertura de anotações, e um job que baixa a FIBO
  para provar que os imports resolvem e que cada IRI alinhada existe. Era
  tudo manual — o `CONTRIBUTING.md` pedia, nada verificava.
- **Axiomas de disjunção**: 20 `owl:AllDisjointClasses`. A ontologia tinha
  zero, o que tornava a verificação de consistência vacuosa: sem disjunção
  nenhuma inconsistência é detectável.
- **Enumerações fechadas** (`owl:oneOf` + `owl:AllDifferent`) para
  `SegmentacaoPrudencial`, `ClassificacaoInvestidor`,
  `ClassificacaoRiscoCredito` e `StatusConsentimento`.
- **Chaves naturais**: `owl:hasKey` em `PessoaFisica` (CPF),
  `PessoaJuridica` (CNPJ), `InstituicaoFinanceiraBrasileira` (ISPB),
  `InstrumentoFinanceiro` (ISIN) e `IndicadorEconomico` (código SGS), mais
  `owl:FunctionalProperty` nos identificadores correspondentes.
- **Inversas nomeadas**: `emite`, `regula`, `realiza`, `custodia`,
  `administra`.
- **48 `skos:definition` ausentes**, incluindo quase todas as object
  properties e os nove níveis de risco da Resolução CMN 2.682/1999. A
  anotação era declarada obrigatória pelo `CONTRIBUTING.md` e agora é
  verificada no CI.
- `ontology/bfo-fibo-alignment.owl`, `queries/`, `w3id/`.

### Removido

- `.vscode/settings.json` saiu do versionamento. Continha configuração de
  conda/Python num repositório sem uma linha de Python.
- `bfo.owl` saiu da raiz para `legacy/brazilian-investment.owl`. É uma
  ontologia de 2021, com namespace próprio
  (`semanticweb.org/eyujis/…/brazilian-investment`), sem anotações e
  sobrepondo conceitos do núcleo (`CDB`, `Debenture`, `Banco`, `Corretora`).
  Já estava fora do versionamento; na raiz, só confundia quem clonava.
  Ver [legacy/README.md](legacy/README.md).

### CI

- O `robot report` deixou de bloquear o merge e virou artefato informativo,
  com as primeiras linhas impressas no resumo do job. O perfil padrão dele é
  feito para ontologias OBO: cobra definição em `IAO:0000115` (aqui se usa
  `skos:definition`, verificada pela nossa própria consulta) e trata rótulo
  bilíngue como erro, sendo que anotação bilíngue é uma característica
  declarada do projeto. Das 39 violações de nível ERROR do primeiro run,
  nenhuma indicava defeito real de modelagem além dos rótulos duplicados
  abaixo. As regras que valem aqui viraram consultas em `queries/`, onde o
  critério é explícito e o nome não depende da versão do ROBOT instalada.
- Nova consulta `queries/rotulos-duplicados.rq`, que substitui a regra
  `duplicate_label` excluindo os pares ligados por `owl:equivalentClass` —
  um stub externo equivalente a um termo BFO denota a mesma classe, então
  compartilhar rótulo não é erro.
- Os 36 stubs `&ofb;` e `&bcb;` passaram a ter rótulo em inglês, com o nome
  do termo no schema de origem. Rotulá-los em pt-BR com a tradução do termo
  BFO equivalente duplicava rótulos entre entidades distintas.
- `queries/alinhamentos-fantasma.rq` passou a rejeitar também alvos marcados
  `owl:deprecated` na FIBO, não só os inexistentes. Foi ela que pegou o
  `CollectiveInvestmentVehicle` abaixo — alinhar com termo deprecado
  apodrece silenciosamente na próxima release da FIBO.

- **213 literais de anotação normalizados para uma única linha.** Anotações
  escritas em várias linhas indentadas geravam literais começando com
  `"\n            "` — o rótulo da `bfo:ANBIMA` era um deles, e ia inteiro,
  com quebras e indentação, para quem consultasse via SPARQL. Atinge
  `rdfs:label`, `skos:definition`, `rdfs:comment` e `dcterms:description`.
- Actions do CI atualizadas (`checkout@v7`, `setup-java@v6`,
  `upload-artifact@v7`); as anteriores rodavam em Node 20, já depreciado.
- O resumo do job agora imprime a contagem de violações por regra e **todas**
  as linhas de nível ERROR, não as 40 primeiras do arquivo.

### Documentação

- Todo o texto do projeto passou a ser em português: `README.md` foi
  traduzido, e a `message` e o `abstract` do `CITATION.cff` também.
- A duplicação entre `README.md` e `docs/README.md` foi resolvida: a tabela
  completa de prefixos vive só em `docs/`, e o `README.md` aponta para lá.

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
