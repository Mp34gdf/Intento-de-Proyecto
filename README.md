#algoritmo
#var

#fun
def leer_archivo(nombre_archivo):
    calificaciones = {}
    with open(nombre_archivo, 'r') as archivo:
        for linea in archivo:
            estudiante, asignatura, calificacion = linea.strip().split('|')
            if estudiante not in calificaciones:
                calificaciones[estudiante] = {}
            calificaciones[estudiante][asignatura] = float(calificacion)
    return calificaciones

def calcular_promedios(calificaciones):
    promedios_estudiantes = {}
    suma_general = 0
    contador_general = 0
    for estudiante, notas in calificaciones.items():
        promedio = sum(notas.values()) / len(notas)
        promedios_estudiantes[estudiante] = promedio
        suma_general += sum(notas.values())
        contador_general += len(notas)
    promedio_general = suma_general / contador_general
    return promedios_estudiantes, promedio_general

def asignatura_extremos(calificaciones):
    promedios_asignaturas = {}
    for notas in calificaciones.values():
        for asignatura, calificacion in notas.items():
            if asignatura not in promedios_asignaturas:
                promedios_asignaturas[asignatura] = []
            promedios_asignaturas[asignatura].append(calificacion)
    asignatura_max = max(promedios_asignaturas, key=lambda k: sum(promedios_asignaturas[k]) / len(promedios_asignaturas[k]))
    asignatura_min = min(promedios_asignaturas, key=lambda k: sum(promedios_asignaturas[k]) / len(promedios_asignaturas[k]))
    return asignatura_max, asignatura_min

def ordenamiento_burbuja(diccionario):
    estudiantes = list(diccionario.items())
    n = len(estudiantes)
    for i in range(n):
        for j in range(0, n-i-1):
            if estudiantes[j][1] < estudiantes[j+1][1]:
                estudiantes[j], estudiantes[j+1] ñ= estudiantes[j+1], estudiantes[j]
    return estudiantes

def exportar_resultados(calificaciones, promedios_estudiantes, asignatura_max, asignatura_min):
    estudiantes_ordenados = ordenamiento_burbuja(promedios_estudiantes)
    with open('resultados.txt', 'w') as archivo:
        for estudiante, promedio in estudiantes_ordenados:
            archivo.write(f"{estudiante}|{promedio:.2f}|{asignatura_max}|{asignatura_min}\n")

#inicio
nombre_archivo = 'calificaciones.txt'
calificaciones = leer_archivo(nombre_archivo)
promedios_estudiantes, promedio_general = calcular_promedios(calificaciones)
asignatura_max, asignatura_min = asignatura_extremos(calificaciones)
exportar_resultados(calificaciones, promedios_estudiantes, asignatura_max, asignatura_min)
