# Objetivos

1. Identificar las principales barreras de acceso al transporte público en los asentamientos humanos de desarrollo incompleto de Cali.

2. Describir las condiciones de accesibilidad de los habitantes de los asentamientos humanos de desarrollo incompleto y su relación con la ubicación de los principales centros laborales en la ciudad.
  
3. Explorar patrones espaciales que influyen en el acceso a las oportunidades laborales de los habitantes de los asentamientos humanos de desarrollo incompleto de Cali según sus niveles de accesibilidad al transporte público y su proximidad a los centros de empleo. 


# Bitácora del proyecto

## Proyecto
DISPARIDADES ESPACIALES EN LA ACCESIBILIDAD A LAS OPORTUNIDADES LABORALES: UN ANÁLISIS DE LAS BARRERAS DE ACCESO AL TRANSPORTE PÚBLICO EN LOS ASENTAMIENTOS HUMANOS DE DESARROLLO INCOMPLETO DE CALI

## Objetivo de la bitácora
Registrar de manera cronológica los avances metodológicos, decisiones tomadas, insumos utilizados, dificultades encontradas y actividades pendientes del proyecto, con el propósito de garantizar trazabilidad y reproducibilidad del proceso de investigación.

---

# Estado actual del proyecto

Actualmente se ha avanzado en la delimitación preliminar de clusters de AHDI, evaluación de su representatividad poblacional y habitacional, cálculo de pesos poblacionales, generación de centroides ponderados, acotamiento espacial de los clusters y validación de los puntos de origen que serán utilizados posteriormente en las simulaciones de accesibilidad.

El trabajo actual se concentra en consolidar las unidades espaciales definitivas y los puntos de origen que representarán a los asentamientos en los análisis posteriores de origen-destino.

---

# 1. Preparación y depuración de la información espacial

## Qué se hizo

Se trabajó inicialmente con la capa AHDI_Union, correspondiente a la integración espacial de los asentamientos humanos de desarrollo incompleto con información territorial y sociodemográfica.

Durante la revisión de los registros se identificaron duplicados, por lo que se realizó una depuración de la base.

Universo inicial:
191 registros de AHDI.

Universo depurado:
188 AHDI únicos.

La capa depurada constituye actualmente el universo de referencia del estudio.

## Resultado

- AHDI totales depurados: 188
- Población total: 164.463 habitantes
- Viviendas totales: 61.665

## Estado
Finalizado.

---

# 2. Construcción preliminar de clusters

## Qué se hizo

La capa de AHDI fue transformada a puntos mediante Feature To Point para permitir análisis de distribución espacial.

Posteriormente se utilizó un criterio basado en la concentración de viviendas para identificar asentamientos con mayor densidad residencial relativa.

Sobre los asentamientos seleccionados se aplicaron elipses de desviación estándar, agrupadas territorialmente, con el propósito de identificar patrones de concentración y orientación espacial.

Las elipses se relacionaron posteriormente con la capa de barrios de Cali. Los barrios asociados fueron seleccionados, exportados y disueltos espacialmente.

Mediante Multipart To Singlepart se separaron las agrupaciones espacialmente independientes.

## Resultado

Se obtuvieron inicialmente 8 clusters territoriales.

## Estado
Finalizado como construcción preliminar.

## Observación

Los clusters fueron posteriormente sometidos a procesos adicionales de validación y ajuste, por lo cual los 8 clusters iniciales no necesariamente equivalen a las unidades finales utilizadas como puntos de origen.

---

# 3. Evaluación de representatividad

## Qué se hizo

Se comparó el universo depurado de 188 AHDI con los asentamientos incorporados en el proceso de clusterización.

AHDI incluidos:
106.

AHDI excluidos:
82.

Se calcularon población y viviendas para cada grupo mediante Summary Statistics.

## Resultados

