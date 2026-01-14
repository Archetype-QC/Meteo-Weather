# Fichier météo historique du Québec

Ce répertoire contient des données météorologiques historique pour les années à partir de 2020 pour 16 localisations au Québec sous différents formats associés aux outils de simulation suivants : **EnergyPlus** (.epw), **eQuest** (.bin) et **SIMEB** (.swdf)


- [Structures des fichiers](#structure-des-fichiers)
- [Source des données](#source-des-données)
- [Traitement des données](#traitement-des-données)
- [Variables météorologiques](#variables-météorologiques)
- [Station météorologiques](#stations-météorologiques)

Essentiellement, il s'agit des données issues du traitement disponible anciennement sur le site de simeb.ca, mis à part les données de rayonnement solaire.

## Structure des fichiers


On retrouve un fichier zippé pour chacune des années 2020 à 2024, nommé "DemandeXXXX-01-01_000003.zip" où XXXX corespond à l'année. 

Chaque fichier zippé contient les fichiers suivant pour les 22 stations météorologiques :
- les 3 formats de ficher (.epw, .bin, .swdf) du 1er janvier au 31 décembre<sup><a href="#note1">1</a></sup> de l'année spécifié
- le fichier en mode texte (_ascii.txt) du 1er janvier au 31 décembre de l'année spécifié
- les fichiers de conditions de design en chauffage (_dd_ch.txt) et de climatisation (_dd_cl.txt) 

<div id="note1">
<sup>1</sup> Pour une année bisextile, la journée du 29 février est omise du fichier et les variables météorologiques sont décalées d'une journée à partir de cette date jusqu'au 31 décembre
</div>

## Source des données

Les données source de chacune des stations proviennent essentiellement d'environnement Canada pour les variables suivantes : température sèche, point de rosée, humidité relative, pression atmosphérique, vitesse du vent et direction du vent.

Le rayonnement solaire total sur surface horizontal provient des données réanalysées ERA5 land associé à la localisation des chacune stations. Cette source de données offre une  précision adequate et une meilleure disponibilité que les valeurs issues des mesures aux stations. https://cds.climate.copernicus.eu/datasets/reanalysis-era5-land?tab=overview

Les conditions de design de la localité choisie (dd_ch.txt, dd_cl.txt) proviennent de l'ASHRAE Handbook of Fundamentals (Edition 2009, chapitre 14). Les conditions de design en chauffage et en climatisation correspondent respectivement à 99% et 1% d'occurrence de la fréquence annuelle cumulative de la température de bulbe sec.


## Traitement des données 

Les données ont été traitées automatiquement afin d'éviter les manques, les sauts de valeurs ou les valeurs impossibles. Un algorithme permet d'interpoler les valeurs s'il y a un manque de 4 heures ou moins. En l'absence de données sur une grande période, un modèle permet de générer des valeurs à partir des valeurs de stations météorologiques avoisinantes qui sont corrélées. Dans tous les cas, les codes d'erreur de chacune des variables traitées peuvent être affichés afin de qualifier la qualité des données météo extraites.

1- Le fichier météo de format DOE-2 (.bin) est créé à l'aide du programme DOEWTH décrit dans le document suivant : http://gundog.lbl.gov/dirun/2001weath.pdf. Les données météo brutes ont été préalablement formatées selon le format prédéfini « Other » (unpacked file option)

2- Le fichier météo de format EnergyPlus (.epw) est généré en utilisant le programme WeatherConverter inclus lors de l'installation d'EnergyPLusEnergyPlus. Les données provenant du fichier DOE-2 préalablement créé sont utilisées comme intrant

3- Le fichier de météo SIMEB (.swdf) est un fichier zippé contenant à la fois le fichier météo EnergyPlus et DOE-2, de même que les conditions de design en chauffage et en climatisation. 


## Variables météorologiques

Le tableau suivantes présentes une description des variables météorologiques du fichier ".txt.


<table border="1" width="591" height="424">
  <tr> 
    <td width="206" height="36" align="center"><strong>Libellé</strong></td>
    <td width="176" align="center"><strong>Informations additionnelles</strong></td>
    <td width="101" align="center"><strong>Variable</strong></td>
    <td width="74" align="center"><strong>Unités</strong></td>
  </tr>
  <tr> 
    <td height="19" align="left">Nombre d'heures depuis le 1er janvier 1904</td>
    <td align="left">Même convention utilisée dans Excel</td>
    <td align="center">nbreheures1904</td>
    <td align="center">h UTC</td>
  </tr>
  <tr>
    <td height="19" align="left">Heure d'été ou d'hiver</td>
    <td align="left">Indication de l'heure normale ou avancée de l'est</td>
    <td align="center">tsl</td>
    <td align="center">HNE/HAE</td>
  </tr>
  <tr> 
    <td height="19" align="left">Date UTC</td>
    <td align="left">Date correspondant au temps universel coordonné</td>
    <td align="center">Date utc</td>
    <td align="center">N/A</td>
  </tr>
  <tr> 
    <td height="19" align="left">Heure UTC</td>
    <td align="left">Heure correspondant au temps universel coordonné</td>
    <td align="center">Heure utc</td>
    <td></td>
  </tr>
  <tr> 
    <td height="19" align="left">Date locale</td>
    <td align="left">Date UTC -5 ou -4</td>
    <td align="center">Date locale</td>
    <td></td>
  </tr>
  <tr> 
    <td height="19" align="left">Heure locale</td>
    <td align="left">Heure UTC -5 ou -4</td>
    <td align="center">Heure locale</td>
    <td></td>
  </tr>
  <tr> 
    <td height="19" align="left">Température [°C]</td>
    <td align="left">Température sèche</td>
    <td align="center">t</td>
    <td align="center">&deg;C</td>
  </tr>
  <tr>
    <td height="19" align="left">Température humide [&deg;C]</td>
    <td align="left">Température de bulbe humide</td>
    <td align="center">td</td>
    <td align="center">°C</td>
  </tr>
  <tr>
    <td height="19" align="left">Humidité relative [%]</td>
    <td align="left">Humidité relative</td>
    <td align="center">hr</td>
    <td align="center">%</td>
  </tr>
  <tr>
    <td height="19" align="left">Vitesse du vent [km/h]</td>
    <td align="left">Vitesse du vent</td>
    <td align="center">w</td>
    <td align="center">km/h</td>
  </tr>
  <tr>
    <td height="19" align="left">Direction du vent [0-360°]</td>
    <td align="left">Direction du vent par rapport au nord</td>
    <td align="center">vd</td>
    <td align="center">°</td>
  </tr>
  <tr>
    <td height="19" align="left">Pression atmosphérique [hPa]</td>
    <td align="left">Pression atmosphérique</td>
    <td align="center">psta</td>
    <td align="center">hPa</td>
  </tr>
  <tr>
    <td height="19" align="left">Rayonnement solaire total sur une surface horizontale [kJ/m&sup2;/h]</td>
    <td align="left">Rayonnement RF1</td>
    <td align="center">rfle</td>
    <td align="center">kJ/m&sup2;/h</td>
  </tr>
  <tr>
    <td height="19" align="left">Code d'erreur sur <<Variable>></td>
    <td align="left">(Voir tableau suivant)</td>
    <td></td>
    <td></td>
  </tr>
</table>

<br>
Le tableau suivant liste les codes d'erreur associés aux variables météorologiques inscrit dans le fichier ".txt".


<br>
<table width="591" border="1">
  <tr>
    <td width="165" align="center"><strong>Libellé</strong></td>
    <td width="383" align="center"><strong>Informations additionnelles</strong></td>
  </tr>
  <tr>
    <td>Code 4</td>
    <td>Donnée obtenue par interpolation linéaire</td>
  </tr>
  <tr>
    <td>Code 5.k=X.Y</td>
    <td><p>Donnée obtenue par corrélation avec une station voisine et raccordée<br>
      X : Rang de la station utilisée<br>
      Y : Nom de la station utilisée</p></td>
  </tr>
  <tr>
    <td>Code 6</td>
    <td>Idem code 5, sauf que la donnée n'est pas raccordée en aval</td>
  </tr>
  <tr>
    <td>Code 8</td>
    <td>Mise à zéro du rayonnement (supérieur à 20 W/m2) la nuit</td>
  </tr>
  <tr>
    <td>ERA 5</td>
    <td>Donnée réanalisée provenant de ERA5 land<sup><a href="#note1">2</a></sup> </td>
  </tr>
</table>



<div id="note1">
<sup>2</sup> Copernicus Climate Change Service (C3S)(2019): ERA5-Land hourly data from 1950 to present. Copernicus Climate Change Service (C3S) Climate Data Store (CDS). DOI: 10.24381/cds.e2161bac
</div>


## Stations météorologiques

Le tableau suivant liste la localisation des 16 stations météorolgiques

<table border="1" width="591" height="200">
  <tr height="27">
    <td width="280" ><strong>Nom de la station météo</strong></td>
    <td width="350"><strong>Région administrative</strong></td>
    <td width="111"><strong>Latitude (°)</strong></td>
    <td width="114"><strong>Longitude (°)</strong></td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Bagotville</td>
    <td align="left">Saguenay-Lac-Saint-Jean</td>
    <td>48,331</td>
    <td>-70,996</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Beauceville</td>
    <td align="left">Chaudière-Appalaches</td>
    <td>46,200</td>
    <td>-70,780</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Chibougameau-Chapais</td>
    <td align="left">Nord-du-Québec</td>
    <td>49,772</td>
    <td>-74,528</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Gaspé</td>
    <td align="left">Gaspésie-Îles-de-la-Madeleine</td>
    <td>48,775</td>
    <td>-64,479</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">L'Acadie</td>
    <td align="left">Montérégie</td>
    <td>45,300</td>
    <td>-73,350</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">L'Assomption</td>
    <td align="left">Lanaudière</td>
    <td>45,820</td>
    <td>-73,430</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Lemieux</td>
    <td align="left">Centre-du-Québec</td>
    <td>46,430</td>
    <td>-71,930</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Mont-Joli</td>
    <td align="left">Bas-Saint-Laurent</td>
    <td>48,609</td>
    <td>-68,208</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Montréal (Dorval)</td>
    <td align="left">Montréal-Laval</td>
    <td>45,471</td>
    <td>-73,741</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Ottawa</td>
    <td align="left">Outaouais</td>
    <td>45,323</td>
    <td>-75,669</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Québec</td>
    <td align="left">Capitale-Nationale</td>
    <td>46,791</td>
    <td>-71,393</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Rouyn (AU7)</td>
    <td align="left">Abitibi-Témiscamingue</td>
    <td>48,206</td>
    <td>-78,836</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Sept-Iles</td>
    <td align="left">Côte-Nord</td>
    <td>50,223</td>
    <td>-66,266</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Sherbrooke</td>
    <td align="left">Estrie</td>
    <td>45,439</td>
    <td>-71,691</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">St-Jovite</td>
    <td align="left">Laurentides</td>
    <td>46,070</td>
    <td>-74,530</td>
  </tr>
  <tr height="27" align="center">
    <td align="left">Trois-Rivières</td>
    <td align="left">Mauricie</td>
    <td>46,350</td>
    <td>-72,520</td>
  </tr>
</table>