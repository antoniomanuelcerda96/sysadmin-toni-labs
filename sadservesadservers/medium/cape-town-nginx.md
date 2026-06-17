# Cape Town - Nginx Troubleshooting

## Objetivo

Restaurar el funcionamiento de un servidor Nginx que devolvía errores al acceder mediante HTTP.

## Herramientas utilizadas

- curl
- systemctl
- journalctl
- nginx
- systemd
- nano

## Análisis

El servicio Nginx no respondía correctamente a las peticiones HTTP.

La investigación comenzó verificando el estado del servicio:

```bash
systemctl status nginx
```

El servicio aparecía como `failed`.

Posteriormente se revisaron los logs:

```bash
journalctl -u nginx
```

y se validó la configuración:

```bash
sudo nginx -t
```

## Problema 1

Se detectó un error de sintaxis en:

```text
/etc/nginx/sites-enabled/default
```

Existía un carácter `;` inesperado al inicio del fichero.

Tras corregirlo:

```bash
sudo nginx -t
```

devolvió:

```text
syntax is ok
test is successful
```

## Problema 2

Aunque Nginx arrancaba correctamente, las peticiones HTTP devolvían:

```text
HTTP/1.1 500 Internal Server Error
```

Revisando los logs se encontraron errores:

```text
Too many open files
```

La configuración de systemd mostraba:

```text
LimitNOFILE=10
```

Este valor impedía que Nginx pudiera abrir los ficheros y sockets necesarios.

Se modificó el límite a un valor adecuado y se recargó la configuración de systemd.

## Verificación

Se reinició el servicio:

```bash
sudo systemctl restart nginx
```

Y finalmente se comprobó:

```bash
curl -Is 127.0.0.1:80 | head -1
```

Resultado:

```text
HTTP/1.1 200 OK
```

## Aprendizaje

Durante este escenario practiqué:

- Troubleshooting de Nginx.
- Diagnóstico mediante logs.
- Validación de configuración con `nginx -t`.
- Gestión de servicios con systemd.
- Interpretación de errores HTTP.
- Límites de recursos en Linux (`LimitNOFILE`).

## Competencias desarrolladas

- Linux Administration
- Systemd
- Nginx
- Log Analysis
- Troubleshooting
- Incident Response
- Infrastructure
