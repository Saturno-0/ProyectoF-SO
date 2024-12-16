# Cuestionario de Sistemas de Archivos

## Tarea 1

### 1. Define el concepto de archivo real y archivo virtual.
- **Archivo Real**: Es un archivo que existe físicamente en el sistema de archivos del sistema operativo. Estos archivos están ubicados en el disco duro o cualquier otro almacenamiento físico.
- **Archivo Virtual**: No existe como un archivo tradicional, pero se comporta como tal dentro del sistema operativo. Un archivo virtual puede ser, por ejemplo, una representación de datos que están en memoria o un enlace simbólico a un archivo real.

### 2. Proporciona ejemplos de cómo los sistemas operativos manejan archivos reales y virtuales.
- **Archivos Reales**: Los sistemas operativos manejan archivos reales como archivos de texto, imágenes, documentos, que pueden ser leídos y modificados por el usuario o el sistema operativo. 
  - Ejemplo: Un documento `.txt` almacenado en el disco.
  
- **Archivos Virtuales**: Los archivos virtuales a menudo se manejan a través de enlaces simbólicos o en sistemas de archivos en memoria como `/tmp` en Unix, que no tiene una existencia física permanente en el disco.
  - Ejemplo: Un enlace simbólico en Unix que redirige a otro archivo físico.

### 3. Explica un caso práctico donde un archivo virtual sea más útil que un archivo real.
- En situaciones donde los datos son temporales y no se requiere almacenamiento permanente, como en aplicaciones de cache o en sistemas de procesamiento de datos en tiempo real, los archivos virtuales son más útiles ya que permiten una gestión eficiente de la memoria sin necesidad de acceder al almacenamiento físico.


## Tarea 2

### 1. Identifica los componentes clave de un sistema de archivos (por ejemplo, metadatos, tablas de asignación, etc.).
- **Metadatos**: Información sobre el archivo (como su nombre, tipo, fecha de creación, etc.).
- **Tablas de Asignación**: Estructuras que rastrean los bloques en disco que están asignados a un archivo.
- **Directorios**: Estructuras jerárquicas que contienen archivos y otros directorios.
- **Superbloques**: Almacenan información crítica sobre el sistema de archivos, como su tamaño, tipo y estado.

### 2. Crea un cuadro comparativo de cómo estos componentes funcionan en sistemas como EXT4 y NTFS.

| Componente                | EXT4                         | NTFS                         |
|---------------------------|------------------------------|------------------------------|
| **Metadatos**              | Inodos que contienen atributos del archivo. | MFT (Master File Table), donde cada archivo tiene una entrada. |
| **Tablas de Asignación**   | Bitmaps para gestionar bloques libres y asignados. | Índices B+ para rastrear bloques de datos. |
| **Directorios**            | Estructuras de directorios tradicionales en forma de listas. | Directorios organizados como árboles B+ dentro del MFT. |
| **Superbloques**           | Contienen información como el tamaño y estado del sistema de archivos. | Utiliza metadatos almacenados dentro del MFT. |

### 3. Describe las ventajas y desventajas de cada sistema basado en sus componentes.
- **EXT4**: 
  - **Ventajas**: Alta velocidad de acceso, buen manejo de grandes volúmenes de datos.
  - **Desventajas**: Puede ser menos eficiente en sistemas con alta fragmentación.
  
- **NTFS**: 
  - **Ventajas**: Soporta archivos de gran tamaño, permisos avanzados, y compresión de archivos.
  - **Desventajas**: Más pesado y complejo, especialmente en sistemas no Windows.

---

## Tarea 3

### 1. Diseña un árbol jerárquico que represente la organización lógica de directorios y subdirectorios.
```markdown
/home/
├── user/
│   ├── Documents/
│   │   └── project1/
│   └── Downloads/
├── system/
│   └── config/
└── backups/
```