| Grupo | N° AHDI | Población | % población | Viviendas | % viviendas |
|---|---:|---:|---:|---:|---:|
| Total | 188 | 164.463 | 100 % | 61.665 | 100 % |
| Incluidos | 106 | 125.601 | 76,37 % | 47.451 | 76,95 % |
| Excluidos | 82 | 38.862 | 23,63 % | 14.214 | 23,05 % |

Aunque los 106 AHDI representan el 56,38 % del número total de asentamientos, concentran aproximadamente tres cuartas partes de la población y de las viviendas del universo analizado.

## Estado
Finalizado.

---

# 4. Clasificación de asentamientos incluidos y excluidos

## Qué se hizo

Se generó una capa de control denominada AHDI_188_estado_cluster.

Se creó el campo ESTADO_CL para diferenciar los AHDI incluidos y excluidos.

La clasificación definitiva se realizó mediante identificadores únicos de los AHDI, evitando depender exclusivamente de relaciones espaciales que podían seleccionar polígonos vecinos por contacto o pequeños traslapes.

## Resultado

- Incluidos: 106 AHDI
- Excluidos: 82 AHDI

## Estado
Finalizado.

---

# 5. Pesos poblacionales

## Qué se hizo

Para los 106 AHDI incorporados en los clusters se calcularon tres indicadores de participación poblacional:

PESO_CL:
peso del AHDI respecto a la población total de su cluster.

PESO_106:
peso del AHDI respecto a los 125.601 habitantes correspondientes a los 106 AHDI seleccionados.

PESO_188:
peso del AHDI respecto a los 164.463 habitantes del universo depurado.

Se verificaron los resultados mediante Summary Statistics.

## Control de calidad

La suma de PESO_CL dentro de cada cluster fue igual a 100 %.

La suma global de PESO_106 fue igual a 100 %.

La suma global de PESO_188 para los AHDI incluidos corresponde aproximadamente al 76,37 % de la población total del universo.

## Estado
Finalizado.

---

# 6. Identificación de AHDI representativos por cluster

## Qué se hizo

Los asentamientos fueron ordenados mediante:

1. ID_CLUSTER ascendente.
2. PESO_CL descendente.

Posteriormente se creó el campo RANK_CL para establecer la posición poblacional de cada AHDI dentro de su cluster.

Esto permitió identificar los AHDI con mayor participación poblacional dentro de cada agrupación.

## Resultado

Se generaron capas con los principales AHDI de cada cluster para facilitar la interpretación espacial y poblacional.

## Estado
Finalizado.

---

# 7. Centroides ponderados por población

## Qué se hizo

Los polígonos de AHDI fueron convertidos a puntos internos mediante Feature To Point.

Posteriormente se utilizó Mean Center con:

Campo de peso:
población del AHDI.

Campo de caso:
ID_CLUSTER.

El procedimiento permitió obtener un centroide ponderado por población para cada cluster.

## Objetivo

Evitar que la ubicación representativa del cluster dependiera únicamente de su geometría y permitir que los asentamientos con mayor población ejercieran mayor influencia sobre la localización del centro.

## Resultado

Se obtuvo una primera capa de centroides ponderados para los clusters.

## Problema identificado

Durante la validación espacial se observó que algunos centroides quedaban alejados de parte de los asentamientos que representaban e incluso, en determinados casos, podían localizarse en áreas poco adecuadas para ser utilizadas como punto de origen.

Esto generó un nuevo problema metodológico:

¿es válido asumir que toda la población de un cluster inicia su desplazamiento desde un único centroide cuando algunos asentamientos están considerablemente alejados de dicho punto?

## Estado
Finalizado como alternativa inicial, pero posteriormente ajustado.

---

# 8. Acotamiento espacial de los clusters

## Qué se hizo

Se evaluó la continuidad espacial de los clusters mediante Dissolve sin entidades multiparte.

Posteriormente se aplicó:

Buffer +50 m
seguido de
Buffer -50 m

disolviendo las entidades por ID_CLUSTER.

