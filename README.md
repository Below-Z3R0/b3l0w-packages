# b3l0w-packages

Repo pacman `[b3l0w]` de **b3l0wz3r0-os**. Solo binarios: la definición
vive en [`b3l0w`](https://github.com/Below-Z3R0/b3l0w).

## Consumirlo

```ini
# /etc/pacman.conf
[b3l0w]
SigLevel = Required DatabaseOptional
Server = https://below-z3r0.github.io/b3l0w-packages/$arch
```

La clave pública está en [`keys/b3l0w.asc`](keys/b3l0w.asc).

## Estructura

```
x86_64/
  b3l0w.db.tar.gz        base de datos firmada (repo-add)
  b3l0w.files.tar.gz     indice de archivos
  *.pkg.tar.zst          paquetes
  *.pkg.tar.zst.sig      firma de cada paquete
keys/
  b3l0w.asc              clave publica de firma
```

## Como se publica

El CI de cada repo construye el `.pkg.tar.zst` y lo deja en un release.
La firma **no** se hace en CI: la clave privada nunca sale del equipo
del mantenedor.

```bash
# en la maquina del mantenedor
export GNUPGHOME="$HOME/.gnupg-b3l0w"
repo-add --sign x86_64/b3l0w.db.tar.gz x86_64/*.pkg.tar.zst
git add -A && git commit -m "paquetes: <release>" && git push
```
