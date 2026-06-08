# CIDSI - PASE PERDIDO =') POR LA TÓXICA XD

## Descripción

Pablito faltó a clases y quiere participar en la 4 Competencia de Seguridad Informática CIDSI.

Uno de sus docentes le dejó la siguiente pista:

```text
494a55574b3354574d565847535a44504c354255535243544a465054454d425347493d3d3d3d3d3d
```

Formato de la flag:

```text
cidsi{flag_en_md5}
```

---

## Análisis

La cadena está compuesta únicamente por caracteres hexadecimales.

Se utiliza CyberChef con la operación:

```text
From Hex
```

obteniendo:

```text
IJUWK3TWMVXGSZDPL5BUSRCTJFPTEMBSGI======
```

Posteriormente se identifica que el resultado corresponde a Base32.

Utilizando:

```text
From Base32
```

se obtiene:

```text
Bienvenido_CIDSI_2022
```

Finalmente se calcula el hash MD5.

---

## Procedimiento

Texto recuperado:

```text
Bienvenido_CIDSI_2022
```

MD5:

```text
28e043ef6109375dafe8d541f2896bed
```

---

## Flag

```text
cidsi{28e043ef6109375dafe8d541f2896bed}
```

---

## Aprendizaje

* Identificación de cadenas en hexadecimal.
* Decodificación Base32.
* Uso de CyberChef para múltiples transformaciones.
* Aplicación de funciones hash MD5.
