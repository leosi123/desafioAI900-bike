# Resumo passo a passo de como criar e implmentar um modelo de regressão no Azure

## Pré-requisitos
Ter conta no azure

## Passo a passo

### 1 - Setup Azure Machine Learning
Criar recurso Azure Machine Learning, caso nao tenha um resource group criado, criar um para que possamos continuar.

Em seguida, colocar informações do workspace : nome, região (colocar USA, pois costuma ser mais barato). Os outros campos não são necessarios alterar para este laboratório.

Clicar em review + create. 

Clicar em create, se tudo estiver ok.

### 2 - Criação do modelo
Abrir estudio Azure Machine Learning.

Criar Workspace (no nosso caso ja criamos o workspace).

No menu da esquerda clicar em ML automatizado. Em seguida clicar em nomo trabalho de ML automatizado.

Nas configurações básicas preencher:

<li>Job name: mslearn-bike-automl
<li>New experiment name: mslearn-bike-rental
<li>Description: Automated machine learning for bike rental prediction
<li>Tags: none

Em seguida clicar em prosseguir.

Escolher tipo de tarefa como regressao e criar o conjunto de dados a ser utilizado.

Dar nome, descrição e tipo do conjunto de dados:
<li>Name: bike-rentals
<li>Description: Historic bike rental data
<li>Type: Tabular

Clicar em avançar.

Escolher fonte de dados de arquivos da web e avançar.
Inserir o seguinte link no campo da URL: https://aka.ms/bike-rentals

Clicar em avançar.

Em configurações seelecionar os seguintes campos:
<li>File format: Delimited
<li>Delimiter: Comma
<li>Encoding: UTF-8
<li>Column headers: Only first file has headers
<li>Skip rows: None
<li>Dataset contains multi-line data: do not select

Esperar carregar e clicar em avançar.

A próxima tela mostrará o esquema da tabela extraída, conferir e avançar.

Na próxima avançar também.
Aqui finalizamos a criação do conjhunto de dados que sera utilizado no modelo.

Em configuração de tarefas:
Escolher coluna de destino como rentals
Clicar eme xisbir configurações adicionais para desmarcar todas as opções e escolher os tipos de modelos que serão utilizados (marcar LIghtGMB e Random Forest)

Expandir seção de limites e colocar os seguintes parâmetros:

<li>Max nodes: 3
<li>Max concurrent trials: 3
<li>Max trials: 3
<li>Metric score threshold: 0.085 (so that if a model achieves a normalized root mean <li>squared error metric score of 0.085 or less, the job ends.)
<li>Timeout: 15
<li>Iteration timeout: 15
<li>Enable early termination: Selected

Em teste de validação escolher a opção: Divisão de validação de treinamento e colocar validação em 10(%)

Clicar em avançar.

Nesta seção vamos escolher o hardware que será utilizado para rodar o modelo.

<li>Computação: sem servidor
<li>Tipo de maquina: CPU
<li>Tipo de maquina: Dedicado
<li>Tamanho da maquina: Standard
<li>Numero de instancias:1

Clicar em avançar e em seguida clicar em enviar trabalho de treinamento.
Esperar até finalizar.
Com isso finalizamos a criação do modelo e seu treinamento do modelo.

### 3 - Análise e teste do modelo

Para analisar o modelo, um bom começo é oplhar as métricas do modelo treinado. Para isso clicar no modelo treinado e acessar a aba métricas.
Nesta aba será possível avaliar as métricas calculadas para o modelo.

Para implementar e testar o modelo, temos que acessar a aba lateral modelo.
Clicar em deploy e em seguida escolher web service.

Em seguida vamos inserir os seguintes parâmetros:

<li>Name: predict-rentals
<li>Description: Predict cycle rentals
<li>Compute type: Azure Container Instance
<li>Enable authentication: Selected

Clicar em avançar. E esperar o deploy status ser bem sucedido.

Para o teste, clicar em Endpoints, na aba lateral e em seguida clicar em predict-rentals (o que acabamos de criar)

Em seguida, acessar a aba de teste.

Na aba de teste teremos um campo para inserir um JSON de input para o modelo, vamos inserir o seguinte JSON:

 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }

 Clique em teste e o modelo deverá retornar um JSON no formato abaixo:

 {
  "Results": [
    368.4827147503707
  ]
}
