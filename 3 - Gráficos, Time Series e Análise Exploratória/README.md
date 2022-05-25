## **Gráficos, Time Series e Análise Exploratória**

```python
import pandas as pd
url = "https://gist.githubusercontent.com/tgcsantos/3bdb29eba6ce391e90df2b72205ba891/raw/22fa920e80c9fa209a9fccc8b52d74cc95d1599b/dados_imoveis.csv"
dados = pd.read_csv(url)

dados.head()

dados.sample(10)

dados.info()

dados["Bairro"][6522]

dados.Metragem.mean()

dados["Metragem"].mean()

sum((dados["Bairro"] == "Vila Mariana"))

tem_imoveis_vila =( dados["Bairro"] == "Vila Mariana")
tem_imoveis_vila

imoveis_vila_mariana = dados[tem_imoveis_vila]
imoveis_vila_mariana

imoveis_vila_mariana["Metragem"].mean()

dados["Bairro"].value_counts()

n_imoveis_bairro = dados["Bairro"].value_counts()
n_imoveis_bairro.head(10).plot.bar()

dados.head()

dados.info()

dados["Valor"][0]

dados["Valor"][0].split()

dados.sample(10)

dados["Valor"].str.split(expand = True)

len(dados["Valor"].str.split(expand = True)[1].unique())

dados["Valor"].str.split(expand = True)[2].unique()

dados[["Valor", "Bairro"]]

dados[["Moeda", "Valor_anuncio", "Tipo_anuncio"]] = dados["Valor"].str.split(expand = True)
dados.head()

dados[dados["Tipo_anuncio"].isnull()]["Tipo_anuncio"].unique()

dados_vendas = dados[dados["Tipo_anuncio"].isnull()]
dados_vendas

dados_vendas.info()

dados_vendas["Valor_anuncio"].str.replace(".","").astype(float)

dados_vendas["Valor_anuncio_float"] = dados_vendas["Valor_anuncio"].str.replace(".","").astype(float)

dados_vendas.info()

dados_vendas["Valor_anuncio_float"].plot.hist(bins = 50)

pd.set_option("display.precision", 2)
pd.set_option('display.float_format', lambda x: '%.2f' % x)

dados_vendas.describe()

dados_vendas[["Rua", "Bairro", "Cidade"]].describe()

import seaborn as sns
import matplotlib.pyplot as plt

sns.set()
plt.figure(figsize=(10, 8))
ax = sns.histplot(data = dados_vendas, x = "Valor_anuncio_float", kde = True)
ax.set_title("Histograma Valor Imóvel")
plt.xlim((-50, 10000000))
plt.show()

dados_vendas.head()

dados_vendas["Valor_m2"] = dados_vendas["Valor_anuncio_float"]/dados_vendas["Metragem"]
dados_vendas.head()

dados_vendas.groupby("Bairro").mean()

dados_bairro = dados_vendas.groupby("Bairro").sum()
dados_bairro

dados_bairro["Valor_m2_bairro"] = dados_bairro["Valor_anuncio_float"]/dados_bairro["Metragem"]
dados_bairro

dados_vendas.groupby("Bairro").mean().loc["Vila Mariana"]

dados_bairro.loc["Vila Mariana"]

dados_bairro["Valor_m2_bairro"]["Vila Mariana"]

top_bairros = dados_vendas["Bairro"].value_counts()[:10].index

dados_bairro.reset_index(inplace = True)
dados_bairro

dados_bairro.query("Bairro in @top_bairros")

plt.figure(figsize=(10, 8))
ax = sns.barplot(x="Bairro", y="Valor_m2_bairro", data=dados_bairro.query("Bairro in @top_bairros"))
ax.tick_params(axis='x', rotation=45)

plt.figure(figsize=(10, 8))
ax = sns.boxplot(data = dados_vendas.query("Bairro in @top_bairros"), x="Bairro", y="Valor_anuncio_float")
ax.tick_params(axis='x', rotation=45)
plt.show()

plt.figure(figsize=(10, 8))
ax = sns.boxplot(data = dados_vendas.query("Bairro in @top_bairros & Metragem < 30000"), x="Bairro", y="Metragem")
ax.tick_params(axis='x', rotation=45)
plt.show()

dados_vendas

ibge_url = "https://gist.githubusercontent.com/tgcsantos/85f8c7b0a2edbc3e27fcad619b37d886/raw/a4954781e6bca9cb804062a3eea0b3b84679daf4/Basico_SP1.csv"
pd.read_csv(ibge_url)
```