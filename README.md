# 🚕 Análise de Viagens de Táxi de Nova York com MapReduce

Projeto desenvolvido para aplicação do paradigma de programação **MapReduce**
na análise de dados de viagens de táxi da cidade de Nova York.

A solução foi implementada em **Python**, utilizando **Jupyter Notebook /
Google Colab**, seguindo explicitamente o fluxo:

**Map → Shuffle → Reduce**

---

## 🎓 Informações Acadêmicas

**Instituição:** Centro Universitário IESB  
**Curso:** Ciência de Dados  
**Disciplina:** Processamento de Dados Massivos  
**Professor:** Alexandre Roriz  
**Aluno:** Ivan Meireles de Abreu  
**Atividade:** Atividade Avaliativa — MapReduce  

---

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como atividade avaliativa da disciplina
**Processamento de Dados Massivos**, do curso de **Ciência de Dados do
Centro Universitário IESB**, ministrada pelo professor **Alexandre Roriz**.

O objetivo da atividade é utilizar o paradigma de programação
**MapReduce** para realizar análises sobre dados de viagens de táxi da
cidade de Nova York, permitindo compreender seu funcionamento e suas
limitações.

A implementação foi estruturada explicitamente nas etapas:

```text
MAP → SHUFFLE → REDUCE
```

Além dos resultados finais, foram mantidos arquivos intermediários
gerados durante o processamento. Dessa forma, é possível visualizar
fisicamente os dados produzidos em cada etapa do MapReduce.

---

## 🎯 Objetivos da Atividade

A partir do dataset de viagens de táxi de Nova York, foram realizadas
as seguintes análises:

1. Número de viagens por tipo de pagamento;
2. Receita total por tipo de pagamento;
3. Tarifa média cobrada nas viagens;
4. Data e hora em que foi realizada a viagem mais longa;
5. Quantidade de viagens por hora;
6. Distância total percorrida por hora.

---

## 📁 Estrutura do Repositório

O projeto foi organizado de forma a separar o notebook, os dados
utilizados, a documentação da atividade e os arquivos produzidos
durante a execução das etapas do MapReduce.

```text
mapreduce-nyc-taxi-analysis/
│
├── README.md
│
├── notebook/
│   └── Atividade_MapReduce_Taxi_NYC_2024.ipynb
│
├── data/
│   └── 
│
├── output/
│   ├── q1_mapper.txt
│   ├── q1_shuffle.txt
│   ├── q1_resultado.txt
│   │
│   ├── q2_mapper.txt
│   ├── q2_shuffle.txt
│   ├── q2_resultado.txt
│   │
│   ├── q3_mapper.txt
│   ├── q3_shuffle.txt
│   ├── q3_resultado.txt
│   │
│   ├── q4_mapper.txt
│   ├── q4_shuffle.txt
│   ├── q4_resultado.txt
│   │
│   ├── q5_mapper.txt
│   ├── q5_shuffle.txt
│   ├── q5_resultado.txt
│   │
│   ├── q6_mapper.txt
│   ├── q6_shuffle.txt
│   └── q6_resultado.txt
│
└── docs/
    ├── Atividade1-MapReduce.pdf
    └── data_dictionary_trip_records_yellow.pdf
```

### 📓 `notebook/`

Contém o Jupyter Notebook utilizado para desenvolver e executar a
atividade.

O notebook contém:

- seleção e carregamento do arquivo CSV;
- leitura dos dados;
- funções auxiliares;
- implementação dos Mappers;
- implementação da etapa Shuffle;
- implementação dos Reducers;
- execução das seis questões propostas;
- apresentação dos resultados obtidos.

O notebook foi salvo com sua execução, permitindo visualizar tanto o
código quanto os resultados produzidos.

### 📊 `data/`

Diretório destinado ao dataset utilizado como entrada para o
processamento.

O arquivo utilizado na execução foi:

```text
nyc_tripdata_2024_sample_1M.csv
```
https://huggingface.co/datasets/alexvaroz/nyc_taxi_trip_2024_p1_sample/resolve/main/nyc_tripdata_2024_sample_1M.csv

A amostra utilizada na execução possui **1.000.000 de registros de
viagens**.

### 📤 `output/`

Contém os arquivos intermediários e finais produzidos durante a
execução do MapReduce.

Para cada questão são gerados arquivos seguindo o padrão:

```text
qN_mapper.txt
qN_shuffle.txt
qN_resultado.txt
```

