# PicoCTF - Caesar

## Descripción

Se proporciona la siguiente bandera cifrada mediante un cifrado César:

```text
picoCTF{furvvlqjwkhuxelfrqslandktj}
```

## Análisis

El cifrado César desplaza cada letra un número fijo de posiciones en el alfabeto.

Probando con un desplazamiento de **3 posiciones hacia atrás**, se obtiene:

```text
furvvlqjwkhuxelfrqslandktj
↓
crossingtherubiconpixkahqg
```

La frase resultante contiene la expresión conocida:

```text
crossing the rubicon
```

## Solución

Aplicando un desplazamiento de 3 a cada carácter:

```text
furvvlqjwkhuxelfrqslandktj
→
crossingtherubiconpixkahqg
```

## Flag

```text
picoCTF{crossingtherubiconpixkahqg}
```

## Aprendizaje

* Identificación de un cifrado César.
* Uso de desplazamientos sobre el alfabeto.
* Reconocimiento de texto legible para validar el resultado.
* Herramienta útil para retos básicos de criptografía en CTFs.