### 2. Explica cómo se traduce la dirección lógica a la dirección física en el disco.
- **Dirección Lógica**: Es la dirección del archivo que el sistema operativo usa para buscar y acceder al archivo (por ejemplo, `/home/user/Documents/project1.txt`).
- **Dirección Física**: Es la dirección real en el almacenamiento donde se encuentran los datos del archivo, determinada por el sistema de archivos y la asignación de bloques.

### 3. Proporciona un ejemplo práctico de cómo un archivo se almacena físicamente.
- Un archivo de texto `project1.txt` puede estar distribuido en varios bloques en un disco. El sistema de archivos mantiene una tabla que indica en qué bloques se encuentra cada parte del archivo.

---

## Tarea 4

### 1. Define los diferentes mecanismos de acceso.
- **Acceso Secuencial**: Lee o escribe datos de manera consecutiva en el archivo.
- **Acceso Directo**: Permite acceder a cualquier parte del archivo sin seguir un orden específico.
- **Acceso por Índice**: Usa una tabla de índices que direcciona a las posiciones físicas donde están los datos.

### 2. Escribe un pseudocódigo que muestre cómo acceder a:
#### 2.1 Un archivo secuencialmente.
```pseudo
Abrir archivo
Leer linea del archivo
Si no hay más líneas
    Cerrar archivo
```

#### 2.2 Un archivo directamente mediante su posición.
```pseudo
Abrir archivo
Ir a posición deseada
Leer/Escribir dato
Cerrar archivo
```

#### 2.3 Un archivo utilizando un índice.
```pseudo
Abrir archivo
Buscar índice para el dato
Ir a la posición usando el índice
Leer/Escribir dato
Cerrar archivo
```

### 3. Compara las ventajas de cada mecanismo dependiendo del caso de uso.
- **Acceso Secuencial**: Mejor cuando los datos deben ser procesados en un orden determinado (por ejemplo, leer logs).
- **Acceso Directo**: Ideal para archivos grandes donde solo se necesita acceder a posiciones específicas.
- **Acceso por Índice**: Útil cuando se necesita un acceso rápido a datos dispersos en el archivo.

---

## Tarea 5

### 1. Diseña un modelo jerárquico para un sistema de archivos con al menos tres niveles de directorios.

```markdown
/root/
├── bin/
│   ├── system/
│   └── admin/
└── user/
    ├── docs/
    └── images/
```

### 2. Simula una falla en un directorio específico y describe los pasos necesarios para recuperarlo.
- **Escenario**: El directorio `/user/docs/` se borra accidentalmente.
  1. Verifica si hay un backup reciente de ese directorio.
  2. Si se tiene un backup, restaura el directorio desde el último backup.
  3. Si no hay backup, utiliza herramientas de recuperación de archivos, como `extundelete` para EXT4 o `Recuva` para NTFS.

### 3. Explica qué herramientas o técnicas de respaldo (backup) utilizarías para evitar pérdida de datos.
- **Herramientas**:
  - **rsync**: Utilizado para respaldos incrementales en sistemas basados en Unix.
  - **Windows Backup**: Para sistemas Windows.
  - **Cloud Storage**: Utilizar servicios como Google Drive o Dropbox para almacenamiento adicional.
  
- **Técnicas**: 
  - **Backup Incremental**: Solo guarda los cambios desde el último respaldo.
  - **Backup Completo**: Respalda todos los datos desde cero.
  - **RAID**: Utilizar un sistema de almacenamiento redundante para evitar la pérdida de datos debido a fallos de hardware.

# Proteccion y Seguridad

## Ejercicio 1
### Pregunta 1
- **Define los conceptos de protección y seguridad en el contexto de sistemas operativos.**
- **Identifica los objetivos principales de un sistema de protección y seguridad, como confidencialidad, integridad y disponibilidad.**
- **Da un ejemplo práctico de cómo se aplican estos objetivos en un sistema operativo.**

