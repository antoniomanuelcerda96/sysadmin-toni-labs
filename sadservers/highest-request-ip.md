# Highest Request IP

## Objetivo

Encontrar la IP que más peticiones realiza en un access.log.

## Herramientas utilizadas

- awk
- sort
- uniq
- head

## Procedimiento

1. Extraer la primera columna del log.
2. Ordenar las IPs.
3. Contar repeticiones.
4. Ordenar por frecuencia.
5. Obtener la IP más repetida.

## Aprendizaje

- Procesamiento de logs.
- Uso de pipelines.
- Análisis básico de tráfico web.
