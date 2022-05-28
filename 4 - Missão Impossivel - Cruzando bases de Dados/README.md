## 4 - **Missão Impossivel: Cruzando bases de Dados**

### Notebook no Colab - https://colab.research.google.com/drive/1WzaYKTJuVoQ1_WeVdBwKsmkCV9PU7IGD?usp=sharing

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

import pandas as pd
ibge_sp = pd.read_csv(
    'https://gist.githubusercontent.com/tgcsantos/85f8c7b0a2edbc3e27fcad619b37d886/raw/a4954781e6bca9cb804062a3eea0b3b84679daf4/Basico_SP1.csv',
    encoding='ISO-8859-1',
    sep=';', thousands='.', decimal=','
)
ibge_sp.dropna(how='all', axis=1, inplace=True)
ibge_sp.head()

ibge_sp.info()

dados_vendas

enderecos = pd.read_csv("/content/drive/MyDrive/imersao_dados_5/enderecos.csv")

enderecos.head()

enderecos_sp = enderecos.query("sigla_uf == 'SP'")
enderecos_sp

enderecos_sp["rua"] = enderecos_sp["tipo_logr"] + " " + enderecos_sp["logr_nome"]
enderecos_sp["rua"] = enderecos_sp["rua"].str.lower().str.strip()
enderecos_sp.head()

dados_vendas["Rua"].str.extract(r'(^[\w ]+)')[:10]

dados_vendas["apenas_rua"] = dados_vendas["Rua"].str.extract(r'(^[\w ]+)')
dados_vendas["apenas_rua"] = dados_vendas["apenas_rua"].str.lower().str.strip()
dados_vendas.head()

dados_geo = pd.merge(left = dados_vendas, right = enderecos_sp[["rua", "cep", "latitude", "longitude"]], how = "left", left_on = "apenas_rua", right_on = "rua").drop_duplicates(subset=dados_vendas.columns).query("cep > 0")
dados_geo

ibge_sp.info()

from shapely.geometry import Point
latitude = -23.56 	
longitude = -46.59
Point(longitude, latitude)

from shapely.geometry import Polygon
Polygon([[0, 0], [1, 0], [1, 1], [0, 1], [0, 0]])

Polygon([[0, 0], [1, 0], [1, 1], [0, 1], [0, 0]]).contains(Point(0.1,0.9))

!pip install geopandas

import geopandas as gpd

setor_censo = gpd.read_file(
'/content/drive/MyDrive/imersao_dados_5/35SEE250GC_SIR.shp'
)
setor_censo.head()

setor_censo_sp = setor_censo[setor_censo.NM_MUNICIP == "SÃO PAULO"]
setor_censo_sp[setor_censo_sp.contains(Point(-46.63, -23.58))]

dados_geo["Point"] = ""
for i in dados_geo.index:
    dados_geo["Point"][i] = Point(dados_geo["longitude"][i], dados_geo["latitude"][i])

dados_geo['setor_censo'] = dados_geo["Point"][:10].map(
    lambda x: setor_censo_sp.loc[setor_censo_sp.contains(x), 'CD_GEOCODI'].values
).str[0]
dados_geo

dados_geo = pd.read_csv("/content/drive/MyDrive/imersao_dados_5/dados_geo.csv")
dados_geo.head()

dados_vendas_censo = pd.merge(left = dados_geo, right = ibge_sp, how = "left", left_on = "setor_censo", right_on = "Cod_setor")
dados_vendas_censo

dados_vendas_censo.info()

plt.figure(figsize=(10, 10))
sns.scatterplot(data = dados_vendas_censo, x="V005", y="Valor_m2")
```