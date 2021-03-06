# Pushiner REST API
La API REST le permite interactuar con Pushiner para cualquier cosa que pueda enviar una solicitud HTTP.
Por ejemplo:

* Enviar notificaciones a todos los ususarios de una aplicación
* Muy pronto muchas cosas más.

## Envió de notificaciones

Endpoint | Método HTTP | Parametros
------------ | ------------- | ------------
https://pushiner/api/v1/send_message/ | POST | token_user, token_app, title, message

## Parametros

Nombre de parámetro | Tipo | Obligatorio | Descripcion
------------ | ------------- | ------------ | ----------
token_user | String | Si | Su clave de usuario. Puede encontrar su ``token_user`` en la sección de configuración de su aplicación                             dentro del portal de desarrolladores.
token_app | String | Si | Su clave de aplicación. Puede encontrar su ``token_user`` en la sección de configuración de su     aplicación dentro del portal de desarrolladores.
title | String | Si | El título de la notificación.
message | String | Si | El contenido de la notificación. Es el detalle de la notificación.

## Enviando notificaciones

### PHP
```php
curl_setopt_array($ch = curl_init(), array(
  CURLOPT_URL => "https://pushiner/api/v1/send_message/",
  CURLOPT_POSTFIELDS => array(
    "token_user" => "my_token_user",
    "token_app" => "my_token_app",
    "title" => "mi título",
    "message" => "hola mundo",
  ),
  CURLOPT_SAFE_UPLOAD => true,
));
curl_exec($ch);
curl_close($ch);
```

### Laravel
Sendpush has its library for laravel 5. * [SendPush][1]

### Python
```py
#!/usr/bin/python 
import json, httplib 
connection = httplib.HTTPSConnection('https://pushiner', 443) 
connection.connect() 
connection.request('POST', '/api/v1/send_message/', json.dumps({ "token_user": "my_token_user", "token_app": "my_token_app", "title": "mi título", "message": "mi contenido"}), { "Content-Type": "application/json" } ) result = json.loads(connection.getresponse().read())
print result
```

### Ruby

```ruby
require "net/https"

url = URI.parse("https://pushiner.com/api/v1/send_message/")
req = Net::HTTP::Post.new(url.path)
req.set_form_data({
  :token_user => "my token_user",
  :token_app => "my token_app",
  :title => "mi título",
  :message => "mi contenido",
})
res = Net::HTTP.new(url.host, url.port)
res.use_ssl = true
res.verify_mode = OpenSSL::SSL::VERIFY_PEER
res.start {|http| http.request(req) }
```

### Consola
```sh
curl -s \
  --form-string "token_user=abc123" \
  --form-string "token_app=user123" \
  --form-string "title=mi título" \
  --form-string "message=mi contennido" \
  https://pushiner.com/api/v1/send_message/
```

## Manejo de respuesta

### Envío éxitoso
```js
{
  "message": "la notificacion fue enviada con éxito",
  "code": 200,
  "success": true
}
```

### Envío errado
```js
{
  "message": "mensaje de error",
  "code": 400,
  "success": false
}
```

[1]: https://github.com/evervasquez/laravel-sendpush
