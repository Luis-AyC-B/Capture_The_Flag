# CIDSI - Tocino

## Descripción

Puerco Araña nos dejó un mensaje en un idioma muy extraño.

```text
AAABABAAAAABAAAABBBABAABAABBABAABBABAAAAAAAAAAABABABAAAAAAAAAAAABAAAAAAAABAABBABABBAA
```

Formato de la flag:

```text
cidsi{flag_en_md5}
```

---

## Análisis

La secuencia únicamente contiene los caracteres A y B, característica típica del cifrado Bacon (Bacon Cipher).

Utilizando CyberChef con la operación:

```text
Bacon Cipher Decode
```

se obtiene:

```text
CRIPTOGRAFIABACON
```

Posteriormente se calcula el hash MD5 del mensaje recuperado.

---

## Procedimiento

Texto recuperado:

```text
CRIPTOGRAFIABACON
```

MD5:

```text
e1a6c781509bfe8bdd095fc05c6c4e82
```

---

## Flag

```text
cidsi{e1a6c781509bfe8bdd095fc05c6c4e82}
```

---

## Aprendizaje

* Reconocimiento del cifrado Bacon.
* Uso de CyberChef para decodificación.
* Aplicación de hashes MD5.
* Técnicas clásicas de criptografía histórica.
