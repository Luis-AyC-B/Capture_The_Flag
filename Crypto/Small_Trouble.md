# PicoCTF - Small Trouble

## Descripción

Everything seems secure; strong numbers, familiar parameters but something small might ruin it all.

Se proporcionan los valores:

```text
n = ...
e = ...
c = ...
```

junto con el código fuente utilizado para generar el cifrado RSA.

---

## Análisis

Al revisar el código se observa que el exponente privado se genera de la siguiente manera:

```python
d = getPrime(256)
```

Posteriormente se calcula:

```python
e = inverse(d, phi)
```

Aunque los primos RSA son grandes (1048 bits), el valor de `d` es relativamente pequeño.

Cuando el exponente privado RSA es demasiado pequeño, el sistema se vuelve vulnerable al **Ataque de Wiener**.

Este ataque utiliza fracciones continuas para aproximar:

```text
e / n
```

y recuperar el valor privado `d` sin necesidad de factorizar `n`.

---

## Vulnerabilidad

La debilidad se encuentra en:

```python
d = getPrime(256)
```

Un exponente privado pequeño puede ser recuperado mediante el Ataque de Wiener.

---

## Script de resolución

```python
# Wiener Attack
# (código mostrado arriba)
```

---

## Resultado

```text
picoCTF{sm4ll_d_a397771c}
```

---

## Flag

```text
picoCTF{sm4ll_d_a397771c}
```

---

## Aprendizaje

- Funcionamiento interno de RSA.
- Relación entre `e`, `d` y `φ(n)`.
- Ataque de Wiener contra exponentes privados pequeños.
- Uso de fracciones continuas en criptografía.
- Importancia de seleccionar correctamente el tamaño de `d`.
