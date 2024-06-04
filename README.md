# Trabajo-final-prueba-
import pandas as pd
import random
import os

# Cargar los datos desde los enlaces proporcionados
dataframe_nombres = pd.read_csv('https://docs.google.com/spreadsheets/d/e/2PACX-1vTzQVXz05E83OWUjhCbWejh-g_pmIEbLdQu0AgbtHOUwCrgKWIxEgovR13RVp00aiIPUFjScIFsrk49/pub?gid=1465750411&single=true&output=csv')
dataframe_apellidos = pd.read_csv('https://docs.google.com/spreadsheets/d/e/2PACX-1vRKqL2dP6uYcR8S6f6Ma1zKS3mtFUnhl8gozxokx1hUlQ77L-DuzDkc1uVnzFjYzBFPRqAGF2hLf0Ug/pub?gid=756598162&single=true&output=csv')

# Crear lista de 1000 estudiantes únicos
estudiantes = set()
while len(estudiantes) < 1000:
    nombre = random.choice(dataframe_nombres['Nombre'])
    apellido = random.choice(dataframe_apellidos['Apellidos'])
    estudiante = f"{nombre} {apellido}"
    estudiantes.add(estudiante)

# Convertir a lista y crear DataFrame incluyendo el semestre
datos_estudiantes = pd.DataFrame(estudiantes, columns=['Nombre Completo'])
datos_estudiantes['Semestre'] = ''

# Asignar apellidos y semestres de manera personalizada
semestres = {
    1: 0.14, 2: 0.13, 3: 0.12, 4: 0.11, 5: 0.10,
    6: 0.10, 7: 0.09, 8: 0.08, 9: 0.07, 10: 0.06
}

# Generar semestres aleatorios proporcionalmente
total_sectores = sum(semestres.values())
semestres_aleatorios = []
for _ in range(len(datos_estudiantes)):
    random_number = random.random()
    cumulative_sum = 0
    for semestre, probability in semestres.items():
        cumulative_sum += probability
        if random_number < cumulative_sum / total_sectores:
            semestres_aleatorios.append(semestre)
            break

datos_estudiantes['Semestre'] = semestres_aleatorios

# Guardar en un archivo CSV
datos_estudiantes.to_csv('estudiantes.csv', index=False)

