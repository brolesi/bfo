# Registro do IRI persistente `w3id.org/bfo-br`

A v2.0.0 move o namespace de `https://ontology.brasil.gov.br/bfo#` — um
domínio do gov.br que o projeto não controla e que nunca dereferenciou —
para `https://w3id.org/bfo-br#`.

> **Por que `bfo-br` e não `bfo`:** a sigla BFO já é ocupada mundialmente pela
> *Basic Formal Ontology*. O próprio README avisa sobre a confusão. Pedir o
> caminho `w3id.org/bfo` provocaria a colisão em cima de um identificador
> que, por definição, é permanente e não pode ser trocado depois.

## Como registrar

1. Fork de [`perma-id/w3id.org`](https://github.com/perma-id/w3id.org).
2. Copiar o diretório `bfo-br/` deste repositório para a raiz do fork.
3. Ajustar o destino do redirecionamento em `bfo-br/.htaccess` se o
   GitHub Pages do projeto não estiver em `brolesi.github.io/bfo`.
4. Abrir o PR. O registro é revisado manualmente pelo W3C Permanent
   Identifier Community Group.

## Enquanto o PR não é aceito

Os IRIs `https://w3id.org/bfo-br#...` não resolvem. Isso **não** quebra nada:
o `catalog-v001.xml` resolve tudo localmente no Protégé, e o CI trabalha
sobre os arquivos do repositório. Dereferenciação é conveniência para
terceiros, não requisito de funcionamento.

## Publicar os arquivos

O redirecionamento aponta para o GitHub Pages do projeto. Habilite Pages
(Settings → Pages → branch `main`, pasta `/`) para que
`https://brolesi.github.io/bfo/ontology/bfo-core.owl` sirva o arquivo.