**Definición de conceptos:**
- **Protección:** En un sistema operativo, se refiere a la prevención del acceso no autorizado a recursos del sistema como memoria, archivos, y procesos.
- **Seguridad:** Garantiza que los datos y recursos sean accesibles solo por los usuarios autorizados y estén protegidos contra el mal uso, la alteración o la destrucción.

**Objetivos principales de un sistema de protección y seguridad:**
- **Confidencialidad:** Asegura que la información solo sea accesible para las personas o sistemas autorizados.
- **Integridad:** Garantiza que los datos no sean modificados de forma no autorizada, preservando su exactitud y fiabilidad.
- **Disponibilidad:** Asegura que los recursos del sistema estén disponibles cuando se necesiten y no sean interrumpidos.

**Ejemplo práctico:**
En un sistema operativo como Windows, la **confidencialidad** se asegura mediante contraseñas de usuario, la **integridad** mediante los permisos de acceso a los archivos, y la **disponibilidad** a través de backups y recuperación de sistemas.

---

## Ejercicio 2
### Pregunta 2
- **Investiga las clasificaciones comunes de la seguridad, como física, lógica y de red.**
- **Explica el papel de cada clasificación en la protección de un sistema operativo.**
- **Proporciona ejemplos prácticos de herramientas o técnicas utilizadas en cada clasificación.**

**Clasificaciones comunes de la seguridad:**
1. **Seguridad física:** Protege los componentes físicos del sistema, como servidores, estaciones de trabajo y redes.
2. **Seguridad lógica:** Protege el sistema de amenazas relacionadas con el acceso a través de cuentas de usuario, contraseñas, o vulnerabilidades del software.
3. **Seguridad de red:** Protege la información mientras viaja por la red, asegurando que no sea interceptada ni manipulada por actores no autorizados.

**El papel de cada clasificación:**
- **Seguridad física** impide el acceso físico no autorizado a los sistemas de hardware.
- **Seguridad lógica** se centra en prevenir el acceso no autorizado o abuso de las cuentas y los recursos del sistema.
- **Seguridad de red** protege los datos en tránsito, por ejemplo, mediante cifrado.

**Ejemplos prácticos:**
- **Seguridad física:** Cerraduras y control de acceso a los servidores.
- **Seguridad lógica:** Cortafuegos, contraseñas fuertes, y gestión de identidad.
- **Seguridad de red:** Uso de VPNs y cifrado SSL/TLS.

---

## Ejercicio 3
### Pregunta 3
- **Describe cómo un sistema de protección controla el acceso a los recursos.**
- **Explica las funciones principales como autenticación, autorización y auditoría.**
- **Diseña un caso práctico donde se muestren las funciones de un sistema de protección en acción.**

**Control de acceso a los recursos:**
Un sistema de protección gestiona quién puede acceder a qué recursos en un sistema operativo, como memoria o archivos. Utiliza políticas y mecanismos de autenticación y autorización para decidir los permisos de los usuarios.

**Funciones principales:**
1. **Autenticación:** Verificación de la identidad del usuario.
2. **Autorización:** Determinación de qué recursos puede acceder el usuario.
3. **Auditoría:** Seguimiento de las acciones realizadas por los usuarios.

**Caso práctico:**
Un usuario se autentica mediante una contraseña. Luego, el sistema verifica que el usuario tiene permiso para acceder a un archivo específico y registra el acceso en un log para auditoría.

---

## Ejercicio 4
### Pregunta 4
- **Diseña una matriz de acceso para un sistema con al menos 3 usuarios y 4 recursos.**
- **Explica cómo esta matriz se utiliza para controlar el acceso en un sistema operativo.**
- **Simula un escenario donde un usuario intenta acceder a un recurso no permitido y cómo la matriz lo bloquea.**

**Matriz de acceso (3 usuarios y 4 recursos):**