onde `N` representa o número da questão.

Por exemplo:

```text
q2_mapper.txt
q2_shuffle.txt
q2_resultado.txt
```

correspondem às etapas da **Questão 2**.

### 📚 `docs/`

Contém os documentos utilizados como referência:

- enunciado da atividade avaliativa;
- dicionário de dados dos registros de viagens do NYC Yellow Taxi.

---

## 📖 Dataset

O dataset contém informações sobre viagens de táxi da cidade de
Nova York.

Entre os principais campos utilizados nesta atividade estão:

| Campo | Descrição |
|---|---|
| `tpep_pickup_datetime` | Data e hora de início da viagem |
| `tpep_dropoff_datetime` | Data e hora de término da viagem |
| `trip_distance` | Distância percorrida pela viagem |
| `payment_type` | Código correspondente ao tipo de pagamento |
| `fare_amount` | Tarifa calculada pelo taxímetro |
| `total_amount` | Valor total cobrado do passageiro |

### Tipos de pagamento

O campo `payment_type` utiliza os seguintes códigos:

| Código | Tipo |
|---:|---|
| 0 | Flex Fare trip |
| 1 | Credit card |
| 2 | Cash |
| 3 | No charge |
| 4 | Dispute |
| 5 | Unknown |
| 6 | Voided trip |

---

# ⚙️ Implementação MapReduce

O processamento foi estruturado explicitamente em três etapas:

```text
                    Dataset CSV
                        │
                        ▼
                  ┌──────────┐
                  │   MAP    │
                  └────┬─────┘
                       │
                  chave, valor
                       │
                       ▼
                 ┌───────────┐
                 │  SHUFFLE  │
                 └─────┬─────┘
                       │
                agrupamento
                 por chave
                       │
                       ▼
                 ┌───────────┐
                 │  REDUCE   │
                 └─────┬─────┘
                       │
                       ▼
                   Resultado
```

## 1️⃣ Map

O **Mapper** percorre os registros do dataset e transforma os dados
necessários para cada análise em pares:

```text
chave → valor
```

Por exemplo, para calcular o número de viagens por tipo de pagamento:

```text
Credit card    1
Cash           1
Credit card    1
Cash           1
```

O Mapper não realiza a soma. Sua responsabilidade é produzir os pares
chave/valor que serão utilizados nas etapas seguintes.

---

## 2️⃣ Shuffle

A etapa **Shuffle** recebe a saída produzida pelo Mapper e agrupa os
valores que possuem a mesma chave.

Conceitualmente:

```text
Credit card → [1, 1, ...]
Cash        → [1, 1, ...]
```

Essa etapa prepara os dados para que o Reducer possa realizar a
agregação.

---

## 3️⃣ Reduce

O **Reducer** recebe cada chave acompanhada dos valores agrupados pelo
Shuffle e executa a operação necessária para a questão.

Dependendo da análise, o Reduce pode realizar operações como:

- soma;
- média;
- comparação de valores;
- identificação do maior valor.

No exemplo da quantidade de viagens:

```text
Credit card → 743405
Cash        → 136221
```

---

# 🔄 Fluxo dos Arquivos Gerados

Uma característica desta implementação é a geração de arquivos
intermediários que permitem acompanhar o processamento.

```text
nyc_tripdata_2024_sample_1M.csv
              │
              ▼
           MAPPER
              │
              ▼
      qN_mapper.txt
              │
              ▼
           SHUFFLE
              │
              ▼
      qN_shuffle.txt
              │
              ▼
           REDUCER
              │
              ▼
     qN_resultado.txt
```

Esses arquivos foram mantidos no repositório para permitir a
visualização das transformações realizadas em cada etapa.

---

## 🗂️ Arquivos `*_mapper.txt`

Contêm os pares **chave/valor** emitidos pelo Mapper antes do
agrupamento.

Exemplo:

```text
Credit card    1
Cash           1
Credit card    1
```

---

## 🗂️ Arquivos `*_shuffle.txt`

Contêm os valores agrupados de acordo com suas respectivas chaves,
representando a etapa intermediária entre Map e Reduce.

Conceitualmente:

```text
Credit card → [1, 1, 1, ...]
Cash        → [1, 1, ...]
```

---

## 🗂️ Arquivos `*_resultado.txt`

Contêm os resultados finais produzidos pelo Reducer.

Por exemplo:

```text
Cash             136221
Credit card      743405
Dispute           16543
Flex Fare trip    97124
No charge          6707
```

