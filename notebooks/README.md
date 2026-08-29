# Notebooks

O projeto está dividido em quatro notebooks, para rodar nesta ordem:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `01_eda.ipynb` | Contexto do problema, entendimento inicial da base, criação do alvo binário, análise exploratória (distribuições, correlações, outliers) |
| 2 | `02_preprocessamento.ipynb` | Verificação de nulos, decisão de normalização, feature engineering |
| 3 | `03_modelagem.ipynb` | Divisão treino/teste e treino dos três modelos |
| 4 | `04_avaliacao.ipynb` | Métricas, curvas, ajuste de threshold, importância de variáveis, validação cruzada e conclusões |

## Como o encadeamento funciona

Cada notebook salva um arquivo de saída que o próximo usa como entrada:

- `01_eda.ipynb` gera `data/processed/wine_quality_eda.csv`
- `02_preprocessamento.ipynb` gera `data/processed/wine_quality_features.csv`
- `03_modelagem.ipynb` gera `pacote_modelagem.zip` (modelos treinados + dados de treino e teste)

Rodando os quatro no Google Colab, cada um pede o upload do arquivo gerado pelo anterior quando
não encontra ele já na pasta (por exemplo, ao abrir uma sessão nova). Se todos forem executados
na mesma sessão do Colab, o upload nem chega a ser pedido, porque o arquivo já está no ambiente.

Os quatro notebooks já vêm com as saídas (gráficos, tabelas) visíveis, então dá pra ver os
resultados direto no GitHub sem precisar rodar nada. Para reproduzir do zero, é só seguir a
ordem da tabela acima.
