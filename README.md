# Script de Generación de Usuarios para Moodle

## Repositorio

```bash
git clone http://192.168.190.95/forgejo/noble/moodle-usuarios-masivos.git
git pull origin main   # actualizar
```

> Primera vez en una máquina nueva: ver [SETUP.md](http://192.168.190.95/forgejo/noble/workspace/raw/branch/main/SETUP.md) para configurar proxy y credenciales Git.

---


Este script permite generar un archivo CSV listo para importar usuarios en Moodle, a partir de un archivo fuente (`fuente.csv`) con datos básicos de personas (nombre, apellido, correo, documento).

El archivo de salida cumple con la estructura requerida por Moodle, incluyendo asignación de rol y contraseña inicial.

## 📄 Estructura Esperada del Archivo Fuente

El archivo de entrada debe ser un archivo CSV delimitado por `;` (punto y coma), con encabezado, y tener la siguiente estructura:

```
nombre;apellido;nro\_doc;mail
Juan;Pérez;20123456789;[juan.perez@email.com](mailto:juan.perez@email.com)

````

> El archivo debe estar ubicado en la carpeta `fuente/` con el nombre `fuente.csv`.

## ⚙️ ¿Qué hace el script?

1. Lee el archivo `fuente/fuente.csv`.
2. Valida:
   - Que el correo tenga un formato válido.
   - Que el documento (DNI o CUIT/CUIL) tenga entre 7 y 11 dígitos numéricos.
3. Limpia los nombres y apellidos (los formatea con mayúscula inicial).
4. Genera un archivo `usuarios_moodle.csv` con los campos que Moodle requiere:
   - `lastname`, `firstname`, `email`, `username`, `password`, `sysrole1`
5. Registra en `errores.csv` las filas inválidas (correos o documentos mal formateados).

## 🛠️ Requisitos

Solo se necesita **Python 3.x**. No se requieren bibliotecas externas.

## ▶️ Cómo usarlo

1. Colocá el archivo `fuente.csv` dentro del subdirectorio `fuente/`.
2. Ejecutá el script:

 ```bash
   python main.py
````

3. El script generará dos archivos en el mismo directorio:

   * `usuarios_moodle.csv`: usuarios válidos listos para importar en Moodle.
   * `errores.csv`: usuarios descartados por errores en correo o documento.

## 🧩 Personalización

En el archivo `main.py`, podés modificar:

* `ROL`: rol a asignar en Moodle (por ejemplo, `student`, `editingteacher`, etc.).
* `NOMBRE_INSTITUCION` y `CURSO`: no utilizados por defecto, pero listos para expansión.
* `con_institucion`: si lo pasás a `True`, agrega el nombre de la institución como primer campo (`lastname`).

## 💡 Notas

* Si tenés un archivo fuente separado por comas en lugar de `;`, deberás ajustar el parámetro `delimiter` en el código.
* Todos los usuarios tendrán como `username` y `password` su número de documento, sin guiones ni espacios. Sino ajustar el código.
* El script no maneja la creación de grupos ni categorías, pero es fácil de extender.

## ✏ TODOs

- Crear una interfaz gráfica para facilitar la carga de archivos.
- Agregar más validaciones (por ejemplo, longitud de nombres y apellidos).
- Revisar la posibilidad de agregar más campos requeridos por Moodle.