El procedimiento buscó conectar asentamientos próximos sin generar delimitaciones excesivamente amplias.

Posteriormente se utilizaron Multipart To Singlepart y Summary Statistics para evaluar fragmentación y cambios de área.

## Resultados generales

Los clusters 1, 2, 3, 5 y 8 presentaron cambios de área inferiores al 10 %.

Los clusters 6 y 7 presentaron incrementos cercanos al 14-15 %.

El cluster 4 presentó el mayor cambio, aproximadamente 28,51 %.

Los clusters 4, 6 y 7 fueron revisados visualmente.

También se revisaron zonas con presencia de cuerpos de agua, especialmente el entorno de la Laguna del Pondaje, evitando incorporar artificialmente áreas sin presencia efectiva de AHDI.

## Estado
Finalizado.

---

# 9. Alternativas evaluadas para representar los puntos de origen

Durante el proceso se identificaron diferentes alternativas para representar espacialmente los clusters.

## Alternativa 1. Centroide geométrico

Representa únicamente la geometría del cluster.

Ventaja:
procedimiento simple.

Limitación:
puede quedar alejado de las zonas con mayor población o incluso en espacios que no corresponden a asentamientos.

## Alternativa 2. Centroide ponderado por población

Considera la distribución poblacional de los AHDI.

Ventaja:
los asentamientos con mayor población tienen mayor influencia en la ubicación del punto.

Limitación:
cuando el cluster presenta alta dispersión espacial, un único centroide puede seguir estando demasiado alejado de algunos asentamientos.

## Alternativa 3. Punto de origen ajustado mediante criterio de proximidad

Se estableció una distancia euclidiana de referencia de 750 m.

Se generaron buffers de 750 m alrededor de los asentamientos y se identificaron zonas de superposición.

El punto de origen ajustado fue ubicado dentro de una zona que permitiera minimizar las distancias respecto a los asentamientos representados.

## Alternativa 4. Subdivisión de clusters espacialmente dispersos

Cuando un único punto no permitió cumplir el criterio de proximidad, el cluster fue subdividido en agrupaciones territoriales menores.

Esta alternativa fue necesaria para los clusters 4 y 6.

---

# 10. Validación de puntos de origen

## Qué se hizo

Se utilizó Generate Near Table para calcular la distancia entre cada punto de origen ajustado y todos los AHDI que representaba.

Método:
Planar.

Unidad:
metros.

Criterio:
distancia máxima deseable de 750 m.

## Primera validación

Los clusters 2, 5, 7 y 8 pudieron representarse mediante un único punto ajustado.

Los clusters 4 y 6 presentaron distancias superiores al criterio establecido.

## Decisión metodológica

Cluster 4:
se subdividió en 4A, 4B y 4C.

Cluster 6:
se subdividió en 6A, 6B y 6C.

Se generaron nuevos puntos de origen y se repitió la validación.

## Validación final

| Unidad | Distancia máxima aproximada |
|---|---:|
| 2 | 664,82 m |
| 4A | 271,99 m |
| 4B | 658,27 m |
| 4C | 421,75 m |
| 5 | 443,62 m |
| 6A | 577,72 m |
| 6B | 590,59 m |
| 6C | 361,54 m |
| 7 | 582,59 m |
| 8 | 200,81 m |

Todas las unidades evaluadas quedaron por debajo de 750 m.

## Estado
Estado: cerrado mediante decisión metodológica.
Inicialmente se identificó como dificultad la definición de puntos de origen representativos para clusters espacialmente extensos. Se evaluaron centroides ponderados por población, puntos ajustados mediante un criterio máximo de 750 m y subdivisiones internas de los clusters 4 y 6. Aunque estas estrategias permitieron corregir las distancias entre los puntos y los AHDI asociados, en reunión posterior con la dirección del proyecto se decidió no utilizar los clusters de AHDI como unidad geográfica definitiva. Se estableció el asentamiento como unidad principal de análisis, reservando la escala de manzana para indicadores de conectividad interna y utilizando el asentamiento como origen para los análisis de accesibilidad hacia los centros de empleo.

