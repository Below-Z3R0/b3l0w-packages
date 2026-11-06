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

```
Fingerprint: 9AE2 C00A D3A9 A647 06A4  335C C6E1 D74A 0783 3042
Clave:       9AE2C00AD3A9A64706A4335CC6E1D74A07833042
Vence:       2031-11-05
UID:         b3l0w packages <paquetes@b3l0w.local>
```

Importar y confiar:

```bash
curl -fsSLO https://below-z3r0.github.io/b3l0w-packages/keys/b3l0w.asc
sudo pacman-key --add b3l0w.asc
sudo pacman-key --lsign-key 9AE2C00AD3A9A64706A4335CC6E1D74A07833042
```

Verificá el fingerprint contra una fuente independiente antes de
firmar la clave. Es lo único que impide que un repo comprometido
reemplace la clave junto con los paquetes.

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
