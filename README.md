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

En este proyecto habremos de efectuar un proceso completo de ETL, las siglas en inglés de Extract, Transform y Load. Los datos a extraer serán de nuestra elección, pero habrá que cumplir ciertos requerimientos. En este caso, vamos a extraer datos y hacer un pequeño análisis acerca de las decisiones arbitrales en la NFL.

### Restricciones:
- Obtener la información de tres fuentes distintas (urls).
- Dos métodos distintos de extracción (csv, excel, api, rss, web scrapping...).

### Objetivo:
 
Nuestro objetivo es encontrar tres fuentes de datos distintas sobre las señalizaciones arbitrales en la NFL, de tal modo que podamos generar un DataFrame rico y completo a través del cual podamos sacar conclusiones al respecto. En este sentido, trabajaremos en la transformación del dato crudo para su adaptación al resto de las fuentes, con el fin de establecer una base de datos SQL con los resultados, pudiendo lanzar queries que nos ayuden a confirmar o refutar las hipótesis que planteemos.

 
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

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/pass_vs_rush.png" width="500" height="800" />

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

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/pass_ratings.png" width="500" height="800" />

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


<a name="special"/>

## Special Teams
	
El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/spe_teams.png" width="500" height="800" />

<details>
<summary>Field Goald %</summary>
<br>

 ![fg](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/fg.png)

</details>

<details>
<summary>Punt Attempts</summary>
<br>

 ![punt](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/punt.png)

</details>

<details>
<summary>Field Goals by Team</summary>
<br>

![fg_teams](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/profootballreference.png)
	

<br>
	
<a name="defense"/>

## Defense
	
El proceso de transformación por cada tabla fue el siguiente:

<img src="https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/profootballnetwork.png" width="500" height="800" />

<details>
<summary>https://www.nflpenalties.com/</summary>
<br>

 ![nflpenalties](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/nflpenalties.png)

</details>

<details>
<summary>https://www.pro-football-reference.com/</summary>
<br>

 ![profootballreference](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/profootballreference.png)

</details>

<details>
<summary>https://www.profootballnetwork.com/</summary>
<br>

![profootballreference](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/profootballreference.png)
	
	
<a name="conclusion"/>

## Conclusiones
	
<p><strong> Los árbitros tienden a buscar un final igualado, y por tanto benefician al equipo que va por debajo en el marcador.</strong>
