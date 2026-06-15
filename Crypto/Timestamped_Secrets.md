# PicoCTF - Timestamped Secrets

## Descripción

Someone encrypted a message using AES in ECB mode but they weren’t very careful with their key.

Se proporciona un archivo con:

```text
Hint: The encryption was done around 1770242597 UTC
Ciphertext (hex): 77c36bef0245021f9d9b7e396b52d2efbdbe6f8e4b79146e5d87c93416453b5f
```

Además se entrega el código fuente utilizado para cifrar.

---

## Análisis

La clave AES se genera a partir del timestamp:

```python
key = sha256(str(timestamp).encode()).digest()[:16]
```

Esto significa que la seguridad depende únicamente del valor del tiempo utilizado durante el cifrado.

El reto proporciona una pista indicando el instante aproximado:

```text
1770242597 UTC
```

Por lo tanto, es posible reconstruir la clave directamente utilizando dicho timestamp.

---

## Script de resolución

```python
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

timestamp = 1770242597

ciphertext = bytes.fromhex(
    "77c36bef0245021f9d9b7e396b52d2efbdbe6f8e4b79146e5d87c93416453b5f"
)

key = sha256(str(timestamp).encode()).digest()[:16]

cipher = AES.new(key, AES.MODE_ECB)

plaintext = unpad(cipher.decrypt(ciphertext), AES.block_size)

print(plaintext.decode())
```

---

## Ejecución

```bash
python3 solve.py
```

Salida:

```text
picoCTF{sa3S_sEc9t_194672d0}
```

---

## Flag

```text
picoCTF{sa3S_sEc9t_194672d0}
```

---

## Aprendizaje

- Uso de AES en modo ECB.
- Generación de claves mediante SHA-256.
- Riesgos de derivar claves criptográficas a partir de información predecible.
- Importancia de utilizar fuentes de entropía seguras.
- Ataques basados en conocimiento parcial del tiempo de generación de la clave.
