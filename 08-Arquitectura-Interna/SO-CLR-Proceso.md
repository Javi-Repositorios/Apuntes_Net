# Arquitectura interna: Sistema Operativo, CLR y proceso

Diagrama de los componentes y su comunicacion, desde el sistema operativo hasta el proceso principal de la aplicacion.

![[SO-CLR-Proceso.png]]

## Definición

El modelo de ejecucion de .NET Framework en Windows consta de cuatro niveles: el sistema operativo, la capa de hospedaje, el Common Language Runtime y el proceso de la aplicacion. El codigo administrado nunca se ejecuta directamente sobre el hardware; lo hace a traves del CLR, que a su vez delega en el sistema operativo.

## Componentes

| Nivel | Componente | Funcion |
|---|---|---|
| 1. Sistema operativo | Kernel | Gestion de procesos, hilos y sincronizacion |
| 1. Sistema operativo | Gestor de memoria | Heap nativo y paginacion |
| 1. Sistema operativo | Subsistema de E/S | Ficheros, red y dispositivos |
| 1. Sistema operativo | Planificador | Asignacion de CPU |
| 1. Sistema operativo | Win32 API | Interfaz nativa accesible por P/Invoke |
| 2. Capa de hospedaje | `mscoree.dll` | Shim / PE Loader que localiza el runtime |
| 2. Capa de hospedaje | CLR Host | Arranca el runtime dentro del proceso |
| 2. Capa de hospedaje | AppDomain | Aislamiento y contexto de ejecucion |
| 3. CLR | Class Loader | Carga ensamblados y metadatos |
| 3. CLR | JIT Compiler | Traduce IL a codigo nativo |
| 3. CLR | Execution Engine | Ejecuta y gestiona hilos |
| 3. CLR | Garbage Collector | Recolecta el heap administrado |
| 3. CLR | CTS / CLS | Sistema de tipos comun |
| 3. CLR | Base Class Library | Tipos base de .NET (FCL) |
| 3. CLR | Seguridad | Validacion y acceso a codigo |
| 3. CLR | Marshal / Interop | Puente con codigo nativo |
| 4. Aplicacion | App.exe | Proceso principal en ejecucion |
| 4. Aplicacion | `Main()` | Punto de entrada del programa |
| 4. Aplicacion | Codigo CIL / MSIL | Instrucciones compiladas por el JIT |
| 4. Aplicacion | Ensamblado | IL + metadatos + recursos |

## Comunicacion entre niveles

1. **Sistema operativo → Capa de hospedaje**: el cargador de Windows (PE Loader) resuelve `mscoree.dll`, que actua como shim para localizar e iniciar la version del CLR.
2. **Capa de hospedaje → CLR**: el CLR Host arranca el runtime en un `AppDomain` y carga el ensamblado principal.
3. **CLR → Aplicacion**: el Class Loader carga los ensamblados, el JIT compila el IL a codigo nativo y el Execution Engine ejecuta `Main()`.
4. **Aplicacion → Sistema operativo**: el codigo administrado accede a memoria, ficheros, red y hilos a traves del CLR; el acceso directo a la API nativa se realiza mediante P/Invoke o Marshal.

## Casos de Uso

- Comprender por que el codigo administrado requiere el runtime instalado.
- Diagnosticar errores de carga de ensamblados o de version del CLR.
- Justificar el coste de arranque (*JIT*) y la recoleccion de basura.
- Ubicar el uso de P/Invoke para interoperar con la API Win32.

## Ejemplo de Código Mínimo

```vb
Imports System
Imports System.Diagnostics

Module Program
    <STAThread>
    Sub Main()
        Dim rutaProceso As String = Process.GetCurrentProcess().MainModule.FileName
        Console.WriteLine("Proceso: " & rutaProceso)
        Console.WriteLine("CLR: " & Environment.Version.ToString())
        Console.WriteLine("SO: " & Environment.OSVersion.ToString())
    End Sub
End Module
```
