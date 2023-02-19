# W5 Project - NFL Success Visualization

![portada](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/portada.png)

## Índice

1. [Descripción](#descripción)
2. [Wins by Team](#wins_by_team)
3. [Passing vs Rushing](#pass_rush)
4. [QB Dependency](#qb)
5. [Special Teams](#special)
6. [Defense](#defense)
7. [Conclusiones](#conclusion)


<a name="descripción"/>

## Descripción del proyecto

En este proyecto habremos de visualizar los datos cargados tras efectuar una ETL, mediante el uso de herramientas como PowerBI, Tableau o ciertas librerías de Python como Matplotlib o Seaborn, con el fin mejorar el análisis de los mismos. Para este proyecto concreto, nosotros utilizaremos PowerBI, lo que podrá permitir, a cualquiera que no conozca los datos, entenderlos a golpe de vista.

### Restricciones:
- Utilizar una de las tres herramientas mencionadas.

### Dataset:
- Utilizaremos un csv con todas las estadísticas de juego de los equipos de la NFL entre 2010 y 2016.

### Objetivo:
 
Nuestro objetivo será presentar una serie de figuras gráficas que nos ayuden a dilucidar visualmente una cuestión planteada, que este caso será la siguiente:

<p><strong> ¿Qué factores del juego son más determinantes para el resultado final?</strong>

 
 <a name="wins_by_team"/>
 
## Wins by Team

En primer lugar, hemos realizado un ejercicio analítico de numerosas páginas web, incluyendo Kaggle o Google Dataset Search. Al no encontrar ningún dataset que se ajustara al objetivo, ampliamos la búsqueda a cualquier url que pudiera proporcionar la información requerida. Durante este proceso, encontramos las siguientes tablas:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/wins.png" />

Para la primera url, el proceso de extracción consistió en hacer web scrapping, utilizando la librería selenium. Tras conseguir tanto los nombres de columna como los datos, mediante la librería pandas generamos nuestro DataFrame principal. A partir de este, la idea fue extraer datos que pudieran complementar los ya existentes.

Como en la primera url conseguimos los datos de "Penalties" de la primera jornada, decidimos completar esa tabla con una columna que devolviera al ganador del partido en cuestión, de modo que pudieramos comprobar la incidencia arbitral en el éxito deportivo. Para ello descargamos un archivo boxscore en formato xlsx con el ganador y perdedor del partido en cuestión, entre otros datos. 

Finalmente decidimos comprobar la incidencia del horario en las decisiones arbitrales, pues existe una creencia popular que asocia los partidos de "prime time" con un número mayor de señalizaciones, ya que los colegiados aparecen más en pantalla cuando se televisa a todo el país. Para ello extraimos un archivo .csv con el "schedule" de la primera jornada.



 <a name="pass_rush"/>
 
## Passing vs Rushing

El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/pass_vs_rush.png" />

<details>
<summary>Score & Passing</summary>
<br>

 ![pass](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/pass_scatter.png)

</details>

<details>
<summary>Score & Rushing</summary>
<br>

 ![rush](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/rush_scatter.png)

</details>

<details>
<summary>Top & Bottom Teams Play Distribution</summary>
<br>

![top](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/top_pass_rush.png)
![bot](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/worst_pass_rush.png)

</details>
<br>


<a name="qb"/>

## QB Dependency

El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/pass_ratings.png" />

<details>
<summary>Average QB Rating by Team</summary>
<br>

 ![qb_rat](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/qb_rat.png)

</details>

<details>
<summary>Top Teams: Passing & Rushing</summary>
<br>

 ![top_pass_rush](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/top_pass_rush.png)

</details>

<details>
<summary>Worst Teams: Passing & Rushing</summary>
<br>

![worst_pass_rush](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/worst_pass_rush.png)

</details>

<a name="special"/>

## Special Teams
	
El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/spe_teams.png" />

<details>
<summary>Field Goald %</summary>
<br>

 ![fg](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/fg.png)

</details>

<details>
<summary>Punt Attempts</summary>
<br>

 [punt](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/punt.png)

</details>

<details>
<summary>Field Goals by Team</summary>
<br>

[fg_teams](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/fg_teams.png)

</details>	

<br>
	
<a name="defense"/>

## Defense
	
El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/key1.png" />

<details>
<summary>Interceptions</summary>
<br>

 ![key1](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/key1.png)

</details>

<details>
<summary>Sacks</summary>
<br>

 ![key2](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/key2.png)

</details>

<details>
<summary>Safeties</summary>
<br>

 ![key3](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/key3.png)

</details>

<details>
<summary>Tackles</summary>
<br>

 ![key4](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/key4.png)

</details>

		
<a name="conclusion"/>

## Conclusiones
	
<p><strong> Los árbitros tienden a buscar un final igualado, y por tanto benefician al equipo que va por debajo en el marcador.</strong>
	
<br>
<br>
