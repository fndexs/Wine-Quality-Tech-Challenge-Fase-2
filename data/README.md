# Dados

O dataset usado neste projeto é o Wine Quality Dataset, na variante `WineQT.csv`, disponível
publicamente no Kaggle. Nenhum arquivo de dados é versionado neste repositório, para manter o
tamanho do Git baixo e respeitar a licença de distribuição da fonte.

## `raw/`

Onde entra o arquivo baixado direto do Kaggle, sem nenhuma alteração. Se for rodar localmente,
baixe `WineQT.csv` e coloque em `data/raw/WineQT.csv`. Se estiver rodando no Google Colab, o
notebook `01_eda.ipynb` pede o upload direto quando chega nessa etapa.

## `processed/`

Onde os notebooks salvam os arquivos intermediários gerados durante o pipeline
(`wine_quality_eda.csv` pelo notebook 01, `wine_quality_features.csv` pelo notebook 02). Também
não é versionado, já que é totalmente reproduzível rodando os notebooks na ordem.

## Sobre o arquivo original

- 1143 linhas, 13 colunas (11 variáveis físico-químicas + `quality` + `Id`).
- Sem necessidade de conversão de formato, é lido direto com `pandas.read_csv`.
- A coluna `Id` é apenas identificador da amostra e não é usada como variável preditiva.
