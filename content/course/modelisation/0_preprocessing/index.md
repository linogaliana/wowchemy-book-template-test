---
title: "Préparation des données pour construire un modèle"
date: 2020-10-15T13:00:00Z
draft: false
weight: 10
plotly: true
output: 
  html_document:
    keep_md: true
    self_contained: true
slug: preprocessing
tags:
  - scikit
  - machine learning
  - US election
  - preprocessing
categories:
  - Exercice
type: book
summary: |
  Afin d'avoir des données cohérentes avec les hypothèses de modélisation,
  il est absolument fondamental de prendre le temps de
  préparer les données à fournir à un modèle. La qualité de la prédiction
  dépend fortement de ce travail préalable de preprocessing.
  Beaucoup de méthodes sont disponibles dans scikit, ce qui rend ce travail
  moins fastidieux. 
---



<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.6/require.min.js" integrity="sha512-c3Nl8+7g4LMSTdrm621y7kf9v3SDPnhxLNhcjFJbKECVnmZHTdo+IRO05sNLTH/D3vA6u1X32ehoLC7WFVdheg==" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js" integrity="sha512-bLT0Qm9VnAYZDflyKcBaQ2gg0hSYNQrJ8RilYldYQ1FxQYoCLtUjuuRuZo+fjqhx/qtq/1itJ0C2ejDxltZVFg==" crossorigin="anonymous"></script>
<script type="application/javascript">define('jquery', [],function() {return window.jQuery;})</script>
<script src="https://cdn.plot.ly/plotly-2.11.1.min.js"></script>