Dessa forma, os arquivos presentes em `output/` não representam
simplesmente arquivos temporários da execução. Eles permitem observar
as diferentes etapas do processamento MapReduce.

---

# 📊 Análises Realizadas

## Questão 1 — Número de viagens por tipo de pagamento

O Mapper associa cada viagem ao seu tipo de pagamento e emite:

```text
tipo_pagamento → 1
```

Após o Shuffle, o Reducer soma os valores correspondentes a cada tipo.

### Resultado

| Tipo de pagamento | Quantidade de viagens |
|---|---:|
| Credit card | 743.405 |
| Cash | 136.221 |
| Flex Fare trip | 97.124 |
| Dispute | 16.543 |
| No charge | 6.707 |
| **Total** | **1.000.000** |

A soma das categorias resulta exatamente em **1.000.000 de viagens**,
confirmando a consistência da contagem com o número de registros lidos.

---

## Questão 2 — Receita total por tipo de pagamento

O Mapper utiliza o tipo de pagamento como chave e o valor total
da viagem como valor:

```text
tipo_pagamento → total_amount
```

Após o agrupamento realizado pelo Shuffle, o Reducer soma os valores
para determinar a receita total correspondente a cada tipo de
pagamento.

| Tipo de pagamento | Receita total |
|---|---:|
| Credit card | $ 21.785.219,95 |
| Cash | $ 3.168.095,90 |
| Flex Fare trip | $ 2.376.069,77 |
| No charge | $ 53.932,48 |
| Dispute | $ 25.214,51 |
| **Total geral** | **$ 27.408.532,61** |

O maior volume de receita ocorreu nas viagens pagas com **cartão de crédito**.

---

## Questão 3 — Tarifa média cobrada nas viagens

O campo utilizado para essa análise é:

```text
fare_amount
```

O Mapper envia as tarifas para processamento e o Reducer calcula a
média a partir da soma das tarifas e da quantidade de registros.

**Tarifa média cobrada: $ 18,8603**

Arredondando para duas casas decimais:

**$ 18,86**

---

## Questão 4 — Data e hora da viagem mais longa

Para identificar a viagem mais longa são utilizados:

```text
trip_distance
tpep_pickup_datetime
```

O Mapper associa a distância da viagem à respectiva data e hora de
início.

Após o Shuffle, o Reducer compara as distâncias e identifica o maior
valor, preservando a data e hora correspondente.

- **Data e hora de início:** `2024-05-10 17:33:00`
- **Distância registrada:** `86.789,20 milhas`

Observação: a maior distância encontrada foi 86.789,2 milhas, registrada em uma viagem iniciada em 10/05/2024 às 17:33. Embora seja um valor incompatível com uma corrida de táxi de 13 minutos, o registro foi mantido porque consta no dataset original e a atividade não prevê tratamento ou remoção de outliers. Assim, o resultado demonstra também uma limitação importante do processamento MapReduce: o algoritmo agrega os dados recebidos, mas a qualidade do resultado depende da qualidade dos dados de entrada.

---

## Questão 5 — Quantidade de viagens por hora

A hora é extraída do campo:

```text
tpep_pickup_datetime
```

O Mapper produz:

```text
hora → 1
```

O Shuffle agrupa os registros pela hora e o Reducer soma a quantidade
de viagens correspondente a cada período.

| Hora | Quantidade de viagens |
|---:|---:|
| 00:00 | 29.165 |
| 01:00 | 18.822 |
| 02:00 | 12.280 |
| 03:00 | 8.281 |
| 04:00 | 6.054 |
| 05:00 | 6.194 |
| 06:00 | 13.966 |
| 07:00 | 28.065 |
| 08:00 | 38.308 |
| 09:00 | 42.309 |
| 10:00 | 44.804 |
| 11:00 | 48.295 |
| 12:00 | 53.128 |
| 13:00 | 55.362 |
| 14:00 | 59.345 |
| 15:00 | 60.205 |
| 16:00 | 61.563 |
| 17:00 | 67.880 |
| 18:00 | 71.403 |
| 19:00 | 62.752 |
| 20:00 | 56.542 |
| 21:00 | 58.333 |
| 22:00 | 54.581 |
| 23:00 | 42.363 |
| **Total** | **1.000.000** |

O período com maior quantidade de viagens foi **18:00**, com
**71.403 viagens**.

---

## Questão 6 — Distância total percorrida por hora

São utilizados:

