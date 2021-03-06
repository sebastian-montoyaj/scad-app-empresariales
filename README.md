# YoTeArriendo.com ~ Agencia SCAD
Repositorio para el proyecto de desarrollo de aplicaciones empresariales.

[ ![Codeship Status for sebastian-montoyaj/scad-app-empresariales](https://app.codeship.com/projects/720d8df0-3b8f-0136-f448-5e3eb9768dd6/status?branch=master)](https://app.codeship.com/projects/290356)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/5a43a18797f949029d03d09b5aab4beb)](https://www.codacy.com/app/sebastian-montoyaj/scad-app-empresariales?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=sebastian-montoyaj/scad-app-empresariales&amp;utm_campaign=Badge_Grade)
[![Codacy Badge](https://api.codacy.com/project/badge/Coverage/5a43a18797f949029d03d09b5aab4beb)](https://www.codacy.com/app/sebastian-montoyaj/scad-app-empresariales?utm_source=github.com&utm_medium=referral&utm_content=sebastian-montoyaj/scad-app-empresariales&utm_campaign=Badge_Coverage)

***

## :busts_in_silhouette: Integrantes:
+ Jean Carlos Herrera Meza ~ jean.herrera@udea.edu.co
+ Juan Diego Goez Durango ~ diego.goez@udea.edu.co
+ Raúl Antonio Martínez Silgado ~ rantonio.martinez@udea.edu.co
+ Sebastián Montoya Jiménez ~ sebastian.montoyaj@udea.edu.co

***

## Funcionalidades

 * [Ver información agencia](#ver-información-agencia)  
 * [Ver inmuebles](#ver-inmuebles)  
 * [Buscar inmuebles](#buscar-inmuebles)  
 * [Hacer una reserva](#hacer-una-reserva)  
 * [Consultar reservas](#consultar-reservas)  
 * [Cancelar una reserva](#cancelar-una-reserva)

***

## Ver información agencia  

> ### GET     /v1/agency/info

#### Descripción
Devuelve la información de la agencia (Arrendamientos Scad).

#### JSON entrada
N/A

#### JSON salida
```
{
    "nit":"1234-4567-00048-6553",
    "name":"Arrendamientos SCAD",
    "description":"Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
}
```

## Ver inmuebles  

> ### GET     /v1/homes/all          

#### Descripción
Devuelve todas las casas registradas en el sistema.

#### JSON entrada
N/A

#### JSON salida
```
[
    {
        "id": 1,
        "name": "Las Brisas 2",
        "description": "40m2, 2 habitaciones y un baño",
        "address": "Calle 56a #49-70",
        "latitude": "41.40338",
        "longitude": "2.17403",
        "city": "CO-MDE",
        "type": 1,
        "rating": 4.2,
        "pricePerNight": 100.25,
        "thumbnail": "https://es.seaicons.com/wp-content/uploads/2016/09/Actions-go-home-icon.png",
        "agencyCode": "1234-4567-00048-6553"
    },
    {
        "id": 2,
        "name": "Essenza",
        "description": "60m2, 5 habitaciones, 2 baños y solar",
        "address": "Circular 2 #74-43",
        "latitude": "16.56648",
        "longitude": "5.15665",
        "city": "CO-BGA",
        "type": 2,
        "rating": 3.75,
        "pricePerNight": 216.15,
        "thumbnail": "http://pngimages.net/sites/default/files/home-png-image-41830.png",
        "agencyCode": "1234-4567-00048-6553"
    }
]
```

## Buscar inmuebles  

> ### POST    /v1/homes/search     

#### Descripción
Devuelve las casas registradas según los parámetros de entrada.

#### JSON entrada
```
{
    "checkIn": "07-04-2018",
    "checkOut": "10-04-2018",
    "city": "MED",
    "type": "1"
}
```

#### JSON salida

##### En caso de éxito
```
{
    "agency": {
        "name": "Arrendamientos Santa Fé",
        "nit": "1123-1233-12313-51414"
    },
    "homes": [
        {
            "id": 1 ,
            "name": "Torre davivienda",
            "description": "Apartamento muy comodito",
            "location": {
                "address": "Avenida siempre viva 123",
                "latitude": "4.1231231",
                "longitude": "-421213212"
            },
            "city": "Medellín",
            "type": "Apartamento",
            "rating": "4.9",
            "totalAmount": "932.234",
            "pricePerNight": "100.233",
            "thumbnail": "https://goo.gl/sccYgg"
        }, 
        {
            "id": 2 ,
            "name": "La quinta porra",
            "description": "Apartamento muy lejos",
            "location": {
                "address": "Cra 1 # 1 A 1",
                "latitude": "5.12",
                "longitude": "21213212"
            },
            "city": "Medellín",
            "type": "Casa",
            "rating": "2",
            "totalAmount": "50.000",
            "pricePerNight": "20.000",
            "thumbnail": "https://goo.gl/sccYgg"
        }
    ]
}
```

##### En caso de error - Caso 1: Request vacio
```
{
	"status": "Error",
	"message": "Request vacio!!!"
}
```

##### En caso de error - Caso 2: No estan todos los parametros
```
{
	"status": "Error",
	"message": "Request no tiene todos los parametros indicados"
}
```

##### En caso de error - Caso 3: Las fechas deben ser tipo String
```
{
	"status": "Error",
	"message": "Las fechas deben ser tipo String"
}
```

##### En caso de error - Caso 4: Las fechas deben tener el formato DD/MM/YYYY o DD-MM-YYYY
```
{
	"status": "Error",
	"message": "Las fechas no tienen el formato DD/MM/YYYY o DD-MM-YYYY"
}
```

##### En caso de error - Caso 5: El parametro city debe ser un string
```
{
	"status": "Error",
	"message": "El tipo del parametro 'city' debe ser String"
}
```

##### En caso de error - Caso 6: Fechas invertidas
```
{
	"status": "Error",
	"message": "Las fecha de partida no puede ser anterior a la fecha de llegada!"
}
```

##### En caso de error - Caso 7: Reserva no puede ser de CERO dias
```
{
	"status": "Error",
	"message": "La reserva debe ser de por lo menos de un dia!"
}
```

## Hacer una reserva  

> ### POST    /v1/homes/booking

#### Descripción
Realiza la reserva del inmueble segun los parametros de entrada. Esta función requiere que dentro del encabezado de la petición se encuentre el token de autenticación del usuario.

#### Cabecera de la petición
```
	"token": "123456789..."
```

#### JSON entrada
```
{
	"checkIn": "04-05-2019",
	"checkOut": "21-05-2019",
	"id": 9
}
```

#### JSON salida

##### En caso de éxito
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 1,
	"mensaje": "Reserva con exito!!!"
}
```

##### En caso de error - Caso 1: Request vacio
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Request vacio!!!"
}
```

##### En caso de error - Caso 2: No estan todos los parametros
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Request no tiene todos los parametros indicados"
}
```

##### En caso de error - Caso 3: No se envio token de autenticacion
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "No hay ninguna clave token en el encabezado"
}
```

##### En caso de error - Caso 4: Token no valido
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Token de usuario invalido"
}
```

##### En caso de error - Caso 5: Fechas con formato incorrecto (Ej: Numeros)
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Las fechas deben ser tipo String"
}
```

##### En caso de error - Caso 6: Id de la casa no es un numero
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "El ID del inmueble debe ser un numero"
}
```

##### En caso de error - Caso 7: Id de la casa no existe
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "El ID del inmueble no existe en la BD"
}
```

##### En caso de error - Caso 8: Fechas sin formato correcto
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Las fechas no tienen el formato DD/MM/YYYY o DD-MM-YYYY"
}
```

##### En caso de error - Caso 9: Fechas invertidas
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Las fecha de partida no puede ser anterior a la fecha de llegada!"
}
```

##### En caso de error - Caso 10: Reserva de CERO DIAS
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "La reserva debe ser de por lo menos de un dia!"
}
```

##### En caso de error - Caso 11: Fechas solapadas
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 0,
	"mensaje": "Reserva invalida por fechas solapadas! El inmueble esta ocupado del 15-5-2019 al 19-5-2019"
}
```

## Consultar reservas  

> ### POST    /v1/homes/myBooking

#### Descripción
Devuelve todas las reservas asociadas al token dado en el encabezado de la petición.

#### Cabecera de la petición
```
	"token": "123456789..."
```

#### JSON entrada
N/A

#### JSON salida

##### En caso de éxito
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "homes": [
        {
            "id": 1,
            "name": "Las Brisas 2",
            "description": "40m2, 2 habitaciones y un baño",
            "location": {
                "address": "Calle 56a #49-70",
                "latitude": "41.40338",
                "longitude": "2.17403"
            },
            "city": "Colombia-Medellin",
            "type": "Apartamento",
            "rating": 4.2,
            "totalAmount": 300.75,
            "pricePerNight": 100.25,
            "thumbnail": "https://es.seaicons.com/wp-content/uploads/2016/09/Actions-go-home-icon.png",
            "booking": [
                {
                    "checkIn": "11-5-2018",
                    "checkOut": "12-5-2018",
                    "totalAmount": 100.25,
                    "bookingId": "9"
                },
                {
                    "checkIn": "18-5-2018",
                    "checkOut": "20-5-2018",
                    "totalAmount": 200.5,
                    "bookingId": "8"
                }
            ]
        },
        {
            "id": 2,
            "name": "Essenza",
            "description": "60m2, 5 habitaciones, 2 baños y solar",
            "location": {
                "address": "Circular 2 #74-43",
                "latitude": "16.56648",
                "longitude": "5.15665"
            },
            "city": "Colombia-Bucaramanga",
            "type": "Casa",
            "rating": 3.75,
            "totalAmount": 432.3,
            "pricePerNight": 216.15,
            "thumbnail": "http://pngimages.net/sites/default/files/home-png-image-41830.png",
            "booking": [
                {
                    "checkIn": "18-5-2018",
                    "checkOut": "20-5-2018",
                    "totalAmount": 432.3,
                    "bookingId": "7"
                }
            ]
        }
    ]
}
```

##### En caso de error - Caso 1: No se envió token de autenticación
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "No hay ninguna clave token en el encabezado"
}
```

##### En caso de error - Caso 2: Token no valido
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "Token de usuario invalido"
}
```

## Cancelar una reserva  

> ### DELETE /v1/homes/removeBooking

#### Descripción
Elimina la reserva especificada en el json de entrada solo si el token entregado en la cabecera es valido.

#### Cabecera de la petición
```
	"token": "123456789..."
```

#### JSON entrada
```
{
	"bookingId": "X@/157"
}
```

#### JSON salida

##### En caso de éxito
```
{
	"agency": {
		"nit": "1234-4567-00048-6553",
		"name": "Arrendamientos SCAD",
		"description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
	},
	"codigo": 1,
	"mensaje": "Cancelacion con exito!!!"
}
```

##### En caso de error - Caso 1: Request vacio
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "Request vacio!!!"
}
```

##### En caso de error - Caso 2: No hay ningún campo bookingId en el json request
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "Request no tiene la clave bookingId"
}
```

##### En caso de error - Caso 3: No se envió token de autenticación
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "No hay ninguna clave token en el encabezado"
}
```

##### En caso de error - Caso 4: Token no valido
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "Token de usuario invalido"
}
```

##### En caso de error - Caso 5: El campo bookingId debe ser tipo texto
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "El ID de la reserva debe ser tipo texto"
}
```

##### En caso de error - Caso 6: No hay ninguna reserva con el id indicado
```
{
    "agency": {
        "nit": "1234-4567-00048-6553",
        "name": "Arrendamientos SCAD",
        "description": "Agencia de arrendamientos para estudiantes de la Universidad de Antioquia."
    },
    "codigo": 0,
    "mensaje": "No existe una reserva con el ID especificado en la BD"
}
```
