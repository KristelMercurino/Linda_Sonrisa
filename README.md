# LINDA SONRISA

## FRONTEND

## BACKEND

### Variables de entorno

Para poder ejecutar el proyecto se debe crear un archivo de nombre **.env** en el directorio BACKEND_FLASK (sin nombre antes de la extension para este caso) dentro del proyecto donde el cuerpo del archivo debe contener la siguiente estructura donde la descripcion entre {} incluyendolos, se debe reemplazar por su valor esperado segun se indique:

```
DB_USER={oracle cloud user}
DB_PASS={oracle cloud pass}
DB_HOST={oracle cloud host}
DB_PORT={oracle cloud port}
DB_SERVICE_NAME={oracle cloud service name}
DSN={dsn name on .ora file}
SECRET_KEY={hashed key}
PUBLIC_KEY={mercadopago public key}
ACCESS_TOKEN={mercadopago access token}
```
---

Adicionalmente, se debe crear un archivo con nombre vars.js en el directorio `LINDA_SONRISA > scripts` del proyecto, el cual debe contener la definición de la siguiente variable:

```
MP_PK={mercadopago public key}
```

# Flask backend usuario cliente

## Base de datos

Para la conexion a las base de datos se utilizo instaclient_21_7, al cual se agrego los archivo necesarios del wallet para poder acceder a la base de datos en la nube de oracle. Algunas definiciones para poder acceder a la base de datos se encuentran en como variables de entorno en el archivo .env de este proyecto.

Luego para la conexion se usa SQLAlchemy pero hay problemas con ciertas funcionalidades por lo que selects con clausulas o inserts se ejecutan directamente en los servicios mediante consultas sql con f-strings de python. Ejemplo:

```
insert = f"""insert into cliente(rut_cliente, dvrut_cliente, nombre_cliente, apaterno_cli, amaterno_cli, fecha_nac_cli, genero, telefono, direccion, email, id_ficha_cli, id_comuna, id_tramo_prev, clave_cli, id_pago) 
    values('{RUT_CLIENTE}', '{DVRUT_CLIENTE}', '{NOMBRE_CLIENTE}', '{APATERNO_CLI}', '{AMATERNO_CLI}', to_date('{FECHA_NAC_CLI}', 'dd/mm/yyyy'), '{GENERO}', {TELEFONO}, '{DIRECCION}', '{EMAIL}', {new_ficha_id}, {ID_COMUNA}, {ID_TRAMO_PREV}, '{pass_hash}', 'P005')"""

try:
    db.session.execute(insert)
    db.session.commit()
except SQLAlchemyError as e:
    print(e)
    db.session.rollback()
    return {"response": "error en la creacion del cliente"}, 400
```

Mas informacion se puede encontrar en el siguiente [link](https://www.geeksforgeeks.org/how-to-execute-raw-sql-in-flask-sqlalchemy-app/)


## Estructura proyecto

- api:
(Carpeta principal del backend del proyecto)
  - auth: json web token y autenticación de usuarios
  - models: todos los modelos de la bd (faltan los no usados por cliente)
  - routes : se definen todos los servicios
  modularizados por Blueprints.

  Lo que hay que agregar: Los html de cada uno, los modelos de tablas que hagan falta para cada usuario, y los routes que son los servicios segun haga falta.
  

VIDEOS DE AYUDA CON FLASK:
https://www.youtube.com/watch?v=BfEjQTYgvhU
https://www.youtube.com/watch?v=Esdj9wlBOaI