# Tech Challenge Fase 2 - Classificação da Qualidade de Vinhos

Projeto da disciplina de Data Analytics (POSTECH), Fase 2. O objetivo é treinar e avaliar
modelos de classificação capazes de prever se um vinho é de alta qualidade a partir de suas
características físico-químicas.

---

## 1. Identificação

| Campo | Valor |
|---|---|
| Turma | 14DTAT |
| Grupo | Eduarda Fernandes e Vitor Campos |
| Data de entrega | 01/09/2026 |

### Integrantes

| Nome completo | RM |
|---|---|
| Eduarda Fernandes da Silva | 374470 |
| Vitor Campos da Silva | 374469 |

---

## 2. Links da entrega

| Item | Link |
|---|---|
| Repositório |[Wine-Quality-Tech-Challenge-Fase-2](https://github.com/fndexs/Wine-Quality-Tech-Challenge-Fase-2)|
| Vídeo executivo (até 5 min) | [Vídeo Executivo ](https://canva.link/lx6ho0phkmw852h)|
| Apresentação executiva |[Apresentação Executiva](https://canva.link/lx6ho0phkmw852h) |

---

## 3. O problema

A avaliação de qualidade de um vinho tradicionalmente é feita por especialistas através de
análise sensorial (aroma, sabor, acidez, equilíbrio), um processo que funciona mas é subjetivo
e não escala bem. A proposta deste case é usar dados físico-químicos, coletados durante o
próprio processo de produção, para treinar um modelo que ajude a prever a qualidade final do
vinho e sirva de apoio à decisão para enólogos e produtores.

### Variável alvo

A nota original de qualidade (`quality`) foi transformada em um problema de classificação
binária:

- **Alta qualidade** → `quality >= 7`
- **Baixa ou média qualidade** → `quality < 7`

### Dataset

| Campo | Valor |
|---|---|
| Fonte | Wine Quality Dataset (Kaggle), arquivo `WineQT.csv` |
| Linhas × colunas | 1143 × 13 (antes da remoção de duplicados) |
| Licença de uso | Ver página do dataset no Kaggle |

Descrição das variáveis:

| Variável | Tipo | Descrição |
|---|---|---|
| `fixed acidity` | numérica | Acidez fixa (ácidos não voláteis do vinho) |
| `volatile acidity` | numérica | Acidez volátil; em excesso, indica gosto de vinagre |
| `citric acid` | numérica | Ácido cítrico; contribui para frescor e equilíbrio |
| `residual sugar` | numérica | Açúcar residual após a fermentação |
| `chlorides` | numérica | Teor de sal (cloretos) no vinho |
| `free sulfur dioxide` | numérica | SO2 livre, disponível para proteger o vinho da oxidação |
| `total sulfur dioxide` | numérica | SO2 total (livre + combinado) |
| `density` | numérica | Densidade do vinho, relacionada ao teor de álcool e açúcar |
| `pH` | numérica | Nível de acidez/alcalinidade |
| `sulphates` | numérica | Sulfatos; atuam como antioxidante e conservante |
| `alcohol` | numérica | Teor alcoólico (% em volume) |
| `quality` | numérica (alvo original) | Nota de qualidade atribuída por especialistas (0–10) |
| `Id` | identificador | Identificador da amostra, não utilizado como feature |

---

## 4. Como reproduzir

```
git clone <URL_DO_REPOSITORIO>
cd wine-quality-classification

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook
```

Baixe o arquivo `WineQT.csv` do Kaggle e coloque em `data/raw/` (os dados não são versionados
no Git, ver `data/README.md`).

Depois execute os notebooks nesta ordem:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `notebooks/01_eda.ipynb` | Contexto do problema, entendimento inicial da base, criação do alvo binário e análise exploratória |
| 2 | `notebooks/02_preprocessamento.ipynb` | Verificação de nulos, decisão de normalização e feature engineering |
| 3 | `notebooks/03_modelagem.ipynb` | Divisão treino/teste e treino dos três modelos |
| 4 | `notebooks/04_avaliacao.ipynb` | Métricas, curvas, importância de variáveis, validação cruzada e conclusões |

Cada notebook salva um arquivo que o seguinte usa como entrada (ver `notebooks/README.md` para
detalhes do encadeamento). Rodando no Google Colab, cada um pede o upload do arquivo gerado
pelo anterior quando não encontra ele já na sessão.

**Semente fixa:** `RANDOM_STATE = 42`, definida na primeira célula de código de cada notebook.
Rodar os quatro na ordem acima, a partir de um ambiente limpo, deve reproduzir os mesmos
números da seção 5.

---

## 5. Resultados

Base final após remoção de duplicados: 1018 amostras (de 1143 originais, foram descartadas
125 linhas duplicadas ignorando o `Id`). A classe de interesse é bem minoritária: 137 vinhos de
alta qualidade contra 881 de baixa/média, ou seja, cerca de 13,5% da base.

Divisão treino/teste de 75/25, estratificada pela classe, com `RANDOM_STATE = 42`.

| Modelo | Acurácia | Precisão | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Regressão Logística | 0,796 | 0,371 | 0,765 | 0,500 | 0,893 |
| Random Forest | 0,902 | 0,800 | 0,353 | 0,490 | 0,916 |
| Gradient Boosting | 0,894 | 0,667 | 0,412 | 0,509 | 0,904 |

Validação cruzada do modelo final (5 folds):

| Métrica | Média | Desvio padrão |
|---|---|---|
| Acurácia | 0,883 | 0,017 |
| Acurácia balanceada | 0,677 | 0,061 |
| F1 | 0,469 | 0,100 |
| Precisão | 0,597 | 0,077 |
| Recall | 0,396 | 0,124 |
| ROC-AUC | 0,872 | 0,029 |

**Modelo escolhido:** Gradient Boosting, por ter o melhor F1 na classe de alta qualidade no
conjunto de teste (0,509), critério que o notebook usa para escolher `best_model_name`
automaticamente.

Existe um trade-off claro entre os três: Random Forest tem a maior precisão (quando prevê
"alta qualidade", acerta 80% das vezes) mas erra a maior parte dos vinhos bons de fato (recall
de só 0,353). A Regressão Logística é o oposto: captura 76,5% dos vinhos realmente bons, mas
com bastante falso positivo. Gradient Boosting fica no meio do caminho entre os dois, o que
explica o F1 mais alto. Qual desses trade-offs importa mais depende de como o modelo seria
usado na prática: se o custo de deixar passar um vinho bom for mais alto que o custo de marcar
um vinho mediano como "promissor", a Regressão Logística pode ser preferível mesmo com F1 menor.

**Por que não usar só acurácia:** com a classe de interesse representando 13,5% da base, um
modelo que sempre prevê "baixa/média qualidade" já acertaria cerca de 86% das vezes sem
identificar nenhum vinho bom. Acurácia balanceada, recall, F1 e ROC-AUC dão uma leitura mais
honesta do que a acurácia simples nesse cenário.

**Variáveis mais relevantes:** tanto pela importância do Random Forest quanto pela importância
por permutação do modelo final, os fatores que mais pesam na previsão são a razão entre álcool
e densidade, o teor alcoólico isolado, os sulfatos e a acidez volátil. A ordem varia um
pouco entre os dois métodos, mas esse conjunto de quatro se repete no topo.

---

## 6. Principais conclusões

1. A razão entre álcool e densidade, o teor alcoólico, os sulfatos e a acidez volátil são os
   fatores que mais pesam na previsão de qualidade nesta base, o que é coerente com o que já
   se sabe sobre esses parâmetros na produção de vinho tinto.
2. Nenhum dos três modelos testados teve F1 acima de 0,51, o que mostra que o problema é difícil
   de resolver só com as variáveis físico-químicas disponíveis: existe uma parcela da qualidade
   sensorial (aroma, complexidade, harmonia) que provavelmente não está capturada nesses dados.
3. Gradient Boosting foi o modelo com melhor equilíbrio entre precisão e recall (F1 de 0,509),
   mas a diferença para a Regressão Logística (F1 de 0,500) é pequena o suficiente para que a
   escolha final dependa também de qual erro custa mais caro no processo: deixar passar um
   vinho bom ou sinalizar um vinho mediano como promissor.
4. O modelo deve ser tratado como ferramenta de apoio à decisão durante o processo produtivo,
   sinalizando lotes com perfil físico-químico fora do padrão de vinhos bem avaliados, e não
   como substituto da avaliação sensorial feita por especialistas.

### Limitações e próximos passos

- A classe de alta qualidade representa só 13,5% da base (137 de 1018 amostras), e isso limita
  o quanto qualquer modelo consegue aprender sobre esse grupo; mais dados de vinhos bem
  avaliados ajudariam a reduzir a variância das métricas entre folds.
- O corte fixo em `quality >= 7` é uma simplificação; um próximo passo seria testar a
  classificação ordinal (prever a nota original) ou ajustar o threshold de decisão conforme o
  custo de falso positivo/negativo for definido pela produção.
- Este trabalho usou apenas a variante de vinho tinto; testar a base de vinho branco é um
  próximo passo natural para ver se o mesmo conjunto de variáveis se mantém relevante.

---

## 7. Estrutura do repositório

```
wine-quality-classification/
│
├── data/
│   ├── README.md            instruções para baixar o dataset
│   ├── raw/                 vazio no Git, dados brutos não versionados
│   └── processed/           vazio no Git, gerado pelos notebooks 01 e 02
│
├── notebooks/
│   ├── README.md            ordem de execução e encadeamento
│   ├── 01_eda.ipynb
│   ├── 02_preprocessamento.ipynb
│   ├── 03_modelagem.ipynb
│   └── 04_avaliacao.ipynb
│
├── src/                      reservado para scripts auxiliares, se necessário
├── results/                  gráficos, tabelas de métricas e modelo final
├── submissao/                PDF de submissão com os três links
├── requirements.txt          bibliotecas utilizadas, com versões
└── README.md                 este arquivo
```

---

## 8. Tecnologias

- Python 3
- pandas, numpy
- matplotlib, seaborn
- scikit-learn
- joblib
- Jupyter Notebook / Google Colab

