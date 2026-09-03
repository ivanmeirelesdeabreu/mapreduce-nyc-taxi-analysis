# Análise de Viagens de Táxi de Nova York com MapReduce

## Informações acadêmicas

**Instituição:** Centro Universitário IESB  
**Curso:** Ciência de Dados  
**Disciplina:** Processamento de Dados Massivos  
**Professor:** Alexandre Roriz  
**Aluno:** Ivan Meireles de Abreu  
**Atividade:** Atividade Avaliativa — MapReduce  

---

## Sobre o projeto

Este projeto foi desenvolvido como atividade avaliativa da disciplina
**Processamento de Dados Massivos**, do curso de **Ciência de Dados do
Centro Universitário IESB**, ministrada pelo professor **Alexandre Roriz**.

O objetivo da atividade é aplicar o paradigma de programação
**MapReduce** na análise de dados de viagens de táxi da cidade de
Nova York, permitindo compreender seu funcionamento e também suas
limitações.

A implementação foi realizada em **Python**, utilizando um
**Jupyter Notebook / Google Colab**, seguindo explicitamente o fluxo:

**Map → Shuffle → Reduce**

Além dos resultados das análises, a implementação gera arquivos
intermediários que permitem visualizar cada etapa do processamento.

---

## Objetivos da atividade

A partir do dataset de viagens de táxi de Nova York, foram realizadas
as seguintes análises:

1. Número de viagens por tipo de pagamento;
2. Receita total por tipo de pagamento;
3. Tarifa média cobrada nas viagens;
4. Data e hora em que foi realizada a viagem mais longa;
5. Quantidade de viagens por hora;
6. Distância total percorrida por hora.

## Considerações finais

A solução foi estruturada explicitamente em **Map → Shuffle → Reduce**,
conforme proposto na atividade.

Cada questão possui um **Mapper**, responsável pela emissão dos pares
chave/valor; a etapa **Shuffle** realiza o agrupamento dos valores por
chave; e o **Reducer** executa a operação necessária para produzir o
resultado agregado.

Além da execução apresentada no Jupyter Notebook, foram gerados arquivos
intermediários:

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
