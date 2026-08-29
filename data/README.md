# Dados

O dataset usado neste projeto é o Wine Quality Dataset, na variante `WineQT.csv`, disponível
publicamente no Kaggle. O arquivo não é versionado neste repositório para manter o tamanho do
Git baixo e respeitar a licença de distribuição da fonte.

## Como obter

1. Baixe o arquivo `WineQT.csv` diretamente da página do dataset no Kaggle.
2. Coloque o arquivo nesta pasta (`data/WineQT.csv`) se for rodar localmente, ou faça o upload
   quando o notebook pedir, se estiver rodando no Google Colab.

## Sobre o arquivo

- 1143 linhas, 13 colunas (11 variáveis físico-químicas + `quality` + `Id`).
- Sem necessidade de conversão de formato — é lido diretamente com `pandas.read_csv`.
- A coluna `Id` é apenas identificador da amostra e não é usada como variável preditiva.