# Malla curricular
malla_curricular = [
    ('Álgebra y Trigonometría', 1, 'Ciencias Básicas', 3),
    ('Cálculo Diferencial', 1, 'Ciencias Básicas', 3),
    ('Geometría Vectorial y Analítica', 1, 'Ciencias Básicas', 3),
    ('Vivamos la Universidad', 1, 'Ciencias Básicas', 1),
    ('Inglés I', 1, 'Formación Complementaria', 1),
    ('Lectoescritura', 1, 'Formación Complementaria', 3),
    ('Introducción a la Ingeniería Industrial', 1, 'Generales', 1),
    ('Gestión de las Organizaciones', 2, 'Administración y Finanzas', 3),
    ('Habilidades Gerenciales', 2, 'Administración y Finanzas', 3),
    ('Álgebra Lineal', 2, 'Ciencias Básicas', 3),
    ('Cálculo Integral', 2, 'Ciencias Básicas', 3),
    ('Descubriendo la Física', 2, 'Ciencias Básicas', 3),
    ('Inglés II', 2, 'Formación Complementaria', 1),
    ('Gestión Contable', 3, 'Administración y Finanzas', 3),
    ('Física Mecánica', 3, 'Ciencias Básicas', 3),
    ('Inglés III', 3, 'Formación Complementaria', 1),
    ('Algoritmia y Programación', 3, 'Métodos Cuantitativos', 3),
    ('Probabilidad e Inferencia Estadística', 3, 'Métodos Cuantitativos', 3),
    ('Teoría General de Sistemas', 3, 'Métodos Cuantitativos', 3),
    ('Ingeniería Económica', 4, 'Administración y Finanzas', 3),
    ('Electiva en Física', 4, 'Ciencias Básicas', 3),
    ('Inglés IV', 4, 'Formación Complementaria', 1),
    ('Diseño de Experimentos y Análisis de Regresión', 4, 'Métodos Cuantitativos', 3),
    ('Optimización', 4, 'Métodos Cuantitativos', 3),
    ('Gestión de Métodos y Tiempos', 4, 'Producción, Logística y Calidad', 4),
    ('Gestión Financiera', 5, 'Administración y Finanzas', 3),
    ('Laboratorio Integrado de Física', 5, 'Ciencias Básicas', 1),
    ('Inglés V', 5, 'Formación Complementaria', 1),
    ('Formación Ciudadana y Constitucional', 5, 'Formación Complementaria', 1),
    ('Dinámica de Sistemas', 5, 'Métodos Cuantitativos', 3),
    ('Muestreo y Series de Tiempo', 5, 'Métodos Cuantitativos', 3),
    ('Procesos Estocásticos y Análisis de Decisión', 5, 'Métodos Cuantitativos', 3),
    ('Gestión por Procesos', 5, 'Producción, Logística y Calidad', 3),
    ('Gestión Tecnológica', 6, 'Administración y Finanzas', 3),
    ('Legislación', 6, 'Administración y Finanzas', 3),
    ('Electiva en Humanidades I', 6, 'Electivas Socio Humanísticas', 3),
    ('Inglés VI', 6, 'Formación Complementaria', 1),
    ('Simulación Discreta', 6, 'Métodos Cuantitativos', 3),
    ('Formulación de Proyectos de Investigación', 6, 'Generales', 3),
    ('Normalización y Control de la Calidad', 6, 'Producción, Logística y Calidad', 3),
    ('Formulación y Evaluación de Proyectos de Inversión', 7, 'Administración y Finanzas', 3),
    ('Emprendimiento', 7, 'Administración y Finanzas', 2),
    ('Electiva en Humanidades II', 7, 'Electivas Socio Humanísticas', 3),
    ('Énfasis Profesional I', 7, 'Énfasis Profesional', 3),
    ('Electiva Complementaria I', 7, 'Formación Complementaria', 3),
    ('Diseño de Sistemas Productivos', 7, 'Producción, Logística y Calidad', 3),
    ('Gestión de Proyectos', 8, 'Administración y Finanzas', 3),
    ('Electiva en Humanidades III', 8, 'Electivas Socio Humanísticas', 3),
    ('Énfasis Profesional I', 8, 'Énfasis Profesional', 3),
    ('Electiva Complementaria I', 8, 'Formación Complementaria', 3),
    ('Administración de la Producción y del Servicio', 8, 'Producción, Logística y Calidad', 3),
    ('Electiva en Humanidades IV', 9, 'Electivas Socio Humanísticas', 3),
    ('Énfasis Profesional I', 9, 'Énfasis Profesional', 3),
    ('Electiva Complementaria I', 9, 'Formación Complementaria', 3),
    ('Gestión de la Cadena de Abastecimiento', 9, 'Producción, Logística y Calidad', 3),
    ('Ingeniería del Mejoramiento Continuo', 9, 'Producción, Logística y Calidad', 3),
    ('Práctica Profesional', 10, 'Generales', 12)
]

# Función para generar código de asignatura
def generar_codigo_asignatura(nombre, nivel, creditos, indice):
    partes = nombre.split()
    codigo = ''.join([p[0:3].upper() for p in partes])
    return f"{codigo}-{nivel}-{creditos:02d}-{indice:02d}"

# Crear DataFrame para asignaturas y cursos
asignaturas_cursos = []

for indice, (asignatura, nivel, nucleo, creditos) in enumerate(malla_curricular, start=1):
    codigo = generar_codigo_asignatura(asignatura, nivel, creditos, indice)
    htd = 64 if creditos == 3 else (96 if creditos == 4 else (32 if creditos == 2 else 16))
    hti = 80 if creditos == 3 else (120 if creditos == 4 else (64 if creditos == 2 else 32))
    nte = int(semestres[nivel] * 1000)
    tca = (nte // 30) + (1 if nte % 30 != 0 else 0)
    asignaturas_cursos.append([codigo, nivel, asignatura, creditos, htd, hti, nte, tca])

# Crear el DataFrame organizado
df_asignaturas_cursos = pd.DataFrame(asignaturas_cursos, columns=['Código', 'Nivel', 'Asignatura', 'Créditos', 'HTD', 'HTI', 'NTE', 'TCA'])

# Guardar el DataFrame en formato CSV
df_asignaturas_cursos.to_csv('asignaturas_cursos.csv', index=False)

# Guardar el DataFrame en formato Excel
df_asignaturas_cursos.to_excel('asignaturas_cursos.xlsx', index=False)

# Crear una lista de nombres de estudiantes
lista_estudiantes = datos_estudiantes['Nombre Completo'].tolist()

# Crear un diccionario para asignar nombres a cada asignatura
asignacion_estudiantes = {codigo: [] for codigo in df_asignaturas_cursos['Código']}

# Parámetro para la cantidad máxima de personas por grupo
max_personas_por_grupo = 30

# Asignar nombres a cada asignatura y dividirlos en grupos
for index, row in df_asignaturas_cursos.iterrows():
    codigo = row['Código']
    nte = row['NTE']

    # Ajustar NTE si es mayor que el número de estudiantes disponibles
    nte = min(nte, len(lista_estudiantes))

    # Asignar estudiantes y actualizar la lista de estudiantes disponibles
    estudiantes_asignados = random.sample(lista_estudiantes, nte)
    lista_estudiantes = [est for est in lista_estudiantes if est not in estudiantes_asignados]

    # Dividir estudiantes en grupos
    grupos = [estudiantes_asignados[i:i + max_personas_por_grupo] for i in range(0, len(estudiantes_asignados), max_personas_por_grupo)]
    asignacion_estudiantes[codigo] = grupos

# Crear la estructura de carpetas y asignar estudiantes a cursos
base_path = 'Ruta Trabajo Final'
os.makedirs(base_path, exist_ok=True)

for index, row in df_asignaturas_cursos.iterrows():
    curso = row['Asignatura']
    semestre = row['Nivel']
    codigo = row['Código']
    grupos_curso = asignacion_estudiantes[codigo]

    semestre_path = os.path.join(base_path, f'Semestre {semestre}')
    os.makedirs(semestre_path, exist_ok=True)

    curso_path = os.path.join(semestre_path, curso)
    os.makedirs(curso_path, exist_ok=True)

    # Guardar los grupos en archivos
    for i, grupo in enumerate(grupos_curso, start=1):
        grupo_path = os.path.join(curso_path, f'Grupo_{i}')
        os.makedirs(grupo_path, exist_ok=True)

        # Crear un DataFrame con la información del grupo
        info_grupo = {
            'Nombre Asignatura': curso,
            'Semestre': semestre,
            'Total Estudiantes': len(grupo)
        }
        df_info_grupo = pd.DataFrame([info_grupo])

        # Crear DataFrame de la lista de estudiantes
        df_lista_estudiantes = pd.DataFrame(grupo, columns=['Nombre Completo'])

        # Guardar en archivo Excel
        archivo_excel = os.path.join(grupo_path, f'{curso}_Grupo_{i}.xlsx')
        with pd.ExcelWriter(archivo_excel) as writer:
            df_info_grupo.to_excel(writer, sheet_name='Información del Grupo', index=False)
            df_lista_estudiantes.to_excel(writer, sheet_name='Lista de Estudiantes', index=False)

        # Guardar en archivo CSV
        archivo_csv_info = os.path.join(grupo_path, f'{curso}_Grupo_{i}_info.csv')
        archivo_csv_lista = os.path.join(grupo_path, f'{curso}_Grupo_{i}_lista.csv')
        df_info_grupo.to_csv(archivo_csv_info, index=False)
        df_lista_estudiantes.to_csv(archivo_csv_lista, index=False)

# Verificar estructura de carpetas y archivos
for root, dirs, files in os.walk(base_path):
    level = root.replace(base_path, '').count(os.sep)
    indent = ' ' * 4 * (level)
    print(f'{indent}{os.path.basename(root)}/')
    subindent = ' ' * 4 * (level + 1)
    for f in files:
        print(f'{subindent}{f}')
