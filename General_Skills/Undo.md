# PicoCTF - Bytemancy 1

## Descripción

Can you conjure the right bytes?

Se proporciona el código fuente del programa y un servicio accesible mediante Netcat:

```bash
nc foggy-cliff.picoctf.net 52391
```

El objetivo es analizar el código y enviar la entrada correcta para obtener la flag.

---

## Análisis

Al revisar el código fuente se observa la siguiente condición:

```python
if user_input == "\x65"*1751:
```

La secuencia:

```python
\x65
```

representa el valor hexadecimal `65`, que corresponde al carácter ASCII decimal `101`.

Verificando la tabla ASCII:

```text
101 -> e
```

Por lo tanto:

```python
"\x65"*1751
```

equivale a:

```python
"e"*1751
```

Es decir, una cadena compuesta por 1751 letras `e`.

Además, el programa proporciona una pista explícita:

```text
Send me ASCII DECIMAL 101 1751 times, side-by-side, no space.
```

Lo que confirma el análisis realizado sobre el código.

---

## Procedimiento

Conectarse al servicio:

```bash
nc foggy-cliff.picoctf.net 52391
```

Generar automáticamente 1751 caracteres `e` y enviarlos al servidor:

```bash
python3 -c 'print("e"*1751)' | nc foggy-cliff.picoctf.net 52391
```

Respuesta obtenida:

```text
picoCTF{h0w_m4ny_e's???_7dbc095c}
```

---

## Flag

```text
picoCTF{h0w_m4ny_e's???_7dbc095c}
```

---

## Aprendizaje

- Interpretación de caracteres ASCII.
- Conversión entre decimal, hexadecimal y caracteres.
- Lectura y análisis de código fuente en Python.
- Comprensión de secuencias de escape como `\x65`.
- Automatización de entradas mediante Python.
- Uso de Netcat para interactuar con servicios remotos.
- Importancia de analizar el código antes de intentar fuerza bruta o pruebas manuales.
