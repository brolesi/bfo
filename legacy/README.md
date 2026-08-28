# Ontologia legada

Este diretório guarda `brazilian-investment.owl` (antes `bfo.owl`, na raiz do
repositório): uma ontologia anterior, de 2021, com o namespace
`http://www.semanticweb.org/eyujis/ontologies/2021/8/brazilian-investment#`.

**Não faz parte da distribuição da BFO** e não é versionada — está no
`.gitignore` e existe apenas na cópia local de quem já a tinha.

## Por que está aqui e não na raiz

Ela sobrepõe conceitos do núcleo (`CDB`, `Debenture`, `Banco`, `Corretora`)
com IRIs completamente diferentes e sem nenhuma anotação. Na raiz, ao lado do
`ontology/`, ela confundia quem clonava o repositório sobre qual era a
ontologia de verdade.

## Se você precisa dela

O conteúdo está preservado como estava. Ela não é carregada por nenhum
módulo, não aparece no `catalog-v001.xml` e não é validada pelo CI. Nada na
BFO 2.0.0 depende dela.