Decisión vigente:
- Unidad geográfica principal: AHDI/asentamiento.
- Escala desagregada para conectividad: manzana.
- Accesibilidad: asentamiento → centros/subcentros de empleo.
- Clusters de AHDI: alternativa metodológica evaluada, no adoptada como unidad final.

---

# 11. Estado metodológico actual

Actualmente se cuenta con:

- universo depurado de AHDI;
- clasificación de incluidos y excluidos;
- evaluación de representatividad;
- clusters preliminares;
- pesos poblacionales;
- centroides ponderados;
- delimitaciones acotadas;
- puntos de origen ajustados;
- subclusters para las agrupaciones espacialmente dispersas;
- tablas de validación de distancias.

La siguiente fase consiste en consolidar definitivamente las unidades de origen para incorporarlas a las simulaciones de accesibilidad origen-destino.

---

# 12. Cuellos de botella actuales

## Justificación de criterios

Se requiere fortalecer la sustentación bibliográfica o técnica de algunos parámetros utilizados, especialmente:

- criterio de representatividad;
- buffer de 50 m utilizado para el acotamiento;
- criterio de proximidad de 750 m para los puntos de origen.

## Consolidación de unidades finales

Debe definirse y documentarse de manera definitiva qué unidades serán utilizadas en las simulaciones posteriores y cuál será su nomenclatura final.

## Reproducibilidad

Existen varias capas intermedias generadas durante los ensayos metodológicos. Se requiere identificar cuáles corresponden a productos definitivos y cuáles deben conservarse únicamente como evidencia del proceso.

## Análisis origen-destino

Está pendiente integrar los puntos de origen definitivos con la información de destinos y posteriormente desarrollar los indicadores de accesibilidad.

## Literatura

Debe consolidarse en un archivo independiente la revisión bibliográfica realizada, indicando para cada documento:

Referencia.
Objetivo.
Metodología.
Indicadores utilizados.
Escala de análisis.
Datos utilizados.
Aporte específico al proyecto.
Limitaciones.

## 08 de octubre de 2026 – Preparación de la prueba piloto de accesibilidad

### Actividades realizadas

Durante la jornada se avanzó en la preparación de la prueba piloto para la simulación de viajes entre los AHDI y los centros de empleo.

En primer lugar, se definió una muestra piloto de 19 AHDI a partir de la capa maestra de 188 asentamientos. La selección se realizó buscando representatividad espacial y cobertura territorial, evitando concentrar todos los casos en una misma zona de la ciudad. Para algunas comunas con distribución espacial dispersa se seleccionó más de un asentamiento representativo.

Los 19 AHDI seleccionados fueron exportados como una capa independiente y posteriormente convertidos a puntos mediante la herramienta `Feature To Point`, garantizando que cada punto se ubicara dentro del polígono del asentamiento.

A cada punto de origen se le calcularon coordenadas geográficas en el sistema WGS 1984 (EPSG:4326), obteniendo los campos de latitud y longitud necesarios para su posterior utilización en las simulaciones mediante Google Maps.

### Centros de empleo

A partir de la revisión de la tesis de Nicolás se identificaron los seis centros de empleo utilizados como destinos en el análisis de accesibilidad:

- Versalles
- Industrial
- San Fernando Nuevo
- Departamental
- El Gran Limonar
- Ciudad Campestre

Debido a que no se cuenta actualmente con el archivo original `CENTROS DE EMPLEOS.xlsx` (no sabemos si existe) utilizado en dicha investigación, se construyó de manera provisional una capa con estos seis centros de empleo.

Los barrios correspondientes fueron seleccionados en ArcGIS Pro y convertidos a puntos representativos mediante `Feature To Point`. Posteriormente se calcularon sus coordenadas geográficas en WGS 1984.

