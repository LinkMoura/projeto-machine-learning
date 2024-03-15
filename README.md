# Relatório de Entrega do Projeto
## Trabalhando com Machine Learning na Prática no Azure ML

Repositório: *projeto-machine-learning*

O objetivo do desafio foi Trabalhar com Machine Learning na Prática no Azure ML, para isso temos três objetivos secundários:

> I. Acesso e uso do recurso de machine learning automatizado no Azure Machine Learning;
> II. Treinar e avaliar um modelo de machine learning;  
> III. Implantar e testar o modelo treinado.  

-------------------


## **Passo a passo da criação do modelo de previsão com seus devidos pontos de extremidade configurados:**  

### 1. Criei um espaço de trabalho do Azure Machine Learning  

1. Após fazer login no portal do Azure em <https://portal.azure.com> usando as credenciais da Microsoft. Criei uma conta gratuita.

2. Selecionei + Criar um recurso, pesquisei Machine Learning e criei um novo recurso Azure Machine Learning com as seguintes configurações:

> - Assinatura: a assinatura do Azure dsiponível.
> - Grupo de recursos: criei o grupo de recursos “labai900”.
> - Nome: inserir o nome do espaço de trabalho “labai900”.
> - Região: opção sugerida “East US”
> - Conta de armazenamento: utilizei a conta padrão criada para esse espaço de trabalho.
> - Cofre de chaves: utilizei o cofre de chaves padrão criado para esse espaço de trabalho.
> - Insights de aplicativos: utilizei o recurso padrão de insights de aplicativos criado esse espaço de trabalho.
> - Registro de contêiner: “Nenhum”.
3. Selecionei Revisar + criar e selecione Criar. Aguardei a criação do seu espaço de trabalho, para ir para o recurso implantado.
4. Selecionei Iniciar estúdio; abriu uma nova guia do navegador para <https://ml.azure.com>.
5. No estúdio Azure Machine Learning, pode ser visto o espaço de trabalho recém-criado, em “Todos os espaços de trabalho”, no menu lateral. Entrei no espaço de trabalho criado: “labai900”.  
  
### 2. Usar Machine Learning para treinar um modelo  

No espaço de trabalho, volta-se ao Studio, onde se faz a opção por “ML automatizado” no menu lateral.
Clicar em + Novo trabalho de ML automatizado.

Observação: se usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.  
  
O novo trabalho abre uma nova página para as configurações necessárias, com os seguintes itens:  
 
 **Método de treinamento: Treinar automaticamente**
> **Configurações básicas:**  
> - Job name: mslearn-bike-automl
> - New experiment name: mslearn-bike-rental
> - Description: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
> - Tags: none  
  
> **Tipo de tarefa e dados:**  
> - Select task type: Regressão
> - Select dataset: Create a new dataset with the following settings:
> - Tipos de dados:  
> > - Name: aluguel-bicicletas
> > - Description: Dados históricos de aluguel de bicicletas
> > - Type: Tabular
  
 » Avançar  
> **Fonte de dados:**  
> - Select de arquivos da web  

  » Avançar  
> **Web URL**  
> -  Web URL: <https://aka.ms/bike-rentals>  
> - Skip data validation: do not select  

  » Avançar  
> **Configurações:**  
> - File format: Delimited  
> - Delimiter: Comma  
> - Encoding: UTF-8  
> - Column headers: Somente o primeiro arquivo possui cabeçalho  
> - Skip rows: None  
> - Dataset contains multi-line data: do not select  

  
 » Avançar  
> **Esquema:**  
> - Include all columns other than Path  
  
 » Avançar  
 > - Review the automatically detected types  
 > - Selecione Criar.  

> Depois que a dataset foi criada, selecionei o dataset aluguel-bicicletas para submeter o ML job.  
 
  
 » Avançar  
  
> **Configurações de tarefa:**  
 > - Task type: Regression  
 > - Dataset: aluguel-bicicletas  
 > - Target column: Rentals (integer)  
  
