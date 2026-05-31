# PicoCTF - RSA Oracle

## Descripción

Se interceptaron dos archivos:

```text
password.enc
secret.enc
```

El reto proporciona un oracle RSA accesible mediante:

```bash
nc titan.picoctf.net 53416
```

El oracle permite:

```text
E --> encrypt
D --> decrypt
```

Sin embargo, al intentar descifrar directamente `password.enc` se obtiene:

```text
Lol, good try, can't decrypt that for you. Be creative and good luck
```

El objetivo es recuperar la contraseña y utilizarla para descifrar `secret.enc`.

---

## Archivos interceptados

### password.enc

```text
2336150584734702647514724021470643922433811330098144930425575029773908475892259185520495303353109615046654428965662643241365308392679139063000973730368839
```

### secret.enc

```text
Salted__...
```

La cabecera `Salted__` indica que fue cifrado con OpenSSL.

---

## Reconocimiento del Oracle

Se realizaron pruebas de cifrado:

### Cifrado de "2"

```text
encoded cleartext as Hex m: 32

ciphertext:
4707619883686427763240856106433203231481313994680729548861877810439954027216515481620077982254465432294427487895036699854948548980054737181231034760249505
```

Observación:

```text
"2" = 0x32 = 50 decimal
```

El oracle cifra el valor ASCII del texto introducido.

---

## Vulnerabilidad

RSA sin padding es multiplicativo:

\[
(c \cdot r^e)^d \equiv m \cdot r \pmod n
\]

Si conocemos:

\[
Enc(r)
\]

podemos construir:

\[
C' = C \cdot Enc(r) \pmod n
\]

El oracle descifrará:

\[
M' = M \cdot r
\]

permitiendo recuperar el mensaje original.

---

## Construcción del ciphertext modificado

Se calculó:

```text
731998038494036031066308952189253431649801679430284396848579835956523176699015636791027988955726033873142541759776347454676952310767304729012225852754540
```

y se envió al oracle:

```text
D
731998038494036031066308952189253431649801679430284396848579835956523176699015636791027988955726033873142541759776347454676952310767304729012225852754540
```

Respuesta:

```text
decrypted ciphertext as hex:
a9573f66360
```

---

## Recuperación de la contraseña

Convertimos:

```text
a9573f66360
```

a decimal:

```text
11637011932000
```

Como el resultado corresponde a:

```text
50 × contraseña
```

dividimos entre 50:

```text
232740238640
```

Convertimos a hexadecimal:

```text
3630663530
```

Convertimos a ASCII:

```text
60f50
```

### Contraseña recuperada

```text
60f50
```

---

## Descifrado del archivo secreto

Se utilizó OpenSSL:

```bash
openssl enc -d -aes-256-cbc -in secret.enc -pass pass:60f50
```

Resultado:

```text
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
```

---

## Flag

```text
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
```

---

## Aprendizaje

- Funcionamiento de RSA.
- Propiedad multiplicativa de RSA.
- Ataques de Chosen Ciphertext Attack (CCA).
- Riesgos de utilizar RSA sin padding seguro.
- Uso de OpenSSL para descifrar archivos protegidos con contraseña.
