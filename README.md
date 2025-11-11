# Análise de Segmentação de Clientes - E-commerce

## Descrição do Projeto

Este projeto realiza uma análise exploratória e segmentação de clientes utilizando técnicas de clusterização (K-Means) em um dataset de comportamento de compra de consumidores. O objetivo principal é identificar perfis distintos de clientes para orientar estratégias de marketing e vendas mais direcionadas.

## Dataset

**Consumer Behavior and Shopping Habits Dataset**  
Fonte: [Kaggle](https://www.kaggle.com/datasets/zeesolver/consumer-behavior-and-shopping-habits-dataset)

O dataset contém informações detalhadas sobre:
- Perfil demográfico (idade, gênero)
- Histórico de compras
- Preferências de produtos
- Comportamento de compra (uso de descontos, frequência)
- Métodos de pagamento e entrega
- Avaliações de produtos

### Variáveis Principais

| Variável | Descrição |
|----------|-----------|
| Age | Idade do cliente |
| Gender | Gênero do cliente |
| Purchase Amount (USD) | Valor gasto na compra |
| Review Rating | Avaliação do produto (2.5-5.0) |
| Previous Purchases | Número de compras anteriores |
| Category | Categoria do produto |
| Subscription Status | Status de assinatura ativa |
| Season | Estação do ano da compra |

## Metodologia

### 1. Análise Exploratória de Dados (EDA)
- Distribuição de variáveis categóricas e numéricas
- Análise de padrões de compra
- Identificação de características do público

### 2. Pré-processamento
- Tratamento de valores ausentes
- Codificação de variáveis categóricas (One-Hot Encoding)
- Normalização de variáveis numéricas (StandardScaler)
- Redução de dimensionalidade para otimização

### 3. Otimização de Features
Análise de entropia para identificar e remover variáveis que prejudicavam a clusterização:

**Variáveis Removidas (8):**
- Location (50 valores únicos, entropia alta)
- Color (25 valores únicos, distribuição uniforme)
- Item Purchased (25 valores únicos, redundante com Category)
- Frequency of Purchases (entropia alta, redundante)
- Shipping Type (distribuição perfeitamente uniforme)
- Payment Method (distribuição perfeitamente uniforme)
- Discount Applied (correlacionado com Promo Code)
- Promo Code Used (correlacionado com Discount Applied)

**Resultado:** Redução de 130 features para 15 features (-88.5%)

### 4. Modelagem
- **Algoritmo:** K-Means Clustering
- **Número de Clusters:** 4 (definido via Elbow Method e Silhouette Score)
- **Técnicas de Visualização:** MDS e t-SNE para redução dimensional

### 5. Métricas de Avaliação

| Métrica | Antes da Otimização | Depois da Otimização | Melhoria |
|---------|---------------------|----------------------|----------|
| Número de Features | 130 | 15 | -88.5% |
| Silhouette Score | 0.040 | 0.1135 | +184% |
| Interpretação | Sem estrutura clara | Estrutura fraca detectável | Significativa |

## Resultados: 4 Personas Identificadas

### Cluster 0: "Compradores Maduros e Engajados" (26.2%)
- Idade média: 47 anos
- Maior valor de compra: $76.10
- Melhor avaliação: 4.21/5
- Alto histórico: 35 compras anteriores
- **Estratégia:** Programa VIP, produtos premium, fidelização

### Cluster 1: "Jovens Gastadores" (23.3%)
- Idade média: 32 anos (mais jovens)
- Valor de compra: $68.16
- Avaliação: 4.13/5
- Baixo histórico: 15 compras (clientes novos)
- **Estratégia:** Aquisição, redes sociais, programa de assinatura

### Cluster 2: "Compradores Casuais e Econômicos" (24.7%)
- Idade média: 41 anos
- Menor valor: $44.87
- Menor avaliação: 3.30/5 (insatisfeitos)
- Baixo histórico: 15 compras
- **Estratégia:** Promoções, melhorar experiência, reengajamento

### Cluster 3: "Maduros Conservadores" (25.8%)
- Idade média: 55 anos (mais velhos)
- Valor médio: $49.81
- Avaliação: 3.36/5 (insatisfeitos mas fiéis)
- Alto histórico: 34 compras
- **Estratégia:** Melhorar qualidade, atendimento personalizado

## Tecnologias Utilizadas

- **Python 3.10+**
- **Pandas** - Manipulação de dados
- **NumPy** - Operações numéricas
- **Scikit-learn** - Machine Learning (K-Means, StandardScaler, MDS, t-SNE)
- **Plotnine** - Visualizações no estilo ggplot2
- **Matplotlib/Seaborn** - Visualizações complementares
- **KaggleHub** - Download do dataset

## Estrutura do Projeto

```
aps-ecommerce-clustering/
│
├── aps_analisys.ipynb          # Notebook principal com análise completa
├── aps_analisys_2.ipynb        # Notebook com otimização de features
├── requirements.txt             # Dependências do projeto
├── env/                         # Ambiente virtual Python
└── README.md                    # Este arquivo
```

## Como Executar

### Pré-requisitos
- Python 3.10 ou superior
- pip instalado

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/nicolasyassuda/aps-ecommerce-clustering.git
cd aps-ecommerce-clustering
```

2. Crie e ative o ambiente virtual:
```bash
python -m venv env
# Windows
.\env\Scripts\activate
# Linux/Mac
source env/bin/activate
```

3. Instale as dependências:
```bash
pip install -r requirements.txt
```

4. Execute o Jupyter Notebook:
```bash
jupyter notebook aps_analisys.ipynb
```

## Principais Insights

1. **Distribuição Balanceada:** Os 4 clusters têm tamanhos similares (~25% cada)

2. **Idade e Valor Correlacionados:** Clientes mais velhos tendem a gastar mais

3. **Problema de Satisfação:** Clusters 2 e 3 apresentam avaliações baixas (3.3-3.4/5), indicando necessidade urgente de melhoria

4. **Oportunidade de Assinatura:** 73% dos clientes não possuem assinatura ativa

5. **Fidelidade vs Satisfação:** Cluster 3 é fiel (34 compras) mas insatisfeito (3.36/5) - alerta crítico

## Limitações e Trabalhos Futuros

### Limitações
- Silhouette Score moderado (0.11) indica sobreposição entre clusters
- Dataset possivelmente sintético com distribuições muito uniformes
- K-Means assume clusters esféricos, pode não capturar estruturas complexas

### Melhorias Futuras
1. Testar algoritmos alternativos (DBSCAN, Hierarchical Clustering, Gaussian Mixture)
2. Implementar análise RFM (Recência, Frequência, Valor Monetário)
3. Validar personas com dados reais de conversão
4. Realizar A/B testing para validar estratégias por cluster
5. Incorporar dados comportamentais adicionais (tempo no site, taxa de conversão)

## Conclusões

Apesar da baixa separação natural dos dados (comum em datasets sintéticos), a análise conseguiu:
- Identificar 4 perfis distintos de clientes
- Gerar insights acionáveis para estratégias de marketing
- Demonstrar o processo completo de segmentação de clientes
- Melhorar significativamente a qualidade da clusterização através de otimização de features

O projeto demonstra que nem sempre métricas perfeitas são necessárias para extrair valor dos dados. O importante é compreender o comportamento dos clientes e fundamentar decisões estratégicas.

## Autores

Projeto desenvolvido como APS (Atividade Prática Supervisionada) de Análise de Comportamento de Consumidores.

## Licença

Este projeto é disponibilizado para fins educacionais.

## Referências

- Dataset: [Consumer Behavior and Shopping Habits Dataset - Kaggle](https://www.kaggle.com/datasets/zeesolver/consumer-behavior-and-shopping-habits-dataset)
- Scikit-learn Documentation: https://scikit-learn.org/
- Plotnine Documentation: https://plotnine.readthedocs.io/
