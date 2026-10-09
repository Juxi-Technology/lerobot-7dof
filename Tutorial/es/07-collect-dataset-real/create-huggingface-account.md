[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | Español | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Registrar una cuenta de Hugging Face (opcional)

## Configurar un espejo de HuggingFace para China

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Añade esto al final del archivo
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Salida
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Añade esto al final del archivo
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Salida
# https://hf-mirror.com
```



## Crear un token

https://huggingface.co/settings/tokens

![La imagen muestra la página "Settings / Tokens" del sitio web de Hugging Face. A la izquierda de la página están el avatar y la información personal del usuario; a la derecha está el área de "User Access Tokens", que explica que el token de acceso sirve para la autenticación y permite que las aplicaciones realicen operaciones según los permisos del token, y que insiste en no compartir el token con otras personas. En la esquina superior derecha hay un botón "+ Create new token" para crear un token nuevo. Esta imagen se relaciona con la sección "Crear un token" del documento y presenta visualmente el punto de entrada para crear un token.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Esta imagen muestra la página de creación de una nueva credencial de acceso (token) en la plataforma Hugging Face. En el área "Token type" se destaca la opción "Write", el campo "Token name" ya contiene el texto "lerobot_zihao_token" y en la parte inferior de la página hay un botón "Create token" para enviar la operación de creación; a la izquierda se muestra la información de la cuenta del usuario con sesión iniciada.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![La imagen muestra la ventana emergente "Save your Access Token" del sitio web de Hugging Face. La ventana avisa al usuario de que guarde el valor del token en un lugar seguro, ya que después de cerrarla no se podrá volver a ver. Debajo se muestra el valor del token "<your-huggingface-token>" y un botón "Copy" para copiarlo. Más abajo aparecen el nombre del token "lerobot_zihao_token" y su permiso "WRITE". Esta imagen se relaciona con el paso "Crear un token" del documento y sirve para guiar al usuario a registrar el token generado.](../../en/images/d24-03.png)
</column>
</grid>

## Registrar el token

Por ejemplo, el mío es:

```Shell
<your-huggingface-token>
```

## Vincular el token

```Shell
hf auth login

hf auth whoami
```

![La imagen muestra la interfaz de operación de la cuenta de Hugging Face mediante la línea de comandos en un sistema Mac. Primero se ejecuta el comando "hf auth login", que indica que ya hay un token y que se puede usar "hf auth whoami" para obtener información o "hf auth logout" para cerrar sesión. A continuación se introduce el token y, tras confirmarlo, se muestra que el token se ha guardado en la ruta indicada y que el token activo actual es "lerobot zihao token"; por último se ejecuta el comando "hf auth whoami", que muestra que el usuario es "TommyZihao". Esta imagen corresponde a la sección "Crear un token" del documento y presenta visualmente la operación de creación del token y su resultado.](../../en/images/d24-04.png)

## Crear un repositorio de conjunto de datos

<grid>
<column width-ratio="0.480604">
![Esta es una captura de la interfaz de la plataforma Hugging Face. En la esquina superior izquierda están las entradas a funciones como Community, Docs y Pricing; la información de la cuenta personal TommyZihao aparece en la zona central de la interfaz, y el icono del avatar de usuario de la esquina superior derecha está marcado con un recuadro verde. En el menú central de la interfaz, la opción "+ New Dataset" está destacada con un recuadro rojo; esta opción corresponde al paso "Crear un repositorio de conjunto de datos" del contexto, y al hacer clic en ella se empieza a crear el repositorio de datos.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![La imagen muestra la interfaz de creación de un nuevo repositorio de conjunto de datos en Hugging Face. En "Dataset name" se ha introducido "lerobot_zihao_dataset_a", en "License" se ha seleccionado "mit" y la opción "Public" está marcada. En la parte inferior hay un botón "Create dataset". Esta imagen corresponde a la sección "Crear un repositorio de conjunto de datos" del documento y presenta visualmente la información clave que hay que rellenar al crear un repositorio de conjunto de datos, ayudando al usuario a comprender la interfaz de la operación y los campos necesarios.](../../en/images/d24-06.png)
</column>
</grid>

![Esta es la interfaz web de la plataforma Hugging Face. La página muestra el contenido del repositorio de Dataset llamado lexobot_zihao_dataset_a creado por el usuario TommyZhao, en modo de tema Dark. A la izquierda de la página está la opción de edición "Edit Dataset Card", y también se ofrecen varias formas de subir archivos del conjunto de datos, con ejemplos de comandos para CLI, Python, HTTPS y SSH; además se indica que la License de este conjunto de datos es mit.](../../en/images/d24-07.png)
