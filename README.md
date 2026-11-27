# Clasificador de animales
## Aplicación de escritorio para identificar animales
### Descripción
En el presente repositorio se muestra el código fuente con el que fue creado la aplicación de escritorio, ésta fue creada con JavaFX.

Con el fin de identificar a los animales correctamente se entrenó un modelo de CNN con pytorch y un aproximado de 20k de imágenes, dichas imágenes se encuentran en: [Zenodo](https://doi.org/10.5281/zenodo.19926200),  adémas, los animales que se pueden identificar con dicha aplicación son los siguientes:

Orden Strigiformes

*  Aegolius acadicus
*  Asio flammeus
*  Asio otus
*  Asio stygius
*  Athene cunicularia
*  Bubo virginianus
*  Glaucidium gnoma
*  Megascops kennicottii
*  Megascops trichopsis
*  Micrathene whitneyi
*  Psiloscops flammeolus
*  Strix virgata 
*  Strix occidentalis
*  Strix sartorii
*  Tyto furcata
 
Género Crotalus

*  Crotalus aquilus
*  Crotalus atrox
*  Crotalus basiliscus
*  Crotalus lepidus
*  Crotalus molossus
*  Crotalus polystictus
*  Crotalus pricei
*  Crotalus scutulatus
*  Crotalus willardi

Todos estos animales habitan en el estado de Zacatecas, México.

Por otro lado, el modelo puro entrenado se puede descargar en: [Modelo ResNet50](https://drive.google.com/drive/folders/1EVbwNs3NiT5-lU0dxr1VLg_0NcgxXaHS?usp=drive_link), para visualizar los resultados obtenidos por el modelo [vaya a la sección "Resultados del modelo de IA"](#resultados-del-modelo-de-ia)

---

### Diseño de la aplicación

La arquitectura de la aplicación está basada en capas, contando con 4 capas los cuales son las siguientes:

```mermaid
flowchart TD
    A[Interfaz de usuario] --> B[Dominio/Negocio]
    B -->C[Almacenamiento]
    C --> E[Comunicación externa]
```

 1. La capa de UI es donde se encuentran los diseños para facilitar el trabajo de los usuarios, siendo intuitivo su uso.
 2. La capa de Dominio/Negocio contiene el backend necesario con el cual funciona la aplicación
 3. La capa de Almacenamiento contiene pequeños archivos y el modelo JSON con el cual se clasifica las imágenes.
 4. La capa de Comunicación externa es la conexión entre las imagenes y observaciones que haga el usuario con la base de datos externa de FireBase de Google.

---
### Diseño de la Base de datos
La base de datos en esta aplicación consta de 2 tablas relacionadas de una a muchas, la primera tabla contiene 3 elementos que sirven para almacenar e identificar los datos del usuario.

| ***Usuarios*** |
| :------------- |
| ID             |
| Email          |
| Contraseña     |

La segunda tabla contiene 5 elementos para almacenar los datos al momento de realizar la clasificación de alguna imagen por cada usuario.
| ***Resultado*** |
| :-------------- |
| ID              |
| Especie         |
| Confianza       |
| Imagen          |
| Id_usuario      |

Cabe destacar que el resultado solo se almacenará cuando su porcentaje de confianza es mayor a 75%, del mismo modo, para acceder a esta base de datos se realizará una petición a una API REST perteneciente a la RealTime Database de Google. 

---

<details>
<summary><b>Click para ver el diseño de la API Rest</b></summary>
 <div align="center">
 
 </div>
</details>

---

<details>
<summary><b>Click para ver los Mockups de la aplicación</b></summary>
 
 <div align="center">
   <img src="imagenes/Login.png" alt="Pantalla de login para los usuarios" height="1000">
 </div>
 
En esta pantalla el usuario podrá ingresar un email y contraseña para entrar a la aplicación. Si decide usar la opción "entrar con Google" internamente se revisará si existe alguna cuenta almacenada en la base de datos y se conectará si es así, en caso de que no exista alguna cuenta, internamente se registrará el email asociado a Gmail de Google.

 <div align ="center">
  <img src="imagenes/registro.png" alt="Pantalla para registrar nuevos usuarios" height="1000">
 </div>
 
La pantalla anterior muestra los datos que se necesitan para el registro de nuevos usuarios. En este caso solo se necesitan el email y una contraseña.

 <div align ="center">
  <img src="imagenes/pan_clas.PNG" alt="Pantalla para realizar la clasificación de una imagen" height="1000">
 </div>
 
Aquí el usuario podrá subir alguna imagen desde el almacenamiento del PC con formato PNG, dicha imagen será enviada mediante una petición al servidor donde se aloja la API, y ésta revolverá los resultados correspondientes a esa imagen.

 <div align = "center">
  <img src="imagenes/clas_cargar.PNG" alt="Pantalla de carga mientras se realiza la clasificación" height="1000">
 </div>
 
Al momento de enviar la imagen al servidor y mientras espera una respuesta, el sistema mostrará un carga indefinida por algunos segundos.

 <div align= "center">
  <img src="imagenes/clas_error.PNG" alt="Pantalla de error al momento de no identificar a la especie" height="1000">
 </div>

En caso de que el porcentaje de confianza sea menor a 75% u ocurra alguna excepción, se mostrará un dialog especificando el tipo de error que ha ocurrido.

 <div align = "center">
  <img src="imagenes/especies.PNG" alt="Pantalla que menciona las especies que clasifica" height="1000">
 </div>

Un apartado dentro de la aplicación que detalla de manera concisa, con una imagen y datos específicos, cada especie que el modelo es capaz de clasificar.

 <div align = "center">
  <img src="imagenes/historial.PNG" alt="Pantalla que muestra las clasificaciones que ha hecho el usuario" height="1000">
 </div>

El usuario podrá visualizar un listado de todas las clasificaciones que ha realizado (con un límite de 20 por página) en la aplicación, en caso de que sea la primera vez que usa la aplicación y no ha hecho ninguna clasificación, el usuario no podrá entrar, simplemente mostrará un dialog con la explicación de porque no puede entrar.

 <div align = "center">
  <img src="imagenes/historial_especie.PNG" alt="Pantalla que muestra la ficha que el usuario eligió" height="1000">
 </div>

Cuando se está en el apartado del historial, el usuario al clickear una entrada el programa mostrará una pantalla con la información resumida y los resultados obtenida durante la clasificación.
 
</details>

***
<details>
<summary><b>Click para ver la aplicación en funcionamiento</b></summary>
 <div align="center">
 
 </div>
</details>

***

### Resultados del modelo de IA
<div align="center">
 
Hiperparametros utilizados en tres modelos.

| Modelo | Optimizer | Epochs | Batch size | num workers | Learning rate | 
|--------|-----------|--------|------------|-------------|---------------|
| ResNet-50         | Adam | 20 | 32  | 2 | 0.001 |
| Efficient Net-B0  | Adam | 20 | 512 | 2 | 0.005 |
| Squezze Net-1.0   | Adam | 15 | 32  | 2 | 0.001 |

***

Resultados obtenidos entre el dataset de entrenamiento y el dataset de validación.

| Modelo | Pérdida (Train) | Exactitud (Train) | Pérdida (Val) | Exactitud (Val) | Tiempo promedio por época (Min). |
|--------|-----------------|-------------------|---------------|-----------------|----------------------------------|
| ResNet-50        | 0.3378 | 88.73% | 0.3412 | 88.88% | 5.45 |
| Efficient Net-B0 | 0.3893 | 87.71% | 0.38   | 87.04% | 5.06 |
| Squeeze Net-1.0  | 0.4065 | 86.61% | 0.429  | 86.71% | 5.29 |

***

Enseguida se muestra los resultados y la evolución durante la aplicación de una validación cruzada con diez folds.
<div align="center">

 <img src="imagenes/_Cross-Val segunda iteración.png" alt="Validación cruzada con 10 folds" height="1000">
</div>

***

Finalmente se calculó las Macro´s necesarias con el que se definió que el mejor modelo entre los tres fue ResNet-50.

| Métrica | ResNet-50 | Efficient Net-B0 | SqueezeNet-1.0 |
|---------|-----------|------------------|----------------|
| Macro puntuación F1 | 91.15% | 87.35% | 85.92% |
| Macro precisión     | 91.19% | 87.39% | 85.98% |
| Macro Sensibilidad  | 91.13% | 87.34% | 85.88% |

</div>





