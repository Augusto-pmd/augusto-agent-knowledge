---

# API Contract — PMD Sistema Base v1

## Convenciones Generales

- Todos los endpoints viven bajo el prefijo `/api`.

- El mecanismo de autenticación oficial es `Authorization: Bearer <JWT>`.

- El contenido se intercambia exclusivamente en formato JSON.

- Los identificadores son UUID en formato string.

- Las fechas se expresan en formato ISO 8601.

## Endpoints de Autenticación

### POST /api/auth/login

Request:

```json
{ "email": "string", "password": "string" }





Response 200:



{

  "accessToken": "string",

  "user": UserDTO

}





Errores:



400 → DTO inválido



401 → Credenciales inválidas



429 → Rate limit activo



GET /api/auth/refresh



Response 200:



{

  "accessToken": "string",

  "user": UserDTO

}



Usuario

GET /api/users/me



Response 200:



UserDTO





Errores:



401 → No autenticado



403 → Rol no autorizado



DTOs Congelados

UserDTO

{

  id: string;

  email: string;

  fullName: string;

  role: {

    id: string;

    name: string;

    permissions: string[];

  };

  organization: {

    id: string;

    name: string;

  };

}





Reglas:



El frontend nunca consume entidades crudas.



Solo se consumen DTOs normalizados.



Formato de Error

{

  "statusCode": number,

  "message": string | string[],

  "timestamp": string,

  "path": string

}





El frontend interpreta únicamente statusCode y message.



Este contrato queda congelado como fuente de verdad del PMD Sistema Base v1

y como base operativa del IAS.
