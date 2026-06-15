# PicoCTF - Undo

## Descripción

Can you reverse a series of Linux text transformations to recover the original flag?

El reto proporciona un servicio accesible mediante Netcat:

```bash
nc foggy-cliff.picoctf.net 64402
```

El objetivo es revertir una serie de transformaciones aplicadas a la flag utilizando comandos de Linux.

---

## Análisis

Al conectarse al servicio, se muestran diferentes versiones transformadas de la flag junto con una pista indicando qué transformación fue aplicada.

La estrategia consiste en identificar la transformación y ejecutar el comando Linux que la revierte.

Las transformaciones observadas fueron:

1. Codificación Base64.
2. Inversión de texto.
3. Reemplazo de guiones bajos por guiones.
4. Reemplazo de llaves por paréntesis.
5. Aplicación de ROT13.

---

## Procedimiento

### Paso 1: Decodificar Base64

Flag transformada:

```text
KTUzbjg4MDI3LWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj
```

Pista:

```text
Base64 encoded the string.
```

Comando utilizado:

```bash
base64 -d
```

---

### Paso 2: Revertir texto invertido

Flag transformada:

```text
)53n88027-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc
```

Pista:

```text
Reversed the text.
```

Comando utilizado:

```bash
rev
```

---

### Paso 3: Restaurar guiones bajos

Flag transformada:

```text
cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-72088n35)
```

Pista:

```text
Replaced underscores with dashes.
```

Comando utilizado:

```bash
tr '-' '_'
```

---

### Paso 4: Restaurar llaves

Flag transformada:

```text
cvpbPGS(Eriref1at_g3kg_Ge4afs0ez@g10af_72088n35)
```

Pista:

```text
Replaced curly braces with parentheses.
```

Comando utilizado:

```bash
tr '()' '{}'
```

---

### Paso 5: Revertir ROT13

Flag transformada:

```text
cvpbPGS{Eriref1at_g3kg_Ge4afs0ez@g10af_72088n35}
```

Pista:

```text
Applied ROT13 to letters.
```

Comando utilizado:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

---

## Resultado

Después de completar correctamente todas las transformaciones, el servicio devuelve:

```text
Congratulations! You've recovered the original flag:
```

```text
picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_72088a35}
```

---

## Flag

```text
picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_72088a35}
```

---

## Aprendizaje

- Uso de Netcat para interactuar con servicios remotos.
- Decodificación Base64 en Linux.
- Manipulación de cadenas utilizando `rev`.
- Sustitución de caracteres mediante `tr`.
- Comprensión y reversión del cifrado ROT13.
- Identificación de transformaciones comunes utilizadas en retos CTF.
- Aplicación práctica de comandos básicos de Linux para análisis y recuperación de información.
