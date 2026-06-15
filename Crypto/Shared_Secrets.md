# PicoCTF - Shared Secrets

## Descripción

A message was encrypted using a shared secret... but it looks like one side of the exchange leaked something. Can you piece together the secret and get the flag?

Se proporcionan:

- Un archivo `message.txt` con los parámetros utilizados.
- El código fuente `encryption.py`.

---

## Análisis

El código implementa un esquema basado en **Diffie-Hellman** para generar una clave compartida.

Se generan parámetros públicos:

```python
g = 2
p = getPrime(1048)
```

El servidor genera un secreto privado `a` y calcula:

```python
A = pow(g, a, p)
```

El cliente tiene un secreto privado `b` y calcula:

```python
B = pow(g, b, p)
```

La clave compartida se obtiene mediante:

```python
shared = pow(A, b, p)
```

Finalmente, la flag se cifra utilizando XOR:

```python
enc = bytes([x ^ (shared % 256) for x in flag])
```

---

## Vulnerabilidad

En un intercambio Diffie-Hellman los valores privados nunca deben revelarse.

Sin embargo, en el archivo proporcionado aparece:

```text
b = 502087552249276796768894199149546386713173741864561762918671131549146319658647813949433247424965048798816294966029262647803764533595143429273283374211302160540685383641060542870573303301014875733971557824236009184578986290165659257363419797500816452080900496604781986251988455903195756181696996025184087945715324970
```

Al conocer `b`, `A` y `p`, podemos calcular directamente la clave compartida:

```python
shared = pow(A, b, p)
```

---

## Script de resolución

```python
p = 1653798930689987750372209240014380521131540183716217687164747711336243702962818359267822691525697642105558753651223568056089606926425342081267821725904109431430327153613733358950243154522848602494020618427146508586350079988809469424456886589329449769221123659126892760967096413248127035734431548987006011015808526671

A = 771122236020803078829911570090382183223626843114693013412703353349864301811612864849857638111588507084769437566078749825291937213523446695097948166153379036322108656350710200734137906115055446496743841090323252143278700024424965369059879247648625799137192258413471893876530475007392243768366999108564494255853654467

b = 502087552249276796768894199149546386713173741864561762918671131549146319658647813949433247424965048798816294966029262647803764533595143429273283374211302160540685383641060542870573303301014875733971557824236009184578986290165659257363419797500816452080900496604781986251988455903195756181696996025184087945715324970

enc = bytes.fromhex(
    "cfd6dcd0fcebf9c4dbd7e0cc8cdccd8ccbe0dddb8c87d98c8889c2"
)

shared = pow(A, b, p)

key = shared % 256

flag = bytes([x ^ key for x in enc])

print(flag.decode())
```

---

## Ejecución

```bash
python3 shared_secrets.py
```

Salida:

```text
picoCTF{dh_s3cr3t_bd38f376}
```

---

## Flag

```text
picoCTF{dh_s3cr3t_bd38f376}
```

---

## Aprendizaje

- Funcionamiento básico de Diffie-Hellman.
- Importancia de mantener privados los exponentes secretos.
- Cálculo de claves compartidas usando `pow(base, exponente, modulo)`.
- Uso de XOR para cifrar y descifrar información.
- Cómo una fuga de información puede comprometer completamente un sistema criptográfico.
