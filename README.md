# Pep 1 Distribuidos

## Descripción del sistema implementado

El sistema desarrollado fue diseñado bajo una arquitectura Cliente - Servidor. En el lado del cliente se ejecuta un Front-End realizado con VueJs, por otra parte, en el lado del servidor se tiene un Back-End implementado en Spring realizando el trabajo de una API Rest con una base de datos PostgresSQL.

El sistema permite ingresar datos en un formulario para simular la obtención de un permiso como lo es en el sitio de “Comisaría Virtual”, además es posible verificar la fecha del permiso generado para obtener su validación.

## Descripción del Servidor utilizado para las pruebas.

Con el propósito de reconocer el funcionamiento del sistema bajo alta demanda de peticiones se han tenido que realizar pruebas de estrés sobre el mismo, por lo cual es importante conocer el sistema con el cual se ha trabajado y tenido de referencia.

Servidor: 1 CPU, 1 GB RAM, 25 GB SSD.

## Principales características de un sistema distribuido

### Disponibilidad

El sistema debe tener disponible los recursos de manera permanente. En la mayoría de los casos, se utiliza la replicación para asegurar que los datos siempre estarán disponibles para el usuario.

### Transparencia de la distribución

La definición de transparencia en un sistema distribuido es la ocultación al usuario que los componentes de este sistema distribuido están separados. Por lo tanto, el usuario percibirá que el sistema es un todo y no varios componentes separados. Esta transparencia está separada en: acceso, ubicación, migración, re-localización, replicación, concurrencia y fallas.

### Apertura

Un sistema distribuido abierto es aquel que puede ofrecer servicios bajo reglas estándar (sintaxis y semántica). Permitiendo interoperabilidad, portabilidad y fácil extensión.

### Escalabilidad

Existen dos formas de escalabilidad, la horizontal, que implica que un sistema que crece en número de nodos de trabajo aumentando la cantidad de los recursos. También está la escalabilidad vertical que es aumentar la capacidad de los recursos, que se logra mejorando lo actual sin necesidad de agregar algo extra.

## Analisis de caracteristicas del sistema implementado

### Transparencia

- **Acceso**: Al trabajar con el módulo de Spring data, es posible abstraerse de la ubicación de la base de datos, por lo cual no importa donde esta se encuentre, es posible acceder a los datos tanto de forma local como remota.
- **Ubicación**: El recurso no se encuentra disponibilizado bajo un dominio web utilizando el protocolo DNS, por lo cual no se cumple con esta característica.
- **migración**: 
- **re-localización**:
- **Replicación**: El recurso no se encuentra replicado por lo tanto no cumple esta características. Sin embargo, al estar corriendo sobre contenedores, es posible crear réplicas y utilizar un balanceador para que sea transparente por el usuario.
- **Concurrencia**: Spring tiene la capacidad de manejar múltiples solicitudes de forma concurrente a través de hilos, por lo cual, es posible que haya más de un usuario utilizando el recurso sin notarlo.
- **Fallas**: Si alguno de los componentes falla (Front-End, Back-End o Base de datos) sería visible para el usuario, ya que el sistema quedaría inutilizable en alguno de estos casos al no contar con réplicas.

### Escalabilidad

- **Horizontal**: El sistema no posee una escalabilidad horizontal, sin embargo, esta característica se vuelve implementable debido a la utilización de contenedores. Con Kubernetes es posible utilizar la imagen de un contenedor para crear réplicas de esta que sean dinámicamente escalables en función de la demanda en el sistema.
- **Vertical**: Ya que el sistema está montado sobre contenedores, es posible modificar la asignación de recursos en estos para obtener un mejor rendimiento de la aplicación cuando sea necesario. Además, el servidor está montado en un droplet de Digitalocean, al cual es posible cambiar las características de su hardware cuando sea necesario sin alterar el contenido de este.

### Disponibilidad

En el estado actual del sistema, no es altamente disponible. Esto debido a que no existe un mecanismo de replicación de los datos. Si se llegan a perder los datos, no hay forma de ponerlos a disposición de manera rápida. Aun así, existen opciones para bases de datos PostgreSQL que permiten la replicación, que están basados en Triggers o ficheros WAL. Se considerará su implementación para mejorar el sistema.

### Apertura

El sistema si posee apertura, debido a que se encuentra implementado bajo una arquitectura de API REST utilizando spring boot framework para esto, por lo cual posee un acceso a sus servicios bajo reglas estándar, cumpliendo con la interoperabilidad, portabilidad y fácil extensión.

## TEST

Se realiza un test de inserción de datos sobre sobre el servicio de registrar un permiso.
Para este test se utilizó la herramienta de JMeter para simular una request de 1000 usuarios paralelos durante 60 segundos, en la cual los primeros 20 segundos son utilizados para ir incrementando el número de usuarios concurrentes de 0 a 1000.

- **Tabla de resumen**:

![alt text](tabla-resumen-test.png)

- **Grafico tiempo de respuesta**:

![alt text](grafico-tiempo-respuesta.png)


Como se puede observar en el gráfico, durante los primeros instantes del test cuando los usuarios concurrentes comenzaban a aumentar, el tiempo de respuesta se aproximaba a los 600 ms, sin embargo, a la medida que incrementaba esta cantidad de usuarios, el tiempo de respuesta también aumentó alcanzando a ser de más de 2 segundos.

Uso de recursos en el servidor durante el test:

![alt text](uso-recursos-test.png)
