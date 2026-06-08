# PicoCTF - Mini RSA

## Descripción

Se proporcionan los siguientes parámetros RSA:

e = 3

N = número entero muy grande

c = ciphertext

El objetivo consiste en recuperar el mensaje original cifrado.

---

## Análisis

En RSA el cifrado se realiza mediante:

c = M^e mod N

En este reto el exponente público es:

e = 3

Cuando se utiliza un exponente tan pequeño, puede ocurrir que:

M^3 < N

Si esto sucede, la operación módulo no altera el resultado y se cumple:

c = M^3

Por lo tanto, para recuperar el mensaje basta con calcular la raíz cúbica exacta del ciphertext.

---

## Script de resolución

c = 5709720175026317841944505166332182779419760031115432215563425901875620052791608163398750297304277886495537367793006288112991932717211066611899319963701838062828209254458935983141207100322897221021063116398621664612418813778554568264340430225832578491114653771121708580262264085968492732626559392615882541267670045570974444907542776294075571558085100930760760722416252378077610954367547852126659348372505030936460878940046808104816042562380177143194070773417030106158693436848897733795588156153439353374354378946281556140400056185498147808737766921652179963585037964078150136578302230878368862263000059181416416783313808950923297557514158129515879758935765781055731969330005473022370060498393105532201494698762244481915406880607127300210095672274400200661881861043562650754163743246330438392531433701024389754537178000821754581449881408420282753987132115940027280372196160429387747427183641264436940827271631231510105131701780747726012636466088768725514820344357055715523621362143648027363044170885031058505704

def iroot3(n):
lo, hi = 0, n

```
while lo <= hi:
    mid = (lo + hi) // 2

    if mid ** 3 == n:
        return mid

    if mid ** 3 < n:
        lo = mid + 1
    else:
        hi = mid - 1

return hi
```

m = iroot3(c)

flag = m.to_bytes((m.bit_length() + 7) // 8, "big")

print(flag.decode())

---

## Resultado

Al ejecutar el script:

python3 RSA_solution.py

se obtiene:

picoCTF{e_sh0u1d_b3_lArg3r_92f4d5a5}

---

## Flag

picoCTF{e_sh0u1d_b3_lArg3r_92f4d5a5}

---

## Aprendizaje

* Riesgos de utilizar exponentes RSA pequeños.
* Importancia del padding adecuado.
* Cómo aprovechar el caso donde M^e < N.
* Recuperación de mensajes mediante raíces enteras exactas.
* Conversión de enteros grandes a texto usando bytes.