> **Selecionar “Additional configuration settings”:**  
 > - Primary metric: Normalized root mean squared error  
 > - Explain best model: Unselected  
 > - Use all supported models: Unselected.  
 > - Allowed models: Select only RandomForest and LightGBM — normally you’d want to try as many as possible, but each model added increases the time it takes to run the job.  
  
> **Limits: Expand this section**  
 > - Max trials: 3  
 > - Max concurrent trials: 3  
 > - Max nodes: 3  
 > - Metric score threshold: 0.085 (so that if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.)  
 > - Timeout: 15  
 > - Iteration timeout: 15  
 > - Enable early termination: Selected  
  
> **Validation and test:**  
 > - Validation type: Divisão de validação de treinamento  
 > - Percentage of validation data: 10  
 > - Test dataset: None  
 
  
 » Avançar  
  
> **Cálculo:**  
        ◦ Select compute type: Serverless  
        ◦ Virtual machine type: CPU  
        ◦ Virtual machine tier: Dedicated  
        ◦ Virtual machine size: Standard_DS3_V2*  
        ◦ Number of instances: 1  
 » Avançar  
  
> **Enviar trabalho de treinamento**  
       Aqui foi Submetido o trabalho de treinamento, automaticamente.  
  
**OBSERVAÇÃO**: Tempo foi menos de 10 min 
  
### c. Avaliar o melhor modelo  

Após completar o “automated machine learning job”  pode ver o melhor modelo treinado.  
  
1.  Na aba Visão geral, aparece o resumo do melhor modelo.  
2. Selecionei o texto sob Algorithm name “VotingEnsemble” para ver os detalhes do melhor modelo.  
3. Selecionar a aba das Metrics (menu acima) e seleciona os graficos residuals e predicted_true if they are not already selected.  

Ao verificar os gráficos que mostram a performance do modelo. O Gráfico "residuals" mostra as diferenças entre o predito e os valores reais) como um histograma. O Gráfico "predicted_true" compara os valores preditos frente os valores reais.  

### d. Implantar e testar o modelo  

1. On the Model tab for the best model trained by your automated machine learning job, select Deploy and use the Web service option to deploy the model with the following settings:  
 > - Name: prevealuguel  
 > - Description: Predict cycle rentals  
 > - Compute type: Azure Container Instance  
 > - Enable authentication: Selected  
 
 » Implantar  
  
  2.  Espera para a impantação comçar – levou um tempo. O Status da Implantação (Deploy status) para o endpoint do prevealuguel ficou indicado nas “notificações” no menu da parte superior da págiana como implantação em andamento.  
  3. Aguardar até o “Deploy status” mudar para “Healthy”.
  
### e. Testar o serviço implantado  
  
 Testei o serviço implantado.  
  1. No Azure Machine Learning studio, no menu do lado esquerdo, selecionei Endpoints e abri o “prevealuguel real-time endpoint”.  
  2. Na página do “ prevealuguel real-time endpoint” abrir a aba de Testar.  
  3. No painel de Inserir dados para teste de ponto de extremidade, troquei o template JSON pelo seguinte código de input data:
     
 ~~~json 
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
 ~~~

  4.  Cliquei o botão de Testar.  
  5. Verificando os resultados do teste, que inclue o numero predito de alugueis baseado no the recursos de entrada – que retornou o seguinte:
     
~~~json  
       {  
         "Results": [  
           388.3180761141912  
         ]  
       }  
~~~  

  O painel de teste pegou os dados de entrada e usou o modelo treinado para retornar o número previsto de aluguéis.  
  
  **Em resumo:** foi usado um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo previu o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.  
  
### f. Limpar o espaço de trabalho  
  
    1.  No estúdio Azure Machine Learning , na guia Endpoints , selecionei o ponto de extremidade de previsão de aluguel . Em seguida,  Excluir e confirmei que desejo excluir o endpoint.

Para deletar o workspace:  

    No portal Azure , na página Grupos de recursos , abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.
    
    Clique em Excluir grupo de recursos , digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione Excluir .
-------------------

