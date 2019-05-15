---
title: PagaDespues API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Contactanos</a>

includes:
  - errors

search: true
---

# Introduccion

Hola 👋 bienvenido a la documentacion de [Pagadespues](http://pagadespues.pe)


## Puntos de entrada

Ambiente | Url | Description
--------- | ------- | -----------
Produccion | https://api.pagadespues.com/v1/ | Solicitar credenciales de acceso.
Desarrollo | https://api.sandbox.pagadespues.com/v1/ | Uso publico de prueba.

## Flujo de trabajo
<img src="./images/flujo.jpeg"
     alt="Flujo de trabajo"
     style="float: left; margin-bottom: 20px;" />


1. Merchant calls the /v1/configuration/ endpoint to refresh Afterpay configuration.
2. Merchant determines if the checkout should display Afterpay as a payment option.
3. Merchant calls the /v1/orders/ endpoint to initiate the Afterpay payment process.
4. The order token to return and stored by the merchant.
5. Merchant executes the AfterPay.redirect({token: YOUR_TOKEN}); JavaScript function within 6. the page to redirect to the Afterpay login page.
7. The consumer’s order is preapproved for a subsequent payment request.
8. If the merchant is able to fulfill the order, the merchant calls the /v1/payments
9. Afterpay finalises the consumers payment and responds with a success or failure.

# Authentication

Se usa la [autenticacion basica](http://example.com/developers). Ejemplo:

Merchant ID | Secret Key
--------- | -------
jdh212jnwq | nsuwn2kqj1jwq

Merchant ID es el usuario y el Secret Key es la contraseña. Las credenciales 
deben ser codificadas en Base64 para usarlar en la cabecera de  Authorization.

Texto plano | Codificado
--------- | -------
jdh212jnwq:nsuwn2kqj1jwq | =wefwefwensuwn2kqj1jwq

|Cabecera|
|---------|
|Authorization: Basic =wefwefwensuwn2kqj1jwq|

# Modelos

## Comercio
### Merchant

> Objecto comercio de ejemplo:

```json
{
  "id": "5cd992ef75251b",
  "name": "Mi comercio",
  "socialIdentity": "Mi comercio S.A.C.",
  "documentType": "ruc",
  "documentNumber": "30601708142",
  "dominio" : "comercio.com",
  "logo" : "https://comercio.com/logo.png",
  "contactPerson": [
    {
      "name": "Juan Antonio",
      "lastName": "Perez Silva",
      "email": "pjuan@micomercio.pe",
      "phone": "958246343"
    }
  ]
}
```

Parametro | Tipo | Detalle
--------- | ------- | -----------
id | string | Identificador
name | string | Nombre comercial
socialIdentity | string | Razon social
documentType | string | Tipo de documento de identificacion
documentNumber | string | Numero de documento de identificacion
contactPerson | Array(ContantPerson) | Personas de contacto

### ContantPerson

> Objecto persona de contacto de ejemplo:

```json
{
  "name": "Juan Antonio",
  "lastName": "Perez Silva",
  "email": "pjuan@micomercio.pe",
  "phone": "958246343"
}
```

Parametro | Tipo | Detalle
--------- | ------- | -----------
name | string | Nombres
lastName | string | Apellidos
email | string | Correo de contacto
phone | string | Numero telefonico


## Producto
### Product

> Objecto producto de ejemplo:

```json
{
  "name": "Zapato",
  "type": "F",
  "id" : "adb223jfn2j2m4",
  "metadata" : {},
  "amount": 100.0,
  "currency": "PEN",
}
```

Parametro | Tipo | Detalle
--------- | ------- | -----------
name | string | Nombre producto
type | string | Tipo de producto (opciones en la tabla inferior)
id | string | Identificar del producto (Comercio)
metadata | object | Propiedades agragadas por el comercio
amount | number | Precio del producto
currency | string | Moneda del importe del producto (opciones en la tabla inferior)
image | string | Url de la imagen del producto

### Tipos de productos

Tipo | Detalle
--------- | -------
F | Fisico
E | Evento
T | Ticket

### Moneda

Tipo | Detalle
--------- | -------
PEN | Soles
USD | Dolares

<aside class="warning">Para los productos en USD se tomara como referencia el tipo de kambio de kambista</aside>


# Merchant

## Obtener informacion del comercio

```shell
curl "https://api.sandbox.pagadespues.com/v1/merchant/config"
  -H "Authorization: Basic =wefwefwensuwn2kqj1jwq"
```

> Ejemplo de response:

```json
{
  "active" : true,
  {
    "id": "5cd992ef75251b",
    "name": "Mi comercio",
    "socialIdentity": "Mi comercio S.A.C.",
    "documentType": "ruc",
    "documentNumber": "30601708142",
    "dominio" : "comercio.com",
    "logo" : "https://comercio.com/logo.png",
    "contactPerson": [
      {
        "name": "Juan Antonio",
        "lastName": "Perez Silva",
        "email": "pjuan@micomercio.pe",
        "phone": "958246343"
      }
    ]
  }
}
```
Este servicio response los parametros de configuracion del comercio y su estatus.

### HTTP Request

`GET https://api.sandbox.pagadespues.com/v1/merchant/config`

### HTTP Headers

Parameter |  Description
--------- | -------
Authorization | Ver [autenticacion](#authentication)
Accept | `application/json`

### Response

Parametro | Tipo | Detalle
--------- | ------- | -----------
active | boolen | Estado del convenido con el comercio
merchant | [Merchant](#comercio) | Detalles del comercio



# Check

## Crear cotizacion

```shell
curl "https://api.sandbox.pagadespues.com/v1/check"
  -H "Authorization: Basic =wefwefwensuwn2kqj1jwq"
```

> Ejemplo de response:

```json
{
  "url" : "https://pagadespues.com/marchant-checkout?check=32ng4n282bnd"
}
```
Este servicio responde el url para el iframe de pago

### HTTP Request

`GET https://api.sandbox.pagadespues.com/v1/check`

### HTTP Headers

Parameter |  Description
--------- | -------
Authorization | Ver [autenticacion](#authentication)
Content-Type | `application/json`
Accept | `application/json`

### Response

Parametro | Tipo | Detalle
--------- | ------- | -----------
url | string | Url del iframe de pago