Esta capa se considera provisional y deberá validarse con el director, principalmente para confirmar si los puntos utilizados originalmente corresponden a centroides de los barrios, puntos interiores o a una localización definida mediante otro criterio relacionado con la concentración de empleo.

### Preparación de los archivos para la simulación

Se exportaron a formato `.xlsx` las siguientes tablas:

- `ORIGENES_AHDI_PILOTO_19.xlsx`
- `CENTROS_EMPLEO_6_PROVISIONALES.xlsx`

Los archivos fueron cargados en Google Drive y conectados correctamente con Google Colab.

En Colab se verificó la lectura de ambas bases:

- 19 registros correspondientes a los AHDI piloto.
- 6 registros correspondientes a los centros de empleo.

Posteriormente se construyeron tablas simplificadas para la simulación con la siguiente estructura:

**Orígenes:**
- ID_AHDI
- AHDI
- Latitud
- Longitud

**Destinos:**
- Id_Centro
- Centro_Empleo
- Latitud
- Longitud

Se verificó que los 19 AHDI y los 6 centros contaran con coordenadas válidas.

### Construcción de la matriz origen-destino

A partir de los 19 AHDI piloto y los 6 centros de empleo se generaron todas las combinaciones posibles de origen y destino:

19 AHDI × 6 centros de empleo = 114 pares origen-destino.

Se creó una matriz base con los campos:

- ID_AHDI
- Nombre del AHDI
- Latitud de origen
- Longitud de origen
- ID del centro de empleo
- Nombre del centro de empleo
- Latitud de destino
- Longitud de destino

Esta matriz queda preparada para incorporar posteriormente los tiempos de desplazamiento obtenidos mediante la API de Google Maps.

### Revisión del código

Se revisó el código suministrado por el director (`Código_28-09-2026`) y se identificó que fue diseñado originalmente para trabajar con barrios como origen y centros de empleo como destino.

Para el presente estudio será necesario adaptarlo para utilizar AHDI como unidades de origen.

También se identificó que el código actualizado trabaja con diferentes franjas horarias y contempla simulaciones en transporte público y vehículo particular.

### Dificultades y aspectos pendientes

El principal aspecto pendiente para ejecutar la simulación corresponde al acceso a la API de Google Maps.

Actualmente no se cuenta con una API Key propia ni se conoce si existe un proyecto de Google Maps Platform disponible dentro del grupo de investigación.

También está pendiente confirmar si existe el archivo original `CENTROS DE EMPLEOS.xlsx` utilizado en la tesis de Nicolás o si los seis centros deben ser reconstruidos a partir de la información espacial y laboral disponible.

### Preguntas para la próxima reunión

1. Confirmar si existe el archivo original `CENTROS DE EMPLEOS.xlsx` utilizado en la tesis de Nicolás.
2. Confirmar el criterio espacial utilizado para definir el punto representativo de cada centro de empleo.
3. Definir si se utilizará una API Key del grupo de investigación o si será necesario crear un proyecto propio en Google Maps Platform.
4. Confirmar si la prueba piloto debe ejecutarse utilizando el código actualizado con diferentes franjas horarias o si debe adaptarse el código empleado originalmente por Nicolás.
5. Confirmar qué variables de tiempo deben conservarse para el posterior cálculo del indicador de accesibilidad.

### Estado actual

La prueba piloto se encuentra preparada hasta la construcción de la matriz origen-destino.

Actualmente se dispone de:

- 19 AHDI piloto espacialmente distribuidos.
- 19 puntos de origen con coordenadas geográficas.
- 6 centros de empleo provisionales con coordenadas geográficas.
- 114 pares origen-destino preparados para simulación.
- Archivos de entrada organizados en formato Excel.
- Código cargado y entorno de trabajo preparado en Google Colab.

El siguiente paso será validar los centros de empleo y el acceso a la API de Google Maps para realizar las primeras simulaciones de viaje.
