# picoCTF - Leaky AES

## Descripción

El servicio realiza una versión simplificada de AES:

```python
out = Sbox[data_byte ^ key_byte]
leak_buf.append(out & 0x01)
```

Por cada byte procesado se filtra el bit menos significativo de la salida de la S-Box.

El servidor no devuelve el ciphertext, únicamente la cantidad total de bits `1` obtenidos:

```python
return leak_buf.count(1)
```

**Objetivo:** recuperar la clave AES de 16 bytes.

---

## Análisis

La operación realizada por cada byte es:

```text
SBOX[plaintext[i] XOR key[i]]
```

El servidor devuelve:

```text
suma(bit0(SBOX[plaintext[i] XOR key[i]]))
```

Si se modifica únicamente una posición del plaintext, la diferencia observada en la fuga depende únicamente del byte de clave correspondiente.

---

## Recolección de firmas

Se envió un bloque completamente nulo:

```text
00000000000000000000000000000000
```

Resultado:

```text
BASE = 12
```

Posteriormente se alteró una sola posición utilizando:

```python
tests = [1, 2, 4, 8, 16, 32, 64, 128]
```

Para cada posición se calculó:

```python
delta = leak - BASE
```

Obteniendo firmas únicas.

---

## Script de recolección

```python
from pwn import *
import re

HOST="saturn.picoctf.net"
PORT=60978

tests=[1,2,4,8,16,32,64,128]

def query(pt):
    r=remote(HOST,PORT,timeout=5)
    r.recvuntil(b"hex:")
    r.sendline(pt.hex().encode())

    data=r.recvall(timeout=2).decode()
    r.close()

    m=re.search(r'(\d+)',data)
    return int(m.group(1))

base=query(bytes(16))
print("BASE =", base)

for pos in range(16):
    sig=[]

    for t in tests:
        pt=bytearray(16)
        pt[pos]=t

        leak=query(bytes(pt))
        sig.append(leak-base)

    print(pos, sig)
```

---

## Recuperación de candidatos

Se comparó cada firma observada contra todas las posibilidades de la S-Box.

```python
for k in range(256):
    base = SBOX[k] & 1

    sig = []
    for t in tests:
        sig.append((SBOX[k ^ t] & 1) - base)

    if sig == observed[pos]:
        matches.append(k)
```

Resultado:

```text
0  -> 74
1  -> c5
2  -> {0a,2a,57}
3  -> {49,5b,73}
4  -> {16,de}
5  -> 84
6  -> {09,89}
7  -> 90
8  -> ef
9  -> 67
10 -> 5a
11 -> 02
12 -> {16,de}
13 -> {c2,f4}
14 -> 69
15 -> cd
```

Quedaron:

```text
144 candidatos posibles
```

---

## Segunda fase

Para resolver las ambigüedades se utilizaron nuevos valores:

```python
tests = [3, 5, 7, 15]
```

Únicamente para las posiciones ambiguas:

```python
[2, 3, 4, 6, 12, 13]
```

Resultado:

```text
2  [-1,-1,0,-1]
3  [-1,-1,-1,-1]
4  [0,0,0,-1]
6  [-1,-1,0,-1]
12 [0,0,-1,-1]
13 [-1,0,0,0]
```

Estas nuevas firmas permitieron seleccionar un único candidato para cada posición.

---

## Clave recuperada

```text
74 c5 57 5b de 84 89 90
ef 67 5a 02 16 f4 69 cd
```

Hexadecimal:

```text
74c5575bde848990ef675a0216f469cd
```

---

## Flag

```text
picoCTF{74c5575bde848990ef675a0216f469cd}
```

---

## Conclusión

Este reto demuestra una vulnerabilidad de tipo **Side-Channel Attack**. Aunque el sistema nunca revela directamente el ciphertext ni la clave, la fuga de un único bit por operación permite reconstruir información suficiente para recuperar la clave completa.

La combinación de múltiples consultas controladas, el conocimiento de la S-Box de AES y el análisis de las diferencias observadas reduce drásticamente el espacio de búsqueda, haciendo posible recuperar la clave secreta utilizada por el servidor.
