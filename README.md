# moodle-usuarios-masivos

<p>
  <img alt="Python" src="https://img.shields.io/badge/python-3.8%2B-blue?logo=python&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-green">
  <img alt="Status" src="https://img.shields.io/badge/status-stable-green">
</p>

> Genera un CSV listo para importar usuarios en Moodle a partir de una lista simple con nombre, apellido, DNI/CUIL y email. Valida formato, normaliza mayúsculas y reporta errores.

## Features

- Lee un CSV fuente con formato propio (`nombre;apellido;nro_doc;mail`) y produce el CSV con el esquema que Moodle espera.
- Valida:
  - Formato del email.
  - DNI/CUIT/CUIL: solo dígitos, entre 7 y 11 caracteres.
- Normaliza nombres y apellidos con capitalización consistente.
- Asigna `username`, `password` inicial y `sysrole1` según config.
- Loggea en `errores.csv` las filas inválidas (para corregir y re-procesar).

## Requirements

- Python 3.8+
- Sin dependencias externas (stdlib suficiente).

## Quickstart

### Install

```bash
git clone https://github.com/GDelpo/moodle-usuarios-masivos.git
cd moodle-usuarios-masivos
```

### Prepare fuente

Colocar un archivo `fuente/fuente.csv` con este formato (delimitador `;`):

```csv
nombre;apellido;nro_doc;mail
Juan;Pérez;20123456789;juan.perez@example.com
María;Gómez;27987654321;maria.gomez@example.com
```

### Run

```bash
python main.py
```

Outputs:
- `usuarios_moodle.csv` — CSV con formato Moodle (lastname, firstname, email, username, password, sysrole1).
- `errores.csv` — filas descartadas por validación fallida.

## Formato de salida Moodle

| Campo | Valor |
|-------|-------|
| `lastname` | Apellido normalizado |
| `firstname` | Nombre normalizado |
| `email` | Email validado |
| `username` | Generado desde `nombre.apellido` |
| `password` | Password inicial configurable |
| `sysrole1` | Rol de sistema asignado |

## License

[MIT](LICENSE) © 2026 Guido Delponte
