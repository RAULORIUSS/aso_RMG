# Documentación del Proyecto: Dominio para Centro Educativo

## 1. Introducción

Este proyecto consiste en la implementación de un dominio en un entorno de Active Directory (AD) para un centro educativo que imparte ciclos formativos de la familia de Informática y Comunicaciones. El objetivo es organizar y gestionar de manera eficiente los recursos, usuarios (alumnos y profesores), y políticas de seguridad, garantizando un entorno seguro y funcional.

## 2. Estructura del Dominio

### 2.1. Unidades Organizativas (OU)

La estructura de las Unidades Organizativas (OU) se ha diseñado para reflejar la organización del centro educativo y facilitar la administración de usuarios y recursos.

#### 2.1.1. Estructura Propuesta

- **OU Principal:** CentroEducativo  
  - **OU CiclosFormativos**  
    - **OU ASIR**  
      - **OU PrimerCurso**  
      - **OU SegundoCurso**  
    - **OU SMR**  
      - **OU PrimerCurso**  
      - **OU SegundoCurso**  
    - **OU DAM**  
      - **OU PrimerCurso**  
      - **OU SegundoCurso**  
    - **OU DAW**  
      - **OU PrimerCurso**  
      - **OU SegundoCurso**  
  - **OU Profesores**  
  - **OU Aulas**  
    - **OU Aula1**  
    - **OU Aula2**  
    - ...  
    - **OU AulaN**  

#### 2.1.2. Justificación

- **Separación por Ciclos y Cursos:** Permite una gestión granular de los usuarios y políticas, aplicando configuraciones específicas para cada ciclo y curso.
- **OU Profesores:** Los profesores tienen necesidades diferentes a los alumnos (acceso a herramientas administrativas, permisos elevados, etc.), por lo que se les asigna una OU separada.
- **OU Aulas:** Cada aula tiene sus propios equipos y recursos, organizados en OUs independientes para facilitar la gestión de permisos y políticas.

### 2.2. Grupos

Los grupos se utilizan para gestionar permisos y políticas de manera eficiente.

#### 2.2.1. Grupos Propuestos

**Grupos de Alumnos:**
- ASIR_PrimerCurso
- ASIR_SegundoCurso
- SMR_PrimerCurso
- SMR_SegundoCurso
- DAM_PrimerCurso
- DAM_SegundoCurso
- DAW_PrimerCurso
- DAW_SegundoCurso

**Grupos de Profesores:**
- Profesores

**Grupos de Aulas:**
- Aula1
- Aula2
- ...
- AulaN

#### 2.2.2. Justificación

- **Grupos de Alumnos:** Cada grupo representa un curso, lo que facilita la asignación de permisos a carpetas compartidas y la aplicación de políticas específicas.
- **Grupo de Profesores:** Los profesores necesitan permisos elevados y acceso a herramientas administrativas, por lo que se agrupan en un único grupo.
- **Grupos de Aulas:** Cada aula tiene sus propios equipos, y los permisos de acceso se gestionan a través de estos grupos.

## 3. Automatización del Alta de Alumnos

### 3.1. Uso de Import-Csv

Para automatizar el alta de alumnos, se utiliza el comando `Import-Csv` en PowerShell. Este comando lee los datos de los alumnos desde un fichero CSV y crea las cuentas de usuario en el dominio..

#### 3.1.1. Estructura del Fichero CSV

El fichero CSV debe tener las siguientes columnas:

- **NombreUsuario** (e.g., alumno1)
- **NombreCompleto** (e.g., Juan Pérez)
- **Curso** (e.g., ASIR_PrimerCurso)

#### 3.1.2. Script de Automatización