<a href="https://github.com/linogaliana/python-datascientist/blob/master/notebooks/course/modelisation/0_preprocessing.ipynb" class="github"><i class="fab fa-github"></i></a>
[![Download](https://img.shields.io/badge/Download-Notebook-important?logo=Jupyter.png)](https://downgit.github.io/#/home?url=https://github.com/linogaliana/python-datascientist/blob/master/notebooks/course/modelisation/0_preprocessing.ipynb)
[![nbviewer](https://img.shields.io/badge/Visualize-nbviewer-blue?logo=Jupyter.png)](https://nbviewer.jupyter.org/github/linogaliana/python-datascientist/blob/master/notebooks/course/modelisation/0_preprocessing.ipynb)
[![Onyxia](https://img.shields.io/badge/SSPcloud-Tester%20via%20SSP--cloud-informational&color=yellow?logo=Python.png)](https://datalab.sspcloud.fr/launcher/inseefrlab-helm-charts-datascience/jupyter?autoLaunch=true&onyxia.friendlyName=%C2%ABpython-datascience%C2%BB&init.personalInit=%C2%ABhttps%3A%2F%2Fraw.githubusercontent.com%2Flinogaliana%2Fpython-datascientist%2Fmaster%2Fsspcloud%2Finit-jupyter.sh%C2%BB&init.personalInitArgs=%C2%ABnotebooks/course/modelisation%200_preprocessing.ipynb%C2%BB&security.allowlist.enabled=false)<br>
[![Binder](https://img.shields.io/badge/Launch-Binder-E66581.svg?logo=data:image/png;base64,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.png)](https://mybinder.org/v2/gh/linogaliana/python-datascientist/master?filepath=notebooks/course/modelisation/0_preprocessing.ipynb)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](http://colab.research.google.com/github/linogaliana/python-datascientist/blob/master/notebooks/course/modelisation/0_preprocessing.ipynb)
[![githubdev](https://img.shields.io/static/v1?logo=visualstudiocode&label=&message=Open%20in%20Visual%20Studio%20Code&labelColor=2c2c32&color=007acc&logoColor=007acc.png)](https://github.dev/linogaliana/python-datascientist/notebooks/course/modelisation/0_preprocessing.ipynb)

Ce chapitre utilise le jeu de données présenté dans l'[introduction
de cette partie](https://linogaliana-teaching.netlify.app/modelisation/):
les données de vote aux élections présidentielles US
croisées à des variables socio-démographiques. Le code
est disponible [sur Github](https://github.com/linogaliana/python-datascientist/blob/master/content/course/modelisation/get_data.py) mais l'exercice 1 permet, à ceux qui le désirent, d'essayer de reproduire la constitution de la base de données.

Le guide utilisateur de `scikit` est une référence précieuse,
à consulter régulièrement. La partie sur le *preprocessing* est
disponible [ici](https://scikit-learn.org/stable/modules/preprocessing.html).

Nous verrons dans le chapitre sur les *pipelines* comment industrialiser
ces étapes de pré-processing afin de se simplifier la vie pour appliquer
un modèle sur un jeu de données différent de celui sur lequel il a été estimé.

# Construction de la base de données

Les sources étant éclatées, le code pour construire une base combinant toutes ces
sources est directement fourni. Le travail de construction d'une base unique
est un peu fastidieux mais il s'agit d'un bon exercice, que vous pouvez tenter,
pour [réviser `pandas`](#pandas) :

{{% box status="exercise" title="Exercice" icon="fas fa-pencil-alt" %}}

**Exercice 1 : Importer les données des élections US** \[OPTIONNEL\]

1.  Télécharger et importer le shapefile [depuis ce lien](https://www2.census.gov/geo/tiger/GENZ2019/shp/cb_2019_02_sldl_500k.zip)
2.  Exclure les Etats suivants: "02", "69", "66", "78", "60", "72", "15"
3.  Importer les résultats des élections depuis [ce lien](https://raw.githubusercontent.com/tonmcg/US_County_Level_Election_Results_08-20/master/2020_US_County_Level_Presidential_Results.csv)
4.  Importer les bases disponibles sur le site de l'USDA en faisant attention à renommer les variables de code FIPS de manière identique
    dans les 4 bases
5.  *Merger* ces 4 bases dans une base unique de caractéristiques socio-économiques
6.  *Merger* aux données électorales à partir du code FIPS
7.  *Merger* au shapefile à partir du code FIPS. Faire attention aux 0 à gauche dans certains codes. Il est
    recommandé d'utiliser la méthode `str.lstrip` pour les retirer
8.  Importer les données des élections 2000 à 2016 à partir du [MIT Election Lab](https://electionlab.mit.edu/data)?
    Les données peuvent être directement requêtées depuis l'url
    <https://dataverse.harvard.edu/api/access/datafile/3641280?gbrecs=false>
9.  Créer une variable `share` comptabilisant la part des votes pour chaque candidat.
    Ne garder que les colonnes `"year", "FIPS", "party", "candidatevotes", "share"`
10. Faire une conversion `long` to `wide` avec la méthode `pivot_table` pour garder une ligne
    par comté x année avec en colonnes les résultats de chaque candidat dans cet état.
11. Merger à partir du code FIPS au reste de la base.
    {{% /box %}}

Si vous ne faites pas l'exercice 1, pensez à charger les données en executant la fonction `get_data.py` :

``` python
#!pip install geopandas

import requests

url = 'https://raw.githubusercontent.com/linogaliana/python-datascientist/master/content/course/modelisation/get_data.py'
r = requests.get(url, allow_redirects=True)
open('getdata.py', 'wb').write(r.content)

import getdata
votes = getdata.create_votes_dataframes()
```

Ce code introduit une base nommée `votes` dans l'environnement. Il s'agit d'une
base rassemblant les différentes sources. Elle a l'aspect
suivant:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>STATEFP</th>
      <th>COUNTYFP</th>
      <th>COUNTYNS</th>
      <th>AFFGEOID</th>
      <th>GEOID</th>
      <th>NAME</th>
      <th>LSAD</th>
      <th>ALAND</th>
      <th>AWATER</th>
      <th>geometry</th>
      <th>...</th>
      <th>share_2008_democrat</th>
      <th>share_2008_other</th>
      <th>share_2008_republican</th>
      <th>share_2012_democrat</th>
      <th>share_2012_other</th>
      <th>share_2012_republican</th>
      <th>share_2016_democrat</th>
      <th>share_2016_other</th>
      <th>share_2016_republican</th>
      <th>winner</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>29</td>
      <td>227</td>
      <td>00758566</td>
      <td>0500000US29227</td>
      <td>29227</td>
      <td>Worth</td>
      <td>06</td>
      <td>690564983</td>
      <td>493903</td>
      <td>POLYGON ((-94.63203 40.57176, -94.53388 40.570...</td>
      <td>...</td>
      <td>0.363714</td>
      <td>0.034072</td>
      <td>0.602215</td>
      <td>0.325382</td>
      <td>0.041031</td>
      <td>0.633588</td>
      <td>0.186424</td>
      <td>0.041109</td>
      <td>0.772467</td>
      <td>republican</td>
    </tr>
    <tr>
      <th>1</th>
      <td>31</td>
      <td>061</td>
      <td>00835852</td>
      <td>0500000US31061</td>
      <td>31061</td>
      <td>Franklin</td>
      <td>06</td>
      <td>1491355860</td>
      <td>487899</td>
      <td>POLYGON ((-99.17940 40.35068, -98.72683 40.350...</td>
      <td>...</td>
      <td>0.284794</td>
      <td>0.019974</td>
      <td>0.695232</td>
      <td>0.250000</td>
      <td>0.026042</td>
      <td>0.723958</td>
      <td>0.149432</td>
      <td>0.045427</td>
      <td>0.805140</td>
      <td>republican</td>
    </tr>
    <tr>
      <th>2</th>
      <td>36</td>
      <td>013</td>
      <td>00974105</td>
      <td>0500000US36013</td>
      <td>36013</td>
      <td>Chautauqua</td>
      <td>06</td>
      <td>2746047476</td>
      <td>1139407865</td>
      <td>POLYGON ((-79.76195 42.26986, -79.62748 42.324...</td>
      <td>...</td>
      <td>0.495627</td>
      <td>0.018104</td>
      <td>0.486269</td>
      <td>0.425017</td>
      <td>0.115852</td>
      <td>0.459131</td>
      <td>0.352012</td>
      <td>0.065439</td>
      <td>0.582550</td>
      <td>republican</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>181</td>
      <td>01008591</td>
      <td>0500000US37181</td>
      <td>37181</td>
      <td>Vance</td>
      <td>06</td>
      <td>653713542</td>
      <td>42178610</td>
      <td>POLYGON ((-78.49773 36.51467, -78.45728 36.541...</td>
      <td>...</td>
      <td>0.630827</td>
      <td>0.004743</td>
      <td>0.364429</td>
      <td>0.638870</td>
      <td>0.004891</td>
      <td>0.356239</td>
      <td>0.612154</td>
      <td>0.020824</td>
      <td>0.367022</td>
      <td>democrats</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47</td>
      <td>183</td>
      <td>01639799</td>
      <td>0500000US47183</td>
      <td>47183</td>
      <td>Weakley</td>
      <td>06</td>
      <td>1503107848</td>
      <td>3707114</td>
      <td>POLYGON ((-88.94916 36.41010, -88.81642 36.410...</td>
      <td>...</td>
      <td>0.335720</td>
      <td>0.017458</td>
      <td>0.646822</td>
      <td>0.287590</td>
      <td>0.014914</td>
      <td>0.697495</td>
      <td>0.227511</td>
      <td>0.033158</td>
      <td>0.739330</td>
      <td>republican</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 383 columns</p>
</div>

La carte choroplèthe suivante permet de visualiser rapidement les résultats
(l'Alaska et Hawaï ont été exclus).

``` python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

color_dict = {'republican': '#FF0000', 'democrats': '#0000FF'}
#votes.plot(column = "winner", figsize = (12,12), c=votes['winner'].map(color_dict))

fig, ax = plt.subplots(figsize = (12,12))
grouped = votes.groupby('winner')
for key, group in grouped:
    group.plot(ax=ax, column='winner', label=key, color=color_dict[key])
plt.axis('off')

# plt.show()
```

    (-127.6146362, -64.0610978, 23.253819649999997, 50.628669349999996)

![](index_files/figure-gfm/cell-5-output-2.png)

``` python
ax.get_figure()
```

![](index_files/figure-gfm/cell-6-output-1.png)

Les cartes choroplèthes peuvent donner une impression fallacieuse ayant servi
de justification pour contester les résultats du vote. En effet, un biais
connu des représentations choroplèthes est qu'elles donnent une importance
visuelle excessive aux grands espaces. Or, ceux-ci sont souvent des espaces
peu denses et influencent donc moins la variable d'intérêt (en l'occurrence
le taux de vote en faveur des républicains/démocrates). Une représentation à
privilégier pour ce type de phénomènes est les ronds proportionnels.

Le [GIF "Land does not vote, people do"](https://www.core77.com/posts/90771/A-Great-Example-of-Better-Data-Visualization-This-Voting-Map-GIF)
qui avait eu un certain succès en 2020 propose un autre mode de visualisation.
La carte originale a probablement été construite avec `JavaScript`. Cependant,
on dispose avec `Python` pour répliquer, à faible coût, cette approche avec
l'une des surcouches à JavaScript vue dans la partie [visualisation](#visualisation).

En l'occurrence, on peut utiliser `plotly` pour tenir compte de la population:

{{< plotly "people_vote" >}}

La Figure a été obtenue avec le code suivant:

``` python
import plotly
import plotly.graph_objects as go
import pandas as pd
import geopandas as gpd


centroids = votes.copy()
centroids.geometry = centroids.centroid
centroids['size'] = centroids['CENSUS_2010_POP'] / 10000  # to get reasonable plotable number

color_dict = {"republican": '#FF0000', 'democrats': '#0000FF'}
centroids["winner"] =  np.where(centroids['votes_gop'] > centroids['votes_dem'], 'republican', 'democrats') 


centroids['lon'] = centroids['geometry'].x
centroids['lat'] = centroids['geometry'].y
centroids = pd.DataFrame(centroids[["county_name",'lon','lat','winner', 'CENSUS_2010_POP',"state_name"]])
groups = centroids.groupby('winner')

df = centroids.copy()

df['color'] = df['winner'].replace(color_dict)
df['size'] = df['CENSUS_2010_POP']/6000
df['text'] = df['CENSUS_2010_POP'].astype(int).apply(lambda x: '<br>Population: {:,} people'.format(x))
df['hover'] = df['county_name'].astype(str) +  df['state_name'].apply(lambda x: ' ({}) '.format(x)) + df['text']

fig_plotly = go.Figure(data=go.Scattergeo(
    locationmode = 'USA-states',
    lon=df["lon"], lat=df["lat"],
    text = df["hover"],
    mode = 'markers',
    marker_color = df["color"],
    marker_size = df['size'],
    hoverinfo="text"
    ))

fig_plotly.update_traces(
  marker = {'opacity': 0.5, 'line_color': 'rgb(40,40,40)', 'line_width': 0.5, 'sizemode': 'area'}
)

fig_plotly.update_layout(
        title_text = "Reproduction of the \"Acres don't vote, people do\" map <br>(Click legend to toggle traces)",
        showlegend = True,
        geo = {"scope": 'usa', "landcolor": 'rgb(217, 217, 217)'}
    )

fig_plotly.show()
```


## Explorer la structure des données

La première étape nécessaire à suivre avant de se lancer dans la modélisation
est de déterminer les variables à inclure dans le modèle.

Les fonctionnalités de `pandas` sont, à ce niveau, suffisantes pour explorer des structures simples. Néanmoins, lorsqu'on est face à un jeu de données présentant de nombreuses variables explicatives (*features* en machine learning, *covariates* en économétrie), il est souvent judicieux d'avoir une première étape de sélection de variable, ce que nous verrons par la suite dans la [partie dédiée](https://linogaliana-teaching.netlify.app/lasso/).

Avant d'être en mesure de sélectionner le meilleur ensemble de variables explicatives, nous allons en prendre un nombre restreint et arbitraire. La première tâche est de représenter les relations entre les données, notamment la relation des variables explicatives à la variable dépendante (le score du parti républicain) ainsi que les relations entre les variables explicatives.

{{% box status="exercise" title="Exercice" icon="fas fa-pencil-alt" %}}

**Exercice 2 : Regarder les corrélations entre les variables**

1.  Créer un DataFrame `df2` plus petit avec les variables `winner` et
    `votes_gop`, `Unemployment_rate_2019`,
    `Median_Household_Income_2019`,
    `Percent of adults with less than a high school diploma, 2015-19`,
    `Percent of adults with a bachelor's degree or higher, 2015-19`

2.  Représenter grâce à un graphique la matrice de corrélation avec `heatmap` de `seaborn`.

La matrice construite avec `seaborn` aura un aspect comme suit:

``` python
g1.figure.get_figure()
```

![](index_files/figure-gfm/cell-11-output-1.png)

Alors que celle construite directement avec `pandas`
ressemblera plutôt à ce tableau :

``` python
g2
```

<style type="text/css">
#T_5a075_row0_col0, #T_5a075_row1_col1, #T_5a075_row2_col2, #T_5a075_row3_col3, #T_5a075_row4_col4 {
  background-color: #b40426;
  color: #f1f1f1;
}
#T_5a075_row0_col1 {
  background-color: #8caffe;
  color: #000000;
}
#T_5a075_row0_col2 {
  background-color: #edd2c3;
  color: #000000;
}
#T_5a075_row0_col3 {
  background-color: #9fbfff;
  color: #000000;
}
#T_5a075_row0_col4 {
  background-color: #f2c9b4;
  color: #000000;
}
#T_5a075_row1_col0 {
  background-color: #4358cb;
  color: #f1f1f1;
}
#T_5a075_row1_col2, #T_5a075_row4_col1 {
  background-color: #4a63d3;
  color: #f1f1f1;
}
#T_5a075_row1_col3 {
  background-color: #f1ccb8;
  color: #000000;
}
#T_5a075_row1_col4 {
  background-color: #6a8bef;
  color: #f1f1f1;
}
#T_5a075_row2_col0 {
  background-color: #c5d6f2;
  color: #000000;
}
#T_5a075_row2_col1, #T_5a075_row3_col0, #T_5a075_row3_col2, #T_5a075_row3_col4, #T_5a075_row4_col3 {
  background-color: #3b4cc0;
  color: #f1f1f1;
}
#T_5a075_row2_col3 {
  background-color: #4961d2;
  color: #f1f1f1;
}
#T_5a075_row2_col4 {
  background-color: #e97a5f;
  color: #f1f1f1;
}
#T_5a075_row3_col1 {
  background-color: #ead5c9;
  color: #000000;
}
#T_5a075_row4_col0 {
  background-color: #cbd8ee;
  color: #000000;
}
#T_5a075_row4_col2 {
  background-color: #ec7f63;
  color: #f1f1f1;
}
</style>
<table id="T_5a075">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_5a075_level0_col0" class="col_heading level0 col0" >votes_gop</th>
      <th id="T_5a075_level0_col1" class="col_heading level0 col1" >Unemployment_rate_2019</th>
      <th id="T_5a075_level0_col2" class="col_heading level0 col2" >Median_Household_Income_2019</th>
      <th id="T_5a075_level0_col3" class="col_heading level0 col3" >Percent of adults with less than a high school diploma, 2015-19</th>
      <th id="T_5a075_level0_col4" class="col_heading level0 col4" >Percent of adults with a bachelor's degree or higher, 2015-19</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_5a075_level0_row0" class="row_heading level0 row0" >votes_gop</th>
      <td id="T_5a075_row0_col0" class="data row0 col0" >1.00</td>
      <td id="T_5a075_row0_col1" class="data row0 col1" >-0.08</td>
      <td id="T_5a075_row0_col2" class="data row0 col2" >0.35</td>
      <td id="T_5a075_row0_col3" class="data row0 col3" >-0.11</td>
      <td id="T_5a075_row0_col4" class="data row0 col4" >0.37</td>
    </tr>
    <tr>
      <th id="T_5a075_level0_row1" class="row_heading level0 row1" >Unemployment_rate_2019</th>
      <td id="T_5a075_row1_col0" class="data row1 col0" >-0.08</td>
      <td id="T_5a075_row1_col1" class="data row1 col1" >1.00</td>
      <td id="T_5a075_row1_col2" class="data row1 col2" >-0.43</td>
      <td id="T_5a075_row1_col3" class="data row1 col3" >0.36</td>
      <td id="T_5a075_row1_col4" class="data row1 col4" >-0.36</td>
    </tr>
    <tr>
      <th id="T_5a075_level0_row2" class="row_heading level0 row2" >Median_Household_Income_2019</th>
      <td id="T_5a075_row2_col0" class="data row2 col0" >0.35</td>
      <td id="T_5a075_row2_col1" class="data row2 col1" >-0.43</td>
      <td id="T_5a075_row2_col2" class="data row2 col2" >1.00</td>
      <td id="T_5a075_row2_col3" class="data row2 col3" >-0.51</td>
      <td id="T_5a075_row2_col4" class="data row2 col4" >0.71</td>
    </tr>
    <tr>
      <th id="T_5a075_level0_row3" class="row_heading level0 row3" >Percent of adults with less than a high school diploma, 2015-19</th>
      <td id="T_5a075_row3_col0" class="data row3 col0" >-0.11</td>
      <td id="T_5a075_row3_col1" class="data row3 col1" >0.36</td>
      <td id="T_5a075_row3_col2" class="data row3 col2" >-0.51</td>
      <td id="T_5a075_row3_col3" class="data row3 col3" >1.00</td>
      <td id="T_5a075_row3_col4" class="data row3 col4" >-0.59</td>
    </tr>
    <tr>
      <th id="T_5a075_level0_row4" class="row_heading level0 row4" >Percent of adults with a bachelor's degree or higher, 2015-19</th>
      <td id="T_5a075_row4_col0" class="data row4 col0" >0.37</td>
      <td id="T_5a075_row4_col1" class="data row4 col1" >-0.36</td>
      <td id="T_5a075_row4_col2" class="data row4 col2" >0.71</td>
      <td id="T_5a075_row4_col3" class="data row4 col3" >-0.59</td>
      <td id="T_5a075_row4_col4" class="data row4 col4" >1.00</td>
    </tr>
  </tbody>
</table>

3.  Choisir quelques variables (pas plus de 4 ou 5) et représenter une matrice de nuages de points

<!-- -->

    array([[<AxesSubplot:xlabel='votes_gop', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='votes_gop'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='votes_gop'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Unemployment_rate_2019'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Median_Household_Income_2019'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Percent of adults with less than a high school diploma, 2015-19'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">]],
          dtype=object)

4.  (optionnel) Refaire ces figures avec `plotly` qui offre également la possibilité de faire une matrice de corrélation. Le résultat devrait ressembler au graphique suivant :

{{% /box %}}

{{< chart data="scatter_matrix" >}}

## Transformer les données

Les différences d'échelle ou de distribution entre les variables peuvent
diverger des hypothèses sous-jacentes dans les modèles.

Par exemple, dans le cadre
de la régression linéaire, les variables catégorielles ne sont pas traitées à la même
enseigne que les variables ayant valeur dans $\mathbb{R}$. Une variable
discrète (prenant un nombre fini de valeurs) devra être transformées en suite de
variables 0/1 par rapport à une modalité de référence pour être en adéquation
avec les hypothèses de la régression linéaire. On appelle ce type de transformation
*one-hot encoding*, sur lequel nous reviendrons. Il s'agit d'une transformation,
parmi d'autres, disponibles dans `scikit` pour mettre en adéquation un jeu de
données et des hypothèses mathématiques.

L'ensemble de ces tâches s'appelle le *preprocessing*. L'un des intérêts
d'utiliser `scikit` est qu'on peut considérer qu'une tâche de preprocessing
est une tâche d'apprentissage (on apprend des paramètres d'une structure
de données) qui est réutilisable pour un jeu de données à la structure
similaire:

![](scikit_predict.png)

### Standardisation

La standardisation consiste à transformer des données pour que la distribution empirique suive une loi $\mathcal{N}(0,1)$. Pour être performants, la plupart des modèles de machine learning nécessitent souvent d'avoir des données dans cette distribution.

{{% box status="warning" title="Warning" icon="fa fa-exclamation-triangle" %}}
Pour un statisticien, le terme `normalization` dans le vocable `scikit` peut avoir un sens contre-intuitif. On s'attendrait à ce que la normalisation consiste à transformer une variable de manière à ce que $X \sim \mathcal{N}(0,1)$. C'est, en fait, la **standardisation** en `scikit`.

La **normalisation** consiste à modifier les données de manière à avoir une norme unitaire. La raison est expliquée plus bas.
{{% /box %}}

{{% box status="exercise" title="Exercice" icon="fas fa-pencil-alt" %}}

**Exercice 3: Standardisation**

1.  Standardiser la variable `Median_Household_Income_2019` (ne pas écraser les valeurs !) et regarder l'histogramme avant/après normalisation.

<!-- -->

    array([[<AxesSubplot:xlabel='votes_gop', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='votes_gop'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='votes_gop'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='votes_gop'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Unemployment_rate_2019'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Unemployment_rate_2019'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Median_Household_Income_2019'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Median_Household_Income_2019'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel='Percent of adults with less than a high school diploma, 2015-19'>,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel='Percent of adults with less than a high school diploma, 2015-19'>],
           [<AxesSubplot:xlabel='votes_gop', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Unemployment_rate_2019', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Median_Household_Income_2019', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel='Percent of adults with less than a high school diploma, 2015-19', ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">,
            <AxesSubplot:xlabel="Percent of adults with a bachelor's degree or higher, 2015-19", ylabel="Percent of adults with a bachelor's degree or higher, 2015-19">]],
          dtype=object)

*Note : On obtient bien une distribution centrée à zéro et on pourrait vérifier que la variance empirique soit bien égale à 1. On pourrait aussi vérifier que ceci est vrai également quand on transforme plusieurs colonnes à la fois.*

2.  Créer `scaler`, un `Transformer` que vous construisez sur les 1000 premières lignes de votre DataFrame. Vérifier la moyenne et l'écart-type de chaque colonne sur ces mêmes observations.

*Note : Les paramètres qui seront utilisés pour une standardisation ultérieure sont stockés dans les attributs `.mean_` et `.scale_`*

On peut voir ces attributs comme des paramètres entraînés sur un certain jeu de
données et qu'on peut réutiliser sur un autre, à condition que les
dimensions coïncident.

3.  Appliquer `scaler` sur les autres lignes du DataFrame et comparer les distributions obtenues de la variable `Median_Household_Income_2019`.

<!-- -->

    array([<AxesSubplot:ylabel='Density'>, <AxesSubplot:ylabel='Density'>],
          dtype=object)

*Note : Une fois appliqués à un autre `DataFrame`, on peut remarquer que la distribution n'est pas exactement centrée-réduite dans le `DataFrame` sur lequel les paramètres n'ont pas été estimés. C'est normal, l'échantillon initial n'était pas aléatoire, les moyennes et variances de cet échantillon n'ont pas de raison de coïncider avec les moments de l'échantillon complet.*

{{% /box %}}

### Normalisation

La **normalisation** est l'action de transformer les données de manière à obtenir une norme ($\mathcal{l}_1$ ou $\mathcal{l}_2$) unitaire. Autrement dit, avec la norme adéquate, la somme des éléments est égale à 1. Par défaut, la norme est dans $\mathcal{l}_2$. Cette transformation est particulièrement utilisée en classification de texte ou pour effectuer du *clustering*.

{{% box status="exercise" title="Exercice" icon="fas fa-pencil-alt" %}}

**Exercice 4 : Normalisation**

1.  Normaliser la variable `Median_Household_Income_2019` (ne pas écraser les valeurs !) et regarder l'histogramme avant/après normalisation.

<!-- -->

    array([<AxesSubplot:xlabel='Median_Household_Income_2019', ylabel='Density'>,
           <AxesSubplot:ylabel='Density'>], dtype=object)

2.  Vérifier que la norme $\mathcal{l}_2$ est bien égale à 1.

{{% /box %}}

{{% box status="warning" title="Warning" icon="fa fa-exclamation-triangle" %}}
`preprocessing.Normalizer` n'accepte pas les valeurs manquantes, alors que `preprocessing.StandardScaler()` s'en accomode (dans la version `0.22` de scikit). Pour pouvoir aisément appliquer le *normalizer*, il faut

-   retirer les valeurs manquantes du DataFrame avec la méthode `dropna`: `df.dropna(how = "any")`;
-   ou les imputer avec un modèle adéquat. [`scikit` permet de le faire](https://scikit-learn.org/stable/modules/preprocessing.html#imputation-of-missing-values).
    {{% /box %}}

### Encodage des valeurs catégorielles

Les données catégorielles doivent être recodées sous forme de valeurs numériques pour être intégrables dans le cadre d'un modèle. Cela peut être fait de plusieurs manières :

-   `LabelEncoder`: transforme un vecteur `["a","b","c"]` en vecteur numérique `[0,1,2]`. Cette approche a l'inconvénient d'introduire un ordre dans les modalités, ce qui n'est pas toujours souhaitable

-   `OrdinalEncoder`: une version généralisée du `LabelEncoder` qui a vocation à s'appliquer sur des matrices ($X$), alors que `LabelEncoder` est plutôt pour un vecteur ($y$)

-   `pandas.get_dummies` effectue une opération de *dummy expansion*. Un vecteur de taille *n* avec *K* catégories sera transformé en matrice de taille $n \times K$ pour lequel chaque colonne sera une variable *dummy* pour la modalité *k*. Il y a ici $K$ modalités et il y a donc multicollinéarité. Avec une régression linéaire avec constante, il convient de retirer une modalité avant l'estimation.

-   `OneHotEncoder` est une version généralisée (et optimisée) de la *dummy expansion*. Il a plutôt vocation à s'appliquer sur les *features* ($X$) du modèle

{{% box status="exercise" title="Exercice" icon="fas fa-pencil-alt" %}}

**Exercice 5 : Encoder des variables catégorielles**

1.  Créer `df` qui conserve uniquement les variables `state_name` et `county_name` dans `votes`.

2.  Appliquer à `state_name` un `LabelEncoder`

*Note : Le résultat du label encoding est relativement intuitif, notamment quand on le met en relation avec le vecteur initial.*

3.  Regarder la *dummy expansion* de `state_name`

4.  Appliquer un `OrdinalEncoder` à `df[['state_name', 'county_name']]`

*Note : Le résultat du *ordinal encoding\* est cohérent avec celui du label encoding\*

5.  Appliquer un `OneHotEncoder` à `df[['state_name', 'county_name']]`

*Note : `scikit` optimise l'objet nécessaire pour stocker le résultat d'un modèle de transformation. Par exemple, le résultat de l'encoding One Hot est un objet très volumineux. Dans ce cas, scikit utilise une matrice Sparse.*

{{% /box %}}
