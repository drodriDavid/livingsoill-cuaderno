# Cuaderno LivingSoiLL · UJA

Cuaderno de trabajo del **visor de fincas experimentales de LivingSoiLL**: las dieciséis
fincas con su analítica, su erosión y sus actuaciones, el estado del visor, el modelo de
datos, el runbook y lo que queda por hacer.

No es el visor. El visor está en
[LIVINGSOILL_FINCAS_EXPERIMENTALES](https://github.com/drodriDavid/LIVINGSOILL_FINCAS_EXPERIMENTALES);
esto es la herramienta para coordinarlo.

Ocho vistas: resumen, panel de situación, las dieciséis fincas, tareas, notas, bitácora,
el modelo de datos y el runbook.

## Cómo se entra

Se entra **con cuenta de Google**. No hay contraseña que escribir.

El repositorio es público porque GitHub Pages lo exige, así que `index.html` no contiene el
documento en claro: contiene el documento cifrado con **AES-256-GCM**, con la clave derivada
mediante **PBKDF2-HMAC-SHA256** (400.000 iteraciones). Sin la clave, lo que hay aquí es ruido.

La clave no la teclea nadie: vive en un archivo `clave-cuaderno.txt` dentro de la carpeta de
Drive **Cuaderno LivingSoiLL (UJA)**. Al pulsar «Entrar con Google», la portada pide acceso a
Drive, busca esa carpeta, lee la clave y descifra la página en el propio navegador.

Así que el control de acceso es el de Google: **compartir esa carpeta de Drive es dar acceso
al cuaderno, y dejar de compartirla es quitarlo.** Quien no la tenga compartida no lee nada y
no se le crea nada en su Drive.

Recargar no vuelve a pedir Google: la clave con la que se entró se queda en la pestaña y
muere al cerrarla.

## Sincronización

El cuaderno se conecta solo al entrar. Lo que se escribe se guarda en
`cuaderno-livingsoill-uja.json`, en esa misma carpeta de Drive, a los pocos segundos de cada
cambio. Al abrir y al volver a la pestaña, comprueba si alguien ha escrito y trae lo nuevo.

Usa el mismo ID de cliente OAuth que el cuaderno de REGEN4ANDALUCIA, porque el origen
autorizado (`https://drodridavid.github.io`) cubre los dos.

## De dónde salen los datos de las fincas

De los objetos embebidos en el `index.html` del visor: `FINCAS`, `SUELO`, `RUSLE`,
`SOLUCIONES`, `CARACTERISTICAS`, `IOT` y `PUNTOS`. Se extraen con un script y se vuelcan en
`fincas.js`, que `montar.py` inserta en el cuaderno. No se transcribe nada a mano.

Si el visor cambia sus datos, hay que volver a extraerlos y regenerar `fincas.js`.

## Cómo se construye

El documento en claro se arma a partir de tres piezas, porque entero pasa de 120 KB:

    _p1.html    estilo y cabecera
    _p2.html    las ocho vistas
    _p3.html    el script
    fincas.js   los datos de las dieciséis fincas

    python montar.py          # las junta en fuente.html e incrusta el logo
    python build.py fuente.html --pass "$(cat clave.txt)"

`build.py` cifra `fuente.html` y lo envuelve en la portada `puerta.html`, produciendo
`index.html`, que es lo único que se publica. `fuente.html` y `clave.txt` están ignorados
por git.

Antes de cifrar hay que validar el JavaScript de `fuente.html` y de `puerta.html`: extraer su
`<script>` y pasarle `node --check`.

## Publicar cambios

**PowerShell** (la consola por defecto de Windows):

    cd C:/ProyectosUJA/livingsoill-cuaderno
    .\publicar.ps1

**Git Bash / WSL:**

    cd C:/ProyectosUJA/livingsoill-cuaderno && ./publicar.sh

La clave se lee de `clave.txt`. Para cambiarla, pásala como argumento; si lo haces, **borra
`clave-cuaderno.txt` de la carpeta de Drive** y entra una vez para dejar la nueva.

En **Windows PowerShell 5.1** el operador `&&` no existe; encadena con `;`. Y usa barras
normales en las rutas.

## Identidad

Del logo del proyecto: el brote verde (`#137A38`), el perfil de suelo en verde oscuro
(`#0A3D2B`) y los terrones marrones (`#7A5E46`). Test en verde y control en rojo, como en el
visor. La escala de erosión sigue las clases RUSLE: baja, moderada, alta y muy alta. Todos
los pares de color verificados sobre WCAG AA en tema claro y oscuro.
