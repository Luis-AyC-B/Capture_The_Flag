# PicoCTF - interencdec

## Descripción

Se proporciona un archivo con el siguiente contenido:

```text
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgya3lNRFJvYTJvMmZRPT0nCg==
```

El objetivo es descubrir el mensaje real oculto.

## Análisis

Al observar la cadena, se identifica que está codificada en **Base64**.

### Primera decodificación Base64

```text
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ=='
```

El resultado contiene otra cadena codificada en Base64 dentro de la representación de bytes de Python.

### Segunda decodificación Base64

Tomando únicamente:

```text
d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ==
```

y decodificándola nuevamente, obtenemos:

```text
wpjvJAM{jhlzhy_k3jy9wa3k_i204hkj6}
```

### Identificación del cifrado

La cadena resultante parece estar cifrada mediante un **Cifrado César**.

La secuencia inicial:

```text
wpjvJAM
```

se asemeja a:

```text
picoCTF
```

Aplicando un desplazamiento de **7 posiciones hacia atrás** a cada letra se obtiene:

```text
picoCTF{caesar_d3cr9pt3d_b204adc6}
```

## Solución

1. Decodificar Base64.
2. Extraer la cadena interna.
3. Decodificar Base64 nuevamente.
4. Aplicar un desplazamiento César de 7 posiciones hacia atrás.

## Flag

```text
picoCTF{caesar_d3cr9pt3d_b204adc6}
```

## Aprendizaje

* Reconocimiento de cadenas codificadas en Base64.
* Decodificación en múltiples capas.
* Identificación de patrones típicos de flags de picoCTF.
* Uso del cifrado César para ocultar texto legible.
