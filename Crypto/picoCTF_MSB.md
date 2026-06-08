# PicoCTF - MSB

## Descripción

Se proporciona una imagen llamada:

Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png

La pista del reto indica:

"This image passes LSB statistical analysis, but we can't help but think there must be something to the visual artifacts present in this image..."

Además, el nombre del reto es:

MSB

Lo que sugiere que la información oculta no se encuentra en los bits menos significativos (LSB), sino en los bits más significativos (MSB) de los canales RGB.

---

## Análisis

Una imagen RGB está formada por tres canales:

* R (Red)
* G (Green)
* B (Blue)

Cada canal utiliza 8 bits:

R = b7 b6 b5 b4 b3 b2 b1 b0

Donde:

* b7 = MSB (Most Significant Bit)
* b0 = LSB (Least Significant Bit)

La pista indica que el análisis LSB no produce resultados, por lo que se decidió extraer el MSB de cada canal.

El procedimiento consiste en:

1. Recorrer todos los píxeles.
2. Obtener el MSB de R, G y B.
3. Concatenar todos los bits obtenidos.
4. Agruparlos en bloques de 8 bits.
5. Convertir cada bloque a ASCII.

---

## Script de resolución

from PIL import Image

img = Image.open("Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png")

bits = []

for r, g, b in img.getdata():
bits.append(str((r >> 7) & 1))
bits.append(str((g >> 7) & 1))
bits.append(str((b >> 7) & 1))

bits = ''.join(bits)

texto = []

for i in range(0, len(bits) - 8, 8):
byte = bits[i:i+8]
texto.append(chr(int(byte, 2)))

resultado = ''.join(texto)

with open("output.txt", "w", encoding="utf-8", errors="ignore") as f:
f.write(resultado)

print("Listo")

---

## Resultado

Al ejecutar el script:

python3 msb.py

se genera el archivo:

output.txt

Dentro del texto recuperado aparecen fragmentos de Don Quijote y posteriormente la flag:

picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_ea7deb4c}

---

## Flag

picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_ea7deb4c}

---

## Aprendizaje

* Diferencia entre LSB y MSB.
* Ocultamiento de información dentro de imágenes RGB.
* Extracción de bits significativos.
* Reconstrucción de texto ASCII a partir de secuencias binarias.
* Uso de Python y Pillow para análisis forense de imágenes.
