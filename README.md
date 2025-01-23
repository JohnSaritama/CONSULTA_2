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

Primero, creé una base de datos en MySQL Workbench:
```sql
CREATE DATABASE peliculasDB;
USE peliculasDB;
```
### 3.2 Genere una tabla con datos de prueba

Luego, generé los datos de la tabla:
```sql
CREATE TABLE peliculas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL,
    director VARCHAR(100) NOT NULL,
    anio INT NOT NULL,
    genero VARCHAR(50)
);

INSERT INTO peliculas (titulo, director, anio, genero) VALUES
('El Padrino', 'Francis Ford Coppola', 1972, 'Drama'),
('Titanic', 'James Cameron', 1997, 'Romance'),
('El Caballero de la Noche', 'Christopher Nolan', 2008, 'Acción'),
('Interestelar', 'Christopher Nolan', 2014, 'Ciencia Ficción'),
('Parasite', 'Bong Joon-ho', 2019, 'Thriller');
```

![image](https://github.com/user-attachments/assets/9e8c2a7e-7688-49ed-babf-0e3fc9746eda)


### 3.3 Desde Scala establezca la conexión a la base datos
Agrego la dependencia en Scala que permite interactuar con MySQL:
```Scala
libraryDependencies += "mysql" % "mysql-connector-java" % "8.0.33"
```
Luego agrego la ruta de acceso:
```Scala
val url = "jdbc:mysql://localhost:3306/peliculasDB?useSSL=false"
val usuario = "root"
val contraseña = "tu_contraseña"
```
Los cuales representa lo siguiente:
url: Ruta de acceso a la base de datos, que incluye el nombre de la base (peliculasDB) y useSSL=false para deshabilitar el uso de SSL.

usuario y contraseña: Credenciales para acceder al servidor MySQL.

### 3.4 Desde Scala realice la consulta de todos los datos de la tabla de prueba. 

Se utiliza Class.forName("com.mysql.cj.jdbc.Driver") para cargar el driver necesario.
```Scala
Class.forName("com.mysql.cj.jdbc.Driver")
```

Se utiliza DriverManager.getConnection con los parámetros definidos.
```Scala
val conexion = DriverManager.getConnection(url, usuario, contraseña)
println("Conexión establecida con éxito!")
```


#### -Ejecutar una consulta

Se crea un Statement con conexion.createStatement().

La consulta SELECT * FROM peliculas se ejecuta y el resultado se almacena en resultSet.
```Scala
val declaracion = conexion.createStatement()
val resultSet = declaracion.executeQuery("SELECT * FROM peliculas")
```


 #### -Procesamiento de resultados

Se recorren las filas de resultSet para construir objetos Pelicula.
```Scala
var peliculas = List[Pelicula]()

while (resultSet.next()) {
  val pelicula = Pelicula(
    resultSet.getInt("id"),
    resultSet.getString("titulo"),
    resultSet.getString("director"),
    resultSet.getInt("anio"),
    resultSet.getString("genero")
  )
  peliculas = pelicula :: peliculas
}
```

#### -Liberar recursos

Se cierran los recursos en el bloque finally para evitar fugas de memoria.
```Scala
if (resultSet != null) { resultSet.close() }
if (declaracion != null) { declaracion.close() }
if (conexion != null) { conexion.close() }
```

#### -Resultados en la terminal:
Se imprime con:
```Scala
peliculas.foreach(p => println(s"Título: ${p.titulo}, Director: ${p.director}, Año: ${p.anio}, Género: ${p.genero}"))
```

RESULTADO:

![image](https://github.com/user-attachments/assets/184aa5a7-e80c-4a48-bab6-697d7eab782e)

### 3.5 CODIGO COMPLETO
```Scala
import java.sql.{Connection, DriverManager, ResultSet}

case class Pelicula(id: Int, titulo: String, director: String, anio: Int, genero: String)

object ConexionMySQLPeliculas extends App {
  val url = "jdbc:mysql://localhost:3306/peliculasDB?useSSL=false"
  val usuario = "root"
  val contrasena = "JOHN"

  var conexion: Connection = null
  var declaracion: java.sql.Statement = null
  var resultSet: ResultSet = null

  try {
    // Registrar el driver JDBC
    Class.forName("com.mysql.cj.jdbc.Driver")

    // Establecer la conexión
    conexion = DriverManager.getConnection(url, usuario, contrasena)
    println("Conexión establecida con éxito!")

    declaracion = conexion.createStatement()
    resultSet = declaracion.executeQuery("SELECT * FROM peliculas")
    var peliculas = List[Pelicula]()

    while (resultSet.next()) {
      val pelicula = Pelicula(
        resultSet.getInt("id"),
        resultSet.getString("titulo"),
        resultSet.getString("director"),
        resultSet.getInt("anio"),
        resultSet.getString("genero")
      )
      peliculas = pelicula :: peliculas
    }

    // Imprimir los datos de las peliculas
    peliculas.foreach(p => println(s"Título: ${p.titulo}, Director: ${p.director}, Año: ${p.anio}, Género: ${p.genero}"))
  } catch {
    case e: Exception => e.printStackTrace()
  } finally {
    if (resultSet != null) { resultSet.close() }
    if (declaracion != null) { declaracion.close() }
    if (conexion != null) { conexion.close() }
  }
}
```
## 4. Referencias:

"Slick 3.3.3 Documentation." Lightbend Inc. Disponible en: http://slick.lightbend.com/doc/3.3.3/

"Doobie Documentation." Tpolecat. Disponible en: https://tpolecat.github.io/doobie/


