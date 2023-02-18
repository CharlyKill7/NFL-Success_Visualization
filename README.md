# W5 Project - NFL Success Visualization

![portada](https://github.com/CharlyKill7/NFL-Success_Visualization/blob/main/images/portada.png)

## Índice

1. [Descripción](#descripción)
2. [Wins by Team](#wins_by_team)
3. [Passing vs Rushing](#pass_rush)
4. [QB Dependency](#qb)
5. [Special Teams](#special)
6. [Defense](#defense)
7. [Conclusiones](#conclusiones)


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

<img src="https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/profootballnetwork.png" width="500" height="800" />

</details>



**Proceso de extracción**

Para la primera url, el proceso de extracción consistió en hacer web scrapping, utilizando la librería selenium. Tras conseguir tanto los nombres de columna como los datos, mediante la librería pandas generamos nuestro DataFrame principal. A partir de este, la idea fue extraer datos que pudieran complementar los ya existentes.

Como en la primera url conseguimos los datos de "Penalties" de la primera jornada, decidimos completar esa tabla con una columna que devolviera al ganador del partido en cuestión, de modo que pudieramos comprobar la incidencia arbitral en el éxito deportivo. Para ello descargamos un archivo boxscore en formato xlsx con el ganador y perdedor del partido en cuestión, entre otros datos. 

Finalmente decidimos comprobar la incidencia del horario en las decisiones arbitrales, pues existe una creencia popular que asocia los partidos de "prime time" con un número mayor de señalizaciones, ya que los colegiados aparecen más en pantalla cuando se televisa a todo el país. Para ello extraimos un archivo .csv con el "schedule" de la primera jornada.



 <a name="pass_rush"/>
 
## Passing vs Rushing

El proceso de transformación por cada tabla fue el siguiente:

- En la primera url obtuvimos un primer DataFrame gracias al web scrapping y la librería Selenium. Una vez obtenido el DF, optamos por mantenerlo intacto hasta después de conectarlo con la información de las otras dos url. Finalmente, una vez enriquecida esta nuestra tabla principal, decidimos eliminar unas cuantas columnas cuya información, aunque interesante, no parecía valiosa para nuestro objetivo ("Player", "Declined", "Offsetting"...). También rellenamos algunos de los valores vacíos de la columna 'Pos' con NP (No position). La columna 'Time', con los minutos y segundos restantes de partido la tranformamos en segundos y la llamamos 'Time left'. Finalmente ajustamos el tipo de dato para optimizar el Dataframe.

<br>
<img src="https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/df.png" />
<br>
<br>

- En la segunda url conseguimos un archivo xlsx, que también convertimos a DataFrame. Al no tener un índice, decidimos añadirlo. Después, como sólo necesitabamos la información de la primera jornada, eliminamos todas las filas correspondientes a otras fechas del campeonato. A continuación tomamos una decisión que afectó a todos nuestros DFs: unificar los nombres de los equipos bajo las siglas habituales (BAL, BUF, KC, NYG...), lo cual logramos mediante diccionarios con los cambios y la función .map. Una vez unificamos los nombres, añadimos dos columnas ('Winner' y 'Loser') al DF principal. 

<br>
<img src="https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/df2.png" />
<br>
<br>

- En la tercera url descargamos un archivo csv con el los horarios por jornada de los partidos, que también convertimos a DataFrame. Entonces aplicamos alguns métodos básicos de la librería Pandas para renombrar las columnas, eliminar duplicados, valores nulos y columnas sin interés, además de unificar los nombres como para el DF anterior. Finalmente, generamos una columna extra 'Prime Time' (YES/NO) para cada partido, la cual añadimos al DF principal para obtener nuestra tabla final.


<br>
<img src="https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/df3.png" />

<a name="qb"/>

## QB Ratings

Una vez finalizada la transformación de los datos, obtuvimos un único DataFrame que exportar a MySQL. Para ello, creamos la base de datos "nflpenalties" y diseñamos su EERD correspondiente. Tras un hacer Forward Engineer creamos la tabla "penalties_week_1_2022", en un principio vacía. Después, mediante SQLAlchemy realizamos la exportación y rellenamos dicha tabla, finalizando así el proceso de carga. 

<br>

![penalties_week_1_2022](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/penalties_week_1_2022.png)

</details>


<a name="special"/>

## Special Teams

La hipótesis principal que me ha llevado a elegir esta temática para mi ETL Project es la siguiente:

<p><strong> Los árbitros tienden a buscar un final igualado, y por tanto benefician al equipo que va por debajo en el marcador.</strong>

Para intentar comprobar esta teoría, he tirado las siguiente querys en MySQL:

<details>
<summary>Relación Beneficiary-Winner</summary>
<br>

 ```
SELECT Beneficiary, Winner, COUNT(*) as Total
FROM penalties_week_1_2022
	
WHERE `Time left` < 300 AND Quarter = 4
GROUP BY Beneficiary, Winner
ORDER BY Total DESC
;
 ```

![beneficiary_winner](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/beneficiary_winner.png)
		       
En esta tabla podemos apreciar que, en la mayoría de casos, el beneficiario de una falta en los últimos cinco minutos de partido NO suele ser el ganador. Vamos un paso más allá en esa dirección.
	
</details>

<details>
<summary>Beneficiary-Winner vs Beneficiary-Loser 4Q</summary>
<br>

```
SELECT
  SUM(CASE WHEN Beneficiary = Winner THEN 1 ELSE 0 END) as Total_Coincidence,
  SUM(CASE WHEN Beneficiary != Winner THEN 1 ELSE 0 END) as Total_No_Coincidence
FROM penalties_week_1_2022

WHERE `Time left` < 300 AND Quarter = 4
;
 ```

![coinci_4](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/coinci_4.png)
		       
En esta segunda tabla podemos comprobar que, aunque la muestra es pequeña, la hipótesis se cumple. En los últimos cinco minutos del último cuarto, las decisiones arbitrales son mayoritariamente favorables al equipo que termina perdiendo.

</details>

<details>
<summary>Beneficiary-Winner vs Beneficiary-Loser 1Q, 2Q y 3Q</summary>
<br>

```
SELECT
  SUM(CASE WHEN Beneficiary = Winner THEN 1 ELSE 0 END) as Total_Coincidence,
  SUM(CASE WHEN Beneficiary != Winner THEN 1 ELSE 0 END) as Total_No_Coincidence
FROM penalties_week_1_2022

WHERE Quarter != 4
;
 ```

![coinci_no4](https://github.com/CharlyKill7/NFL-Penalties_ETL/blob/main/images/coinci_no4.png)
	
En esta tabla final usamos la misma query que en la anterior, con la salvedad de hacerlo para los cuartos 1, 2 y 3. Como podemos ver, durante los primeros tres cuartos la mayoría de las decisiones arbitrales benefician al a la postre ganador del encuentro. Esto no se cumple en el cuarto cuarto, como vimos en la query anterior.
	

</details>
	
<p><strong> Como conclusión, y aunque la muestra es todavía pequeña, parece que la hipótesis se confirma: los equipos perdedores son más beneficiados por las decisiones arbitrales en los minutos finales de partido.</strong>
	

<br>
	
<a name="defense"/>

## Defense
	
	
<a name="conclusion"/>

## SConclusión