```text
tpep_pickup_datetime
trip_distance
```

O Mapper produz pares:

```text
hora → distância
```

Após o agrupamento realizado pelo Shuffle, o Reducer soma as
distâncias correspondentes a cada hora.

| Hora | Distância total (milhas) |
|---:|---:|
| 00:00 | 109.568,73 |
| 01:00 | 60.695,19 |
| 02:00 | 36.643,43 |
| 03:00 | 29.745,90 |
| 04:00 | 28.350,30 |
| 05:00 | 91.136,53 |
| 06:00 | 131.056,86 |
| 07:00 | 195.237,07 |
| 08:00 | 169.111,73 |
| 09:00 | 222.652,02 |
| 10:00 | 138.342,76 |
| 11:00 | 145.189,30 |
| 12:00 | 166.186,16 |
| 13:00 | 190.847,83 |
| 14:00 | 215.732,27 |
| 15:00 | 312.374,38 |
| 16:00 | 238.718,29 |
| 17:00 | 321.659,21 |
| 18:00 | 252.601,19 |
| 19:00 | 265.021,98 |
| 20:00 | 267.461,66 |
| 21:00 | 283.775,08 |
| 22:00 | 189.960,49 |
| 23:00 | 161.753,45 |
| **Total** | **4.223.821,81** |

A maior distância acumulada foi registrada às **17:00**, com
**321.659,21 milhas**.

---

## Conferência dos resultados

A execução utilizada para esta versão do README carregou
**1.000.000 de registros**.

Duas verificações simples de consistência confirmam esse total:

- a soma das categorias de pagamento da Questão 1 é **1.000.000**;
- a soma das quantidades por hora da Questão 5 também é **1.000.000**.

A implementação segue explicitamente o fluxo **Map → Shuffle → Reduce**
solicitado na atividade.

---

# ▶️ Como Executar o Projeto

## 1. Clonar o repositório

Utilizando HTTPS:

```bash
git clone https://github.com/ivanmeirelesdeabreu/mapreduce-nyc-taxi-analysis.git
```

Entre no diretório:

```bash
cd mapreduce-nyc-taxi-analysis
```

---

## 2. Abrir o Notebook

O notebook está localizado em:

```text
notebook/Atividade_MapReduce_Taxi_NYC_2024.ipynb
```

Ele pode ser executado utilizando:

- Google Colab;
- Jupyter Notebook;
- JupyterLab;
- ambiente compatível com arquivos `.ipynb`.

---

## 3. Selecionar o Dataset

No Google Colab, o notebook permite selecionar o arquivo CSV que será
processado.

Exemplo:

```python
from google.colab import files

uploaded = files.upload()

ARQUIVO = next(iter(uploaded))

print("Arquivo selecionado:", ARQUIVO)
```

A variável `ARQUIVO` passa a representar o dataset selecionado e é
utilizada nas células seguintes.

---

## 4. Executar as células

A ordem de processamento é:

```text
Selecionar o dataset
        │
        ▼
Executar os imports
        │
        ▼
Executar as funções auxiliares
        │
        ▼
Executar as questões
        │
        ▼
 MAP → SHUFFLE → REDUCE
        │
        ▼
Visualizar os resultados
```

É importante executar as células do notebook sequencialmente, pois
funções definidas nas células anteriores são reutilizadas nas questões
seguintes.

---

# 🛠️ Tecnologias Utilizadas

- **Python**
- **Jupyter Notebook**
- **Google Colab**
- **Git**
- **GitHub**
- **CSV**
- Paradigma **MapReduce**

---

# 📝 Conclusão

A solução foi estruturada explicitamente em **Map → Shuffle → Reduce**,
conforme proposto na atividade.

Cada questão possui um **Mapper** responsável pela emissão dos pares
chave/valor. A etapa **Shuffle** realiza o agrupamento dos valores de
acordo com suas respectivas chaves e o **Reducer** executa a operação
necessária para produzir o resultado agregado.

Além da execução apresentada no Jupyter Notebook, foram gerados
arquivos intermediários:

```text
q*_mapper.txt
q*_shuffle.txt
q*_resultado.txt
```

Esses arquivos permitem observar fisicamente o fluxo dos dados durante
o processamento e compreender as transformações realizadas em cada
etapa do paradigma MapReduce.

Dessa forma, o projeto apresenta não somente os resultados finais das
seis análises, mas também evidencia o funcionamento interno das etapas
**Map**, **Shuffle** e **Reduce**.

