## Problema 1 - Precificação de Imóveis

### Notebook no Colab - https://colab.research.google.com/drive/1moSUZcAzuPJvPWI2g7Bzjl0i1kp98Leh?usp=sharing

- Eu quero saber se estou pagando bem ou mal por um imóvel.
- Kaggle: site que disponibiliza base de dados.
- Os dados usados na aula foram previamente limpados. Faz parte da área limpar os dados para enviá-los para a análise.
- As bases de dados são chamadas de data frame.
- Primeiro **Colab com Python e Pandas**
    
    ```python
    # Importando biblioteca do Pandas. "pd" é uma abreviação de Pandas. 
    import pandas as pd
    # Variável que será usada de caminho em um parâmetro. 
    url = "https://gist.githubusercontent.com/tgcsantos/3bdb29eba6ce391e90df2b72205ba891/raw/22fa920e80c9fa209a9fccc8b52d74cc95d1599b/dados_imoveis.csv"
    # Chamando função de leitura de um arquivo no formato .csv no parâmetro (url).
    dados = pd.read_csv(url)
    
    # Imprimindo o conteúdo das 5 primeiras linhas da base de dados na variável dados.
    dados.head()
    
    # Imprime todo o conteúdo da variável dados. 
    dados
    
    # Imprime uma linha aleatória da variável dados. Posso alterar o número de linha adicionando um número inteiro no parâmetro.
    dados.sample()
    
    # Imprime o tipo de dado que está sendo trabalhado.
    type(dados)
    
    # Imprime os dados de uma determinada coluna. 
    dados["Bairro"]
    
    # Imprime um determinado dado de um determinado index.
    dados["Bairro"][6522]
    
    # Imprime informações detalhadas sobre o conteúdo de um data frame. 
    dados.info
    
    # Média entre o m² de todos os terrenos. dados["Metragem"].mean() é a mesma coisa. 
    dados.Metragem.mean()
    
    # Imprime a quantidade de um dado específico dentro do data frame. 
    sum((dados["Bairro"] == "Vila Mariana"))
    
    # Guarda os dados em formato booleano
    tem_imoveis_vila = (dados["Bairro"] == "Vila Mariana")
    tem_imoveis_vila
    
    # Imprime somente os dados que correspondem ao comando
    imoveis_vila_mariana = dados[tem_imoveis_vila]
    imoveis_vila_mariana
    
    # Imprime um dados específico de uma variável 
    imoveis_vila_mariana["Metragem"].mean()
    
    # Quantidade de imóveis em cada bairro
    dados["Bairro"].value_counts()
    
    # Gráfico com os 10 bairros com mais imóveis
    n_imoveis_bairro = dados["Bairro"].value_counts()
    n_imoveis_bairro.head(10).plot.bar()
    ```