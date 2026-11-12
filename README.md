# ¿Por qué el frontend no necesita conectarse a PostgreSQL?

Porque el frontend se comunica con la base de datos a través del backend mediante la API, no se comunica directamente con el Postgres.

# ¿Por qué el backend pertenece a dos redes?

Porque, como hemos dicho, es el intermediario entre el frontend y la BD. Se comunica a través de una red con el front, y a través de otra con el Postgres.

# ¿Qué servicio necesita publicar un puerto en el anfitrión?

Solo el front, para que podamos acceder desde el navegador. En mi configuración, conectamos el puerto indicado en PUERTO_WEB del equipo con el puerto 8080 del contenedor. El backend y PostgreSQL se comunica mediante las redes internas y no necesitan publicar puertos.

# ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?

Publicar el puerto de Postgre permitiría acceder desde fuera sin resolver el problema. Lo suyo es corregir la conexión o configuración en la red interna de datos.

# ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?

Nos permite arrancar los tres servicios a la vez con un solo comando, guardando la configuración en un solo archivo. Evitamos repetir comandos y podemos reproducir el despliegue en otras máquinas fácilmente.

# ¿Por qué hemos dividido la arquitectura en dos redes?

Como ya hemos dicho, para que cada servicio se comunique solo con los componentes que necesita. El front con el back sin acceso directo a la BD, y el back a la BD, quedando la base aislada del front.

# ¿Por qué el backend utiliza db como dirección de PostgreSQL?

Porque db es el nombre que le hemos puesto al servicio de la base de datos en el YAML. Docker reconoce el nombre y permite que el back encuentre PostreSQL sin tener que indicar su IP. Si usasemos localhost, el back buscaría la base de datos dentro de su propio contenedor.

# ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?

Detenerlo conseva los contenedores y los datos. Eliminar los volúmenes borra contenedores y datos, por lo que se pierden los datos de la base de datos.

# ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?

Se perderían las incidencias y toda la info almacenada en la BD. Si no hay una copia de seguridad, no se podría recuperar la información y habría que introducir los datos otra vez. 