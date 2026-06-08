# CIDSI - Samuel

## Descripción

Nuestro amigo nos dejó este mensaje, al parecer trabaja ahí.

```text
-.-. . -. - .-. --- / -.. . / --. . ... - .. --- -. / -.. . / .. -. -.-. .. -.. . -. - . ... / .. -. ..-. --- .-. -- .- - .. -.-. --- ...
```

Formato de la flag:

```text
cidsi{Tu_flag_en_MD5}
```

---

## Análisis

El mensaje está compuesto por puntos y rayas, por lo que se identificó como código Morse.

Utilizando CyberChef y la operación:

```text
From Morse Code
```

se obtiene:

```text
CENTRODEGESTIONDEINCIDENTESINFORMATICOS
```

Posteriormente se calcula el hash MD5 del resultado.

---

## Procedimiento

Mensaje decodificado:

```text
CENTRODEGESTIONDEINCIDENTESINFORMATICOS
```

MD5:

```text
942fb4e8d4a076e1c38e446ad94cc378
```

---

## Flag

```text
cidsi{942fb4e8d4a076e1c38e446ad94cc378}
```

---

## Aprendizaje

* Identificación de código Morse.
* Uso de CyberChef para decodificación.
* Generación de hashes MD5.
* Reconocimiento de patrones de codificación clásicos.
