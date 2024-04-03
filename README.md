## 🤖 Este repositorio está diseñado para automatizar el proceso de inyección de variables de entorno y variables de repositorio. Utiliza flujos de trabajo de GitHub Actions para realizar las tareas.

### 📁 Estructura de archivos
- `.github/workflows/var_env.yml`: Este flujo de trabajo lee variables de entorno de `env.json` e inyecta en el entorno del repositorio de GitHub especificado.
- `.github/workflows/var_rep_auto.yml`: ste flujo de trabajo lee las variables de `var_rep.json` e inyecta en las acciones del repositorio de GitHub.
- `env.json`: Este archivo contiene las variables de entorno que serán inyectadas por `var_env.yml`.
- `var_rep.json`: Este archivo contiene las variables que serán inyectadas por `var_rep_auto.yml`.
- `repositories.json`: Este archivo contiene la lista de repositorios donde se inyectarán las variables de entorno.

### 🔄 Flujos de trabajo
#### `var_env.yml`
Este flujo de trabajo lee de `env.json` e inyecta variables de entorno en el entorno del repositorio de GitHub especificado. Utiliza `jq` para analizar el archivo JSON y extraer las variables de entorno. Luego utiliza `curl` para hacer una solicitud PUT a la API de GitHub y actualizar el entorno con las variables extraídas.

#### `var_rep_auto.yml`
Este flujo de trabajo lee variables de `var_rep.json` e inyecta en las acciones del repositorio de GitHub. Utiliza `jq` para analizar el archivo JSON y extraer las variables. Luego utiliza `curl` para hacer una solicitud POST a la API de GitHub y crear una nueva variable en las acciones del repositorio.

### 📄 Archivos JSON
#### `env.json`
Este archivo contiene las variables de entorno que serán inyectadas por `var_env.yml`. Cada objeto en el array debe tener un campo `name_env` (el nombre de la variable de entorno), un campo `body_parameters` (los parámetros para el entorno) y un campo `variables` (un array de variables que serán inyectadas).

#### `var_rep.json`
Este archivo contiene las variables que serán inyectadas por `var_rep_auto.yml`. Cada objeto en el array debe tener un campo `name` (el nombre de la variable) y un campo `value` (el valor de la variable).

## Variables APPNAME Y PROJECTKEY
Este código es parte de un script de GitHub Actions. Verifica si el valor del campo "name" es igual a "APPNAME" o "PROJECTKEY". Si es así, establece el valor del elemento actual en la "matriz" al valor de la variable correspondiente. Utiliza el comando jq para procesar y modificar los objetos JSON.

```bash
# Verifica si el valor del campo "name" es igual a "APPNAME"
if [ "$name" == "APPNAME" ]; then
    # Si es igual, establece el valor en "matrix"
    current_element=$(jq --arg project_name "$rep_name" '.value = $project_name' <<< "$current_element")
fi

# Verifica si el valor del campo "name" es igual a "PROJECTKEY"
if [ "$name" == "PROJECTKEY" ]; then
    # Si es igual, establece el valor en "matrix"
    current_element=$(jq --arg project_name "$project_name" '.value = $project_name' <<< "$current_element")
fi

## Consideraciones de seguridad

🔒 Ten en cuenta que debes tener el secreto `GH_PAT` configurado en tu repositorio con un token que tenga los permisos necesarios para agregar equipos a los repositorios. Además, el archivo `repositories.json` debe estar correctamente formateado y actualizado con los repositorios a los que deseas agregar grupos.

## 🛠️ Configuración 🛠️

El archivo `repositories.json` puede contener múltiples repositorios y debe tener el siguiente formato:

```json
  {
      "owner" : "BancoBice",
  
      "repositories": [
        {
          "repo_name": "AR-plugin-nodejs-oag",
          "repo_name": "AR-plugin-angular-oag"
        }
      ]
  }
