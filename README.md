# TRABAJO DE CONSULTA CONSULTA_2
### INTEGRANTE: JOHN PAUL SARITAMA SARITAMA
## 1. ¿Qué es JDBC y cuáles son sus componentes?

es una API que proporciona una interfaz estándar para que las aplicaciones en Java puedan conectarse y operar sobre bases de datos. Gracias a JDBC, los desarrolladores pueden interactuar con distintos sistemas de bases de datos de manera uniforme. Sus principales componentes son:

Driver JDBC: Permite la comunicación con bases de datos específicas.

Conexión: Representa la sesión con la base de datos.

Statement: Permite ejecutar consultas SQL.

ResultSet: Contiene los resultados de las consultas realizadas.

## 2. Documente 2 librerías de Scala que permitan conectarse a una base de datos relacional. En una tabla resuma sus diferencias.

Segun lo que encontre dos librerías son populares en Scala para conectarse a bases de datos relacionales son Slick y Doobie. Ahora muestro la tabla de resumen de comparación de sus características principales:

| Característica        | Slick                                         | Doobie                                      |
|----------------------|-----------------------------------------------|---------------------------------------------|
| Tipo de Mapeo        | Mapeo Funcional Relacional (FRM)              | Mapeo Funcional Reactivo (FRP)              |
| Seguridad en tiempo de compilación | Sí                                         | Sí                                          |
| Composabilidad       | Alta (permite componer consultas como colecciones de Scala) | Media (se enfoca en un estilo más reactivo) |
| API Asincrónica      | Sí (usa "Future", compatible con Reactive Streams) | Sí (usa IO de Cats Effect, compatible con Reactive Streams) |
| Soporte de SQL       | Sí                                            | Sí                                          |


## 3.Documentar cómo establecer una conexión a una base de datos relacional (mysql). Siga los siguientes pasos:
### 3.1 Genere una base de datos en mysql
### 3.2 Genere una tabla con datos de prueba
### 3.3 Desde Scala establezca la conexión a la base datos
### 3.4 Desde Scala realice la consulta de todos los datos de la tabla de prueba. 
### 3.5 CODIGO COMPLETO
## 4. Referencias:
