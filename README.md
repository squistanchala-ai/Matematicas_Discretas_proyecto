# Matematicas_Discretas_proyecto
Síntesis topológica automática de mecanismos planos usando teoría de grafos
Este proyecto implementa un procedimiento computacional para generar, validar y clasificar topologías de mecanismos planos a partir del número de eslabones y los grados de libertad deseados.

El programa representa cada mecanismo como un grafo simple, donde:

los vértices representan eslabones;

las aristas representan juntas entre eslabones;

el grado de cada vértice indica cuántas juntas posee el eslabón.

El desarrollo se encuentra en el notebook DiscretasProyecto.ipynb.

Objetivo

Generar automáticamente las posibles topologías de un mecanismo plano que satisfacen:

la ecuación de movilidad;

la secuencia de grados de los eslabones;

las restricciones de conectividad;

la consistencia entre juntas, puertos y matrices;

la ausencia de subcadenas estructuralmente bloqueadas;

la eliminación de soluciones topológicamente equivalentes mediante isomorfismos.

Como resultado, el programa obtiene las topologías únicas y genera sus matrices de adyacencia, incidencia, grados y laplaciana.

Configuración inicial

Los parámetros principales se encuentran al inicio del Bloque 1:

n = 8       # Número total de eslabones
GDL = 1     # Grados de libertad

Para mecanismos planos con pares inferiores, el número de juntas se calcula mediante:

GDL = 3(n - 1) - 2J

Por tanto:

J = [3(n - 1) - GDL] / 2

El programa verifica que el valor obtenido para J sea entero.

Estructura del notebook

El notebook contiene cinco bloques de código. La numeración actual salta del Bloque 4 al Bloque 6; no existe una celda independiente denominada Bloque 5.

Bloque 1 — Generación de surtidos de eslabones

Determina las posibles distribuciones de grados de los eslabones.

Resuelve mediante backtracking las ecuaciones diofánticas:

Σ x_g = n
Σ g·x_g = 2J

donde x_g representa la cantidad de eslabones de grado g.

Para cada surtido válido:

crea los eslabones;

asigna un identificador a cada eslabón;

crea los puertos disponibles;

asigna identificadores locales y globales a los puertos.

La salida principal de este bloque es:

surtidos_encontrados

Bloque 2 — Generación de grafos candidatos

Construye los grafos simples etiquetados que cumplen la secuencia de grados de cada surtido.

Utiliza:

backtracking;

poda de ramas inviables;

el criterio de Erdős-Gallai;

matrices de adyacencia;

asignación de puertos a las juntas.

Parámetros configurables:

MAX_CANDIDATOS_POR_SURTIDO = None
LIMITE_GRAFOS_A_MOSTRAR = 10

None indica que se intentarán generar todos los candidatos. Para pruebas rápidas puede utilizarse un límite entero.

Este bloque suele ser el de mayor costo computacional, ya que el número de grafos posibles crece rápidamente con el número de eslabones.

Bloque 3 — Validación de restricciones topológicas

Revisa cada grafo candidato y descarta las configuraciones que no cumplen las condiciones del mecanismo.

Entre las validaciones realizadas se encuentran:

propiedades de la matriz de adyacencia;

conectividad del grafo;

número correcto de juntas;

correspondencia entre aristas y matriz;

cumplimiento de los grados objetivo;

consistencia de juntas y puertos;

detección de subcadenas bloqueadas;

cálculo de movilidad de subcadenas.

Parámetros configurables:

LIMITE_VALIDOS_A_MOSTRAR = 10
GUARDAR_GRAFOS_RECHAZADOS = True

Cuando GUARDAR_GRAFOS_RECHAZADOS es True, se conserva información sobre los candidatos descartados y las razones de rechazo.

Bloque 4 — Eliminación de isomorfismos

Agrupa los grafos válidos que representan la misma topología.

Para ello utiliza NetworkX y compara los grafos teniendo en cuenta el grado de sus nodos. De cada clase de isomorfismo se conserva un único representante.

También:

calcula firmas topológicas básicas;

obtiene mapeos entre grafos isomorfos;

verifica que los representantes finales no sean isomorfos entre sí.

Parámetros configurables:

LIMITE_TOPOLOGIAS_A_MOSTRAR = 10
GUARDAR_MAPEOS_ISOMORFOS = True

Bloque 6 — Matrices finales y visualización

Procesa cada topología única y genera:

matriz de adyacencia;

matriz de incidencia;

matriz diagonal de grados;

matriz laplaciana;

etiquetas para eslabones y juntas;

dibujo del grafo correspondiente.

La matriz laplaciana se calcula como:

L = D - A

donde D es la matriz de grados y A es la matriz de adyacencia.

Parámetros configurables:

LIMITE_TOPOLOGIAS_A_MOSTRAR = None
SEMILLA_DIBUJO = 42
MOSTRAR_GRAFICOS = True

El programa intenta utilizar una distribución planar cuando el grafo lo permite. En caso contrario, utiliza una distribución automática basada en fuerzas.

Flujo general

Parámetros del mecanismo
          │
          ▼
Generación de surtidos de eslabones
          │
          ▼
Generación de grafos candidatos
          │
          ▼
Validación de restricciones topológicas
          │
          ▼
Eliminación de grafos isomorfos
          │
          ▼
Matrices finales y visualización

Requisitos

Python 3.10 o superior

Jupyter Notebook o JupyterLab

NetworkX

Matplotlib

Las demás herramientas utilizadas, como copy e itertools, pertenecen a la biblioteca estándar de Python.

Instalación

Clone el repositorio:

git clone <URL_DEL_REPOSITORIO>
cd <NOMBRE_DEL_REPOSITORIO>

Cree y active un entorno virtual, de forma opcional pero recomendada:

python -m venv .venv

En Windows:

.venv\Scripts\activate

En Linux o macOS:

source .venv/bin/activate

Instale las dependencias:

pip install jupyter networkx matplotlib

Ejecución

Inicie Jupyter:

jupyter notebook

Abra el archivo:

DiscretasProyecto.ipynb

Ejecute las celdas en orden, desde el Bloque 1 hasta el Bloque 6.

Los bloques dependen de variables y resultados creados anteriormente, por lo que no deben ejecutarse de forma aislada ni en un orden diferente.

También puede ejecutar todo el notebook desde el menú:

Kernel → Restart & Run All

Ajustes para pruebas rápidas

La generación exhaustiva puede tardar considerablemente. Para probar el programa con menor tiempo de ejecución se recomienda:

utilizar un número pequeño de eslabones;

limitar los candidatos generados por surtido;

reducir la cantidad de resultados impresos;

desactivar temporalmente el almacenamiento de rechazados;

desactivar las visualizaciones cuando no sean necesarias.

Ejemplo:

MAX_CANDIDATOS_POR_SURTIDO = 100
LIMITE_GRAFOS_A_MOSTRAR = 5
GUARDAR_GRAFOS_RECHAZADOS = False
MOSTRAR_GRAFICOS = False

Limitar los candidatos sirve para realizar pruebas, pero impide garantizar que se hayan obtenido todas las topologías posibles.

Resultados

Durante la ejecución, el notebook muestra:

número de surtidos encontrados;

secuencia de grados de cada surtido;

cantidad de grafos candidatos;

cantidad de grafos válidos y rechazados;

causas de rechazo;

cantidad de clases isomórficas;

topologías únicas;

matrices asociadas a cada topología;

representación gráfica de los mecanismos.

Complejidad computacional

El problema es combinatorio. El número de conexiones posibles aumenta rápidamente al incrementar n, por lo que una ejecución exhaustiva puede requerir bastante tiempo y memoria.

Los principales costos se encuentran en:

la generación de grafos mediante backtracking;

la validación de todos los candidatos;

la comparación de isomorfismos.

Las podas y las firmas topológicas reducen el espacio de búsqueda, pero no eliminan el crecimiento combinatorio del problema.

Estructura sugerida del repositorio

.
├── DiscretasProyecto.ipynb
├── README.md
├── requirements.txt
└── .gitignore

Un archivo requirements.txt mínimo puede contener:

jupyter
matplotlib
networkx

Un .gitignore básico para Python y Jupyter puede contener:

.venv/
__pycache__/
.ipynb_checkpoints/
*.pyc

Tecnologías utilizadas

Python

Jupyter Notebook

NetworkX

Matplotlib

teoría de grafos;

backtracking;

ecuaciones diofánticas;

criterio de Erdős-Gallai;

isomorfismo de grafos.

Posibles mejoras

corregir la numeración interna de los bloques;

separar las funciones en módulos .py;

agregar pruebas unitarias;

exportar automáticamente las matrices a archivos;

guardar las imágenes de las topologías;

añadir mediciones de tiempo por bloque;

implementar procesamiento paralelo;

mejorar las podas del generador;

añadir una interfaz para modificar los parámetros.

Autor

Sebastián Alexander Quistanchala Vallejo

Proyecto desarrollado en el contexto de la asignatura de Matemáticas Discretas.