```powershell
# Importar el fichero CSV
$alumnos = Import-Csv -Path "C:\ruta\alumnos.csv"

# Recorrer cada alumno y crear la cuenta de usuario
foreach ($alumno in $alumnos) {
    $nombreUsuario = $alumno.NombreUsuario
    $nombreCompleto = $alumno.NombreCompleto
    $curso = $alumno.Curso

    # Crear la cuenta de usuario
    New-ADUser -Name $nombreCompleto `
               -SamAccountName $nombreUsuario `
               -UserPrincipalName "$nombreUsuario@centroeducativo.local" `
               -Path "OU=$curso,OU=CiclosFormativos,DC=centroeducativo,DC=local" `
               -AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) `
               -Enabled $true

    # Añadir el usuario al grupo correspondiente
    Add-ADGroupMember -Identity $curso -Members $nombreUsuario
}
```

### 3.1.3. Justificación

- **Automatización:** El script permite crear cuentas de usuario de manera rápida y sin errores, ahorrando tiempo en la administración.
- **Flexibilidad:** El uso de un fichero CSV permite añadir o modificar alumnos fácilmente.

## 4. Automatización de la Creación de Carpetas Personales
### 4.1. Carpetas Personales
Cada alumno debe tener una carpeta personal en el servidor, con permisos de acceso exclusivo para el usuario.
### 4.1.1. Script de Automatización
```powershell
foreach ($alumno in $alumnos) {
    $nombreUsuario = $alumno.NombreUsuario
    $rutaCarpeta = "C:\CarpetasPersonales\$nombreUsuario"

    # Crear la carpeta personal
    New-Item -Path $rutaCarpeta -ItemType Directory

    # Asignar permisos a la carpeta
    $acl = Get-Acl -Path $rutaCarpeta
    $permisos = "$nombreUsuario", "FullControl", "Allow"
    $regla = New-Object System.Security.AccessControl.FileSystemAccessRule($permisos)
    $acl.SetAccessRule($regla)
    Set-Acl -Path $rutaCarpeta -AclObject $acl
}
```
### 4.1.2. Justificación
•	**Privacidad:** Cada alumno tiene acceso exclusivo a su carpeta personal, garantizando la privacidad de sus datos.

•	**Automatización:** El script crea las carpetas y asigna permisos de manera automática.

## 5. Carpetas Compartidas por Grupo
### 5.1. Carpetas Compartidas
Cada grupo de alumnos tiene una carpeta compartida para facilitar la colaboración.
### 5.1.1. Script de Automatización

```powershell
$cursos = @("ASIR_PrimerCurso", "ASIR_SegundoCurso", "SMR_PrimerCurso", "SMR_SegundoCurso", "DAM_PrimerCurso", "DAM_SegundoCurso", "DAW_PrimerCurso", "DAW_SegundoCurso")

foreach ($curso in $cursos) {
    $rutaCarpeta = "C:\CarpetasCompartidas\$curso"

    # Crear la carpeta compartida
    New-Item -Path $rutaCarpeta -ItemType Directory

    # Compartir la carpeta
    New-SmbShare -Name $curso -Path $rutaCarpeta -FullAccess "$curso"

    # Asignar permisos NTFS
    $acl = Get-Acl -Path $rutaCarpeta
    $permisos = "$curso", "Modify", "Allow"
    $regla = New-Object System.Security.AccessControl.FileSystemAccessRule($permisos)
    $acl.SetAccessRule($regla)
    Set-Acl -Path $rutaCarpeta -AclObject $acl
}
```
### 5.1.2. Justificación
•	**Colaboración:** Las carpetas compartidas permiten a los alumnos trabajar en equipo y compartir recursos.

•	**Permisos:** Cada grupo tiene permisos de modificación sobre su carpeta compartida.
## 6. Políticas de Seguridad

### 6.1.1 Restricción de CMD y PowerShell
•	**Alumnos:** Se prohíbe el acceso a CMD y PowerShell para evitar el uso indebido de estas herramientas.

•	**Profesores:** Se permite el acceso a CMD y PowerShell para tareas administrativas.
### 6.1.2. Política de Contraseñas
•	Longitud mínima: 8 caracteres.

•	Complejidad: Requerida (mayúsculas, minúsculas, números y caracteres especiales).

•	Caducidad: 90 días.
### 6.1.3. Bloqueo de Cuentas
•	Intentos fallidos: 5 intentos.

•	Duración del bloqueo: 30 minutos.
### 6.1.4. Auditoría
•	Habilitar la auditoría de eventos de inicio de sesión y acceso a recursos críticos.
### 6.1.5. Justificación
•	**Seguridad:** Estas políticas protegen el dominio contra accesos no autorizados y uso indebido de recursos.

•	**Excepciones:** Los profesores tienen permisos elevados para realizar tareas administrativas.
## 7. Conclusión

Este proyecto ha permitido diseñar e implementar un dominio de Active Directory para un centro educativo, garantizando un entorno funcional, seguro y fácil de administrar.

[Volver](../index.md)