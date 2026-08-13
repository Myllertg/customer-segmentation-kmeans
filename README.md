# Credit Card Customer Segmentation using K-Means
Projeto de segmentação de clientes utilizando Machine Learning não supervisionado e o algoritmo K-Means.

[🇧🇷 Português](README.md) | [🇺🇸 English](README_EN.md)

## Sobre o Projeto

Este projeto utiliza **Machine Learning não supervisionado**, por meio do algoritmo **K-Means**, para identificar e interpretar diferentes perfis de clientes de cartão de crédito a partir de seus padrões de relacionamento com a instituição financeira.

O objetivo não foi prever uma variável específica, mas **descobrir grupos de clientes com comportamentos semelhantes** e transformar esses grupos em segmentos que possam apoiar decisões de negócio.

## Dataset

Foi utilizado o dataset [Credit Card Customer Data](https://www.kaggle.com/datasets/aryashah2k/credit-card-customer-data), disponibilizado no Kaggle.

A base possui **660 registros e 7 variáveis**:

- `Sl_No`
- `Customer Key`
- `Avg_Credit_Limit`
- `Total_Credit_Cards`
- `Total_visits_bank`
- `Total_visits_online`
- `Total_calls_made`

Para o clustering foram utilizadas cinco features comportamentais:

- `Avg_Credit_Limit`
- `Total_Credit_Cards`
- `Total_visits_bank`
- `Total_visits_online`
- `Total_calls_made`

`Sl_No` e `Customer Key` foram mantidos apenas como identificadores e não participaram do cálculo dos clusters.

## Metodologia

O projeto seguiu um fluxo de desenvolvimento de Machine Learning:

**Entendimento do problema → Coleta → Análise exploratória → Tratamento → Padronização → Definição do K → K-Means → Interpretação dos clusters → Operacionalização**

A análise inicial mostrou que não havia valores ausentes nem registros duplicados exatos.

Foram identificadas **5 Customer Keys repetidas**, totalizando 10 registros. Como os registros associados às mesmas chaves apresentavam características diferentes, não foram considerados duplicações exatas e foram mantidos na análise.

Também foram identificados possíveis outliers em `Avg_Credit_Limit` e `Total_visits_online`. Esses valores foram mantidos porque podem representar comportamentos reais e relevantes para a segmentação.

## Análise dos Dados

A análise exploratória revelou diferenças importantes entre os perfis de clientes. Entre as principais relações observadas estão:

- `Avg_Credit_Limit` × `Total_Credit_Cards`: correlação de **0,61**
- `Avg_Credit_Limit` × `Total_visits_online`: correlação de **0,55**
- `Total_Credit_Cards` × `Total_calls_made`: correlação de **-0,65**
- `Total_visits_bank` × `Total_calls_made`: correlação de **-0,51**

Esses resultados indicaram que os clientes apresentam diferentes padrões de utilização de crédito e de preferência pelos canais de relacionamento.

## Padronização

As cinco features possuem escalas bastante diferentes. O `Avg_Credit_Limit`, por exemplo, varia de **3.000 a 200.000**, enquanto as demais variáveis possuem escalas muito menores.

Como o K-Means utiliza distância para determinar a proximidade entre os clientes, foi aplicado o **StandardScaler** para colocar todas as features em uma escala comparável.

Após a transformação, as variáveis apresentaram média aproximadamente igual a 0 e desvio padrão aproximadamente igual a 1.

## Definição do Número de Clusters

Foram avaliados valores de **K entre 2 e 10**, utilizando o Método do Cotovelo e o Silhouette Score.

O melhor resultado foi obtido com:

**K = 3**

**Silhouette Score = 0,5157**

A configuração com três clusters apresentou o maior Silhouette Score e uma redução significativa da inércia em relação às configurações anteriores, indicando uma boa relação entre compactação e separação dos grupos.

## Resultado da Segmentação

O modelo identificou três segmentos:

| Cluster | Clientes | % da Base | Perfil |
|---|---:|---:|---|
| 0 | 386 | 58,48% | Relacionamento Presencial |
| 1 | 50 | 7,58% | Relacionamento Digital |
| 2 | 224 | 33,94% | Relacionamento Assistido |

### Relacionamento Presencial

Representa **58,48% da base**.

Caracteriza-se por maior utilização do banco físico, menor utilização do canal online e quantidade de cartões acima da média.

**Possíveis oportunidades:** incentivar a utilização de canais digitais, identificar operações que podem ser migradas para autosserviço e aproveitar interações presenciais para ampliar o relacionamento.

### Relacionamento Digital

Representa **7,58% da base**.

Apresenta o maior limite médio de crédito, maior quantidade de cartões e forte utilização do canal online, com baixa dependência do banco físico e do telefone.

**Possíveis oportunidades:** personalização de ofertas, cross-sell, up-sell, estratégias de retenção e relacionamento prioritariamente digital.

### Relacionamento Assistido

Representa **33,94% da base**.

Apresenta menor limite médio e menor quantidade de cartões, mas maior utilização do atendimento telefônico.

**Possíveis oportunidades:** investigar os motivos das ligações, identificar dificuldades nos canais digitais, estimular autosserviço e avaliar oportunidades de ampliação do relacionamento.

Os nomes dos segmentos representam **padrões comportamentais observados nos dados** e não significam que um grupo seja necessariamente melhor ou pior que outro.

## PCA e Visualização

Foi utilizado **PCA (Principal Component Analysis)** para visualizar os clusters em duas dimensões.

Os dois primeiros componentes explicaram:

- **PC1:** 45,74%
- **PC2:** 37,43%
- **Variância acumulada:** 83,16%

O PCA foi utilizado apenas para visualização. O K-Means continua baseado nas cinco features originais padronizadas.

## Modelo Operacional

Após a validação, o modelo foi transformado em um **Pipeline reutilizável**, combinando:

**StandardScaler → K-Means**

Essa estrutura permite utilizar posteriormente o mesmo processo para novos clientes.

Foram gerados dois principais artefatos:

- `segmentacao_clientes_kmeans.pkl`: modelo treinado para segmentação de novos clientes.
- `clientes_segmentados.csv`: base contendo os clientes, clusters e respectivos perfis.

Dessa forma, o projeto possui dois resultados complementares:

**1. Insight:** compreensão dos diferentes segmentos existentes na carteira.

**2. Modelo:** capacidade de aplicar a segmentação a novos clientes.

## Principais Insights

A análise mostrou que os clientes apresentam diferenças não apenas no nível de crédito, mas também na **forma como se relacionam com a instituição**.

O segmento de Relacionamento Digital concentra clientes com maior limite de crédito, mais cartões e forte utilização online. O segmento de Relacionamento Presencial concentra a maior parte da carteira e apresenta maior dependência do atendimento físico. Já o Relacionamento Assistido apresenta menor relacionamento financeiro e maior utilização do atendimento telefônico.

Esses padrões podem ser utilizados como ponto de partida para estratégias diferentes de atendimento, comunicação, relacionamento e ofertas.

## Limitações

A segmentação foi construída com apenas cinco características e não possui informações como renda, rentabilidade, inadimplência, saldo, histórico transacional, produtos contratados ou tempo de relacionamento.

Por isso, os clusters representam principalmente **padrões de comportamento e relacionamento com os canais**, e não uma avaliação completa do valor econômico dos clientes.

Uma aplicação empresarial poderia incorporar variáveis financeiras, transacionais e de rentabilidade para tornar a segmentação mais precisa e acionável.

## Conclusão

O projeto demonstrou como o **K-Means pode ser utilizado para descobrir segmentos de clientes sem uma variável-alvo previamente definida**.

A partir das características comportamentais disponíveis, foi possível identificar três grupos distintos e transformá-los em perfis interpretáveis para o negócio.

Além da análise da carteira atual, o projeto foi convertido em um modelo reutilizável, permitindo que novos clientes sejam posteriormente associados aos segmentos identificados.

Assim, o resultado final combina **análise exploratória, Machine Learning, interpretação de negócio e operacionalização do modelo**, criando uma base para futuras aplicações de CRM, personalização, atendimento e tomada de decisão orientada por dados.

## Próximas Evoluções

Como evolução do projeto, podem ser incorporados:

- Dados financeiros e transacionais;
- Rentabilidade e inadimplência;
- Produtos contratados;
- Histórico de relacionamento;
- Monitoramento da estabilidade dos clusters ao longo do tempo;
- Dashboard para acompanhamento dos segmentos;
- API ou aplicação para classificação automática de novos clientes.