| **Recurso/Usuario** | Usuario 1 | Usuario 2 | Usuario 3 |
|---------------------|-----------|-----------|-----------|
| Recurso 1           | Acceso    | No acceso | No acceso |
| Recurso 2           | No acceso | Acceso    | No acceso |
| Recurso 3           | No acceso | No acceso | Acceso    |
| Recurso 4           | Acceso    | Acceso    | No acceso |

**Uso de la matriz en el control de acceso:**
La matriz de acceso se utiliza para definir qué usuarios pueden interactuar con qué recursos. Si un usuario intenta acceder a un recurso para el que no tiene permiso (como el usuario 3 intentando acceder al Recurso 1), el sistema lo bloquea.

---

## Ejercicio 5
### Pregunta 5
- **Explica el concepto de protección basada en el lenguaje.**
- **Proporciona un ejemplo de cómo un lenguaje como Java o Rust asegura la memoria y evita accesos no autorizados.**
- **Compara este enfoque con otros mecanismos de protección en sistemas operativos.**

**Protección basada en el lenguaje:**
La protección basada en el lenguaje emplea características del lenguaje de programación para asegurar que las operaciones se realicen de manera segura, evitando accesos no autorizados a la memoria.

**Ejemplo práctico con Java:**
Java utiliza la máquina virtual Java (JVM) para gestionar la memoria y evitar desbordamientos de búfer, asegurando que no se acceda a áreas de memoria no autorizadas.

**Comparación con otros mecanismos de protección:**
En comparación con los mecanismos de protección en sistemas operativos (como el uso de permisos de archivos y el aislamiento de procesos), la protección basada en el lenguaje ofrece una seguridad más enfocada en las aplicaciones y la ejecución de código.

---

## Ejercicio 6
### Pregunta 6
- **Investiga y describe al menos tres tipos de amenazas comunes (por ejemplo, malware, ataques de fuerza bruta, inyección de código).**
- **Explica los mecanismos de validación como autenticación multifactor y control de integridad.**
- **Diseña un esquema de validación para un sistema operativo con múltiples usuarios.**

**Tipos de amenazas comunes:**
1. **Malware:** Programas maliciosos que pueden infectar el sistema (virus, troyanos).
2. **Ataques de fuerza bruta:** Intentos de adivinar contraseñas mediante prueba y error.
3. **Inyección de código:** Inserción de código malicioso en un sistema que lo ejecuta, como en ataques SQL injection.

**Mecanismos de validación:**
- **Autenticación multifactor:** Uso de múltiples factores (contraseña, autenticación biométrica).
- **Control de integridad:** Uso de sumas de verificación para asegurarse de que los archivos no hayan sido alterados.

**Esquema de validación:**
1. El usuario introduce su contraseña.
2. El sistema verifica la identidad del usuario mediante una contraseña y una segunda autenticación (por ejemplo, SMS).
3. El sistema valida la integridad del archivo mediante un algoritmo de suma de verificación.

---

## Ejercicio 7
### Pregunta 7
- **Define los conceptos de cifrado simétrico y asimétrico.**
- **Proporciona un ejemplo práctico de cada tipo de cifrado aplicado en sistemas operativos.**
- **Simula el proceso de cifrado y descifrado de un archivo con una clave dada.**

**Cifrado simétrico y asimétrico:**
- **Cifrado simétrico:** Usa la misma clave para cifrar y descifrar los datos (por ejemplo, AES).
- **Cifrado asimétrico:** Usa dos claves: una pública para cifrar y una privada para descifrar (por ejemplo, RSA).

**Ejemplos prácticos:**
- **Cifrado simétrico:** Usado en discos duros cifrados con AES.
- **Cifrado asimétrico:** Usado en la comunicación segura HTTPS con RSA.

**Simulación de cifrado y descifrado (AES):**
```plaintext
Cifrado: Texto claro -> Clave AES -> Texto cifrado
Descifrado: Texto cifrado -> Clave AES -> Texto claro
```