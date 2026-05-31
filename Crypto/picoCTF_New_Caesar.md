# PicoCTF - New Caesar

## Descripción

Se proporciona el siguiente texto cifrado:

```text
fegdeogdgecoeocgcgchcfcffccfca
```

Además, se entrega el archivo `new_caesar.py`, que contiene la implementación del algoritmo de cifrado.

## Análisis del código

El script utiliza un alfabeto reducido:

```python
ALPHABET = "abcdefghijklmnop"
```

La función `b16_encode()` convierte cada carácter del texto original en dos símbolos pertenecientes a dicho alfabeto.

Posteriormente, la función `shift()` aplica un desplazamiento similar a un cifrado César:

```python
def shift(c, k):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 + t2) % len(ALPHABET)]
```

La clave tiene una longitud de un solo carácter:

```python
assert len(key) == 1
```

Por lo tanto, únicamente existen 16 claves posibles (`a`–`p`), lo que hace viable un ataque por fuerza bruta.

## Script de resolución

Se creó el siguiente script para probar automáticamente todas las claves posibles y reconstruir el texto original:

```python
import string

ALPHABET = string.ascii_lowercase[:16]
enc = "fegdeogdgecoeocgcgchcfcffccfca"

for key in ALPHABET:
    dec_b16 = ""

    for c in enc:
        t1 = ALPHABET.index(c)
        t2 = ALPHABET.index(key)
        dec_b16 += ALPHABET[(t1 - t2) % 16]

    try:
        flag = ""

        for i in range(0, len(dec_b16), 2):
            a = ALPHABET.index(dec_b16[i])
            b = ALPHABET.index(dec_b16[i + 1])

            binary = format(a, "04b") + format(b, "04b")
            flag += chr(int(binary, 2))

        print(f"Clave {key}: {repr(flag)}")

    except:
        pass
```

## Resultado

Al ejecutar el script:

```bash
python3 solve.py
```

se obtiene una salida legible para la clave:

```text
p
```

Texto recuperado:

```text
et_tu?_77866c61
```

## Flag

```text
picoCTF{et_tu?_77866c61}
```

## Aprendizaje

* Análisis de código fuente para comprender algoritmos personalizados.
* Uso de fuerza bruta cuando el espacio de búsqueda es pequeño.
* Inversión de una codificación Base16 personalizada.
* Aplicación de técnicas de criptografía básica en retos CTF.
