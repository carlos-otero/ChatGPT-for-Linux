# 🐧 ChatGPT for Linux (Cliente con Nativefier)

Esta guía te muestra cómo crear una aplicación de escritorio de ChatGPT para Linux (distribuciones basadas en Debian/Ubuntu) usando [Nativefier](https://github.com/nativefier/nativefier).

Nativefier envuelve cualquier sitio web (en este caso, `chat.openai.com`) en una aplicación de Electron, dándote un cliente de escritorio simple y aislado de tu navegador principal.

## 1. Preparar el Entorno (Node.js con NVM)

En lugar de usar `apt`, usaremos `nvm` (Node Version Manager) para instalar `node` y `npm`. Esto evita problemas de permisos y nos permite instalar la versión más reciente y compatible.

```bash
# 1. Descarga e instala nvm (Node Version Manager)
curl -o- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash

# 2. Activa nvm en tu sesión actual
# (Tal vez necesites cerrar y reabrir tu terminal después de este paso)
export NVM_DIR="$HOME/.nvm"
source "$NVM_DIR/nvm.sh"

# 3. Instala, usa y pon por defecto Node.js v18 (una versión LTS estable)
nvm install 18
nvm use 18
nvm alias default 18

# 4. Confirma las versiones
echo "Node version:"
node -v  # Debería ser v18.x.x
echo "NPM version:"
npm -v   # Debería ser 9.x.x o 10.x.x
````

## 2\. Instalar Nativefier

Ahora que tienes `npm`, puedes instalar `nativefier` de forma global en tu sistema:

```bash
npm install -g nativefier
```

## 3\. Crear la Aplicación ChatGPT

1.  Decide dónde quieres que se "instale" la aplicación. Un buen lugar es una carpeta `Apps` en tu directorio `home`.
2.  Navega a ese directorio y ejecuta el comando de `nativefier`.

<!-- end list -->

```bash
# Crea la carpeta si no existe y entra en ella
mkdir -p ~/Apps
cd ~/Apps

# Ejecuta nativefier con opciones recomendadas
nativefier --name "ChatGPT" \
           --single-instance \
           --internal-urls ".*chat.openai.com.*" \
           "[https://chat.openai.com](https://chat.openai.com)"
```

**Explicación de las opciones:**

  * `--name "ChatGPT"`: El nombre de la aplicación.
  * `--single-instance`: Evita que abras múltiples ventanas de la app por accidente.
  * `--internal-urls ".*chat.openai.com.*"`: Fuerza a que cualquier enlace *fuera* de ChatGPT (p.ej., un enlace a Google) se abra en tu navegador web normal, manteniendo la app limpia.

Nativefier trabajará por unos segundos y creará una nueva carpeta llamada `ChatGPT-linux-x64` en el directorio `~/Apps`.

## 4\. (Opcional) Crear un Acceso Directo en el Menú

Para que "ChatGPT" aparezca en tu menú de aplicaciones (junto a Firefox, VSCode, etc.), necesitas crear un archivo `.desktop`.

1.  Abre un editor de texto (como `nano`) para crear el archivo del lanzador:

    ```bash
    nano ~/.local/share/applications/chatgpt.desktop
    ```

2.  **Pega el siguiente contenido** dentro de `nano`.

    **¡IMPORTANTE\!** Debes reemplazar `TU_USUARIO` con tu nombre de usuario real (el que ves en tu terminal, ej. `/home/tunombredeusuario`).

    ```ini
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=ChatGPT
    Comment=Cliente de escritorio para ChatGPT (creado con Nativefier)

    # ¡¡CAMBIA ESTA RUTA!!
    # Reemplaza 'TU_USUARIO' por tu nombre de usuario
    Exec=/home/TU_USUARIO/Apps/ChatGPT-linux-x64/ChatGPT

    # Esta ruta al ícono debería ser la correcta
    # Reemplaza 'TU_USUARIO' por tu nombre de usuario
    Icon=/home/TU_USUARIO/Apps/ChatGPT-linux-x64/resources/app/icon.png

    Terminal=false
    Categories=Utility;Network;Application;
    ```

3.  Guarda el archivo y cierra `nano` (Ctrl+O, Enter, Ctrl+X).

4.  Finalmente, dale permisos de ejecución al lanzador:

    ```bash
    chmod +x ~/.local/share/applications/chatgpt.desktop
    ```

-----

¡Y eso es todo\! Ahora deberías poder buscar "ChatGPT" en tu menú de aplicaciones e iniciarlo como cualquier otro programa.

```
```
