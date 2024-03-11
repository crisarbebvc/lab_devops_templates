# Lab-devops-templates
Templates, scripts y workflows reusables para la implementación de flujos de integración y despliegue continuos y la automatización de trabajos en proyectos Nuam.

## Nomenclatura de templates de workflows
Todos los archivos correspondientes a workflows reusables se encuentran dentro de la ruta `.github/workflows`.

Estos archivos poseen un nombre el cual coincide con la sigueinte estructra: {**TYPE**}-{**WORK_NAME**}-{**LANGUAGE**}.

> [!CAUTION]
> Si el nombre del trabjo posee mas de una palabra, estas estarán separadas por medio de guiones bajos, ya que los guiones normales están reservados para la estructura de la nomenclatura.

| Parámetro        | Descripción | Valores permitidos |
| :-------------: |-------------| -------|
| `TYPE`  | Indica a que parte del flujo DevOps pertence el fujo | `CI`, `CD` |
| `WORK_NAME`  | Nombre breve y explicito para el flujo | Definidos por arquitectura |
| `LANGUAGE`  | Indica el lenguaje de programación en el que funciona el flujo | `java`, `javascript`, `go`, `python` |

## Workflows reusables
Los workflows reusables son archivos YML disponibles para usar por medio de Github Actions listos para integrar por medio de la práctica [Reusable Workflows - GitHub Actions](https://docs.github.com/en/actions/using-workflows/reusing-workflows#access-to-reusable-workflows) que ejecutan diferentes procesos, facilitando la ejecución de prácticas DeOps dentro de Nuam.

La principal función de los workflows reusables es evitar replicar código a través de los diferentes repositorios, en cambio solo bastará una simple implementación para contar con los procesos requeridos, ya que estos worklows están diseñados de forma estandar y cumpliendo con estandares de calidad y seguridad para ejecutar de forma exitosa los procesos de CI/CD.

### Flujos reusables de integración continua
Reutilice de forma fácil y sencilla los workflows diseñados en este repositorio mientras ajusta su proyecto a los lineamientos de DevOps definidos por la organización para procesos de integración continua (CI).

**Pruebas unitarias**

Los flujos de pruebas unitarias fueron construidos siguiendo el siguiente flujo.

![Flujo de pruebas unitarias](https://github.com/bvcco/lab_devops_templates/blob/main/assets/Unit_Testing_Flowchart.png)

- **Java**
  
  Las pruebas unitarias unitarias para java se encuentran en el archivo ``.github/workflows/CI-unit-testing-java.yml``este workflow esta diseñado para funcionar con múltiples versiones.
  
  - **Versiones disponibles**
    El workflow para pruebas unitarias está definido bajo la versión de [java-setup@4](https://github.com/actions/setup-java) la cual usa notación SemVer. Las versiones disponibles son `8`, `11`, `17`, `21`.
  
  - **Requerimientos**
    Es necesario que el desarrollo cuente con la integración de la dependencia [Jacoco](https://www.eclemma.org/jacoco/) (Java Code Coverage) la cual es una dependencia que permite medir el nivel de cobertura de pruebas unitarias; si esta coniguración no se realiza, la ejecución del workflow de pruebas unitarias siempre tendrá una salida de error por porcentaje de cobertura.

  - **Parámetros**
    - **java-version**: indica la versión de java sobre la que se ejecutarán las pruebas unitarias, asegurese de que sea la misma versión sobre la que fue construida el proyecto.
    - **distribution**: indica la distribucion del JDK a utilizar, por defecto se recomienda usar `temurin`. Sin embargo, tambien se cuenta con la disponibilidad de `corretto` en caso de que el proyecto lo requiera.

### Implementación de los workflows reusables
Para usar los workflows reusables dentro de un repositorio es necesario seguir los siguientes pasos:

1. Cree el directorio ``.github/workflows`` dentro de su repositorio de código.

2. Dentro de directorio anterior cree un archivo .yml con el nombre deseado, desde este ejecutará la llamada a los workflows reusables del presente repositorio.

3. Use el sigueinte código

```
name: {WORKFLOW_NAME}

on:
  pull_request:
    branches:
      - development
      - 'release/*'
      - preprod
      - prod

jobs:
  call-unitestig-workflow:
    uses: bvcco/lab_devops_templates/.github/workflows/{TEMPLATE_NAME}.yml@{TAG_VERSION}
    with:
      {PARAMETER}: {PARAMETER_VALUE}
```

> [!IMPORTANT]  
> Asegurese de modificar los valores pertinenes acorde a la siguiente tabla

| Parámetro        | Descripción |
| :-------------: |-------------|
| `WORKFLOW_NAME`  | Nombre del workflow, este nombre aparecerá en la pestaña "Actions" en el repositorio de código. |
| `TEMPLATE_NAME`  | Nombre del template que se desea usar, puede encontrar todos los nombres de los archivos en la ruta .github/workflows o descritos en esta documentación.   |
| `TAG_VERSION` | Tag publicada de los workflows, consulte los tags de este repositorio para obtener las información o siga los lineamientos dados. |
| `PARAMETER` | Nombre del parametro que se asignará al workflow usado, puede crear la cantidad especifica y requerida por cada workflow. |
| `PARAMETER_VALUE` | Asigna el valor al parametro correspondiente. |

> [!TIP]
> Para encontrar ejemplos de uso de los workflows de integración y despliegue dirigirse al directorio `/examples`.