# Estructura en Capas en VB.NET

## Definición

La arquitectura en capas divide la aplicación en niveles con responsabilidades separadas que se comunican mediante interfaces, reduciendo el acoplamiento y facilitando las pruebas y el mantenimiento.

## Sintaxis / Métodos

### Esquema de capas

```
Presentación (UI)        -> Formularios y enlace de datos
Lógica de Negocio        -> Servicios y reglas
Acceso a Datos (DAL)     -> Repositorios
Persistencia             -> Base de datos / ficheros
```

| Capa | Responsabilidad | Dependencia permitida |
|---|---|---|
| Presentación | Interacción con el usuario | Negocio |
| Negocio | Reglas y validaciones | Abstracciones de datos |
| Datos | Operaciones CRUD | Modelo y motor de datos |

### Contrato de repositorio

```vb
Public Interface IClienteRepositorio
    Function Obtener(id As Integer) As Cliente
    Sub Guardar(cliente As Cliente)
End Interface
```

### Servicio de negocio

```vb
Public Class ClienteServicio
    Private ReadOnly _repositorio As IClienteRepositorio

    Public Sub New(repositorio As IClienteRepositorio)
        _repositorio = repositorio
    End Sub

    Public Sub Registrar(cliente As Cliente)
        If String.IsNullOrWhiteSpace(cliente.Nombre) Then
            Throw New ArgumentException("El nombre es obligatorio")
        End If
        _repositorio.Guardar(cliente)
    End Sub
End Class
```

### Implementación de datos

```vb
Public Class ClienteRepositorioSql
    Implements IClienteRepositorio

    Private ReadOnly _cadenaConexion As String

    Public Sub New(cadenaConexion As String)
        _cadenaConexion = cadenaConexion
    End Sub

    Public Function Obtener(id As Integer) As Cliente Implements IClienteRepositorio.Obtener
        ' Acceso a datos mediante ADO.NET
        Return New Cliente()
    End Function

    Public Sub Guardar(cliente As Cliente) Implements IClienteRepositorio.Guardar
        ' Inserción o actualización
    End Sub
End Class
```

### Composición e inyección de dependencias

```vb
Dim repositorio As IClienteRepositorio = New ClienteRepositorioSql(cadena)
Dim servicio As New ClienteServicio(repositorio)
servicio.Registrar(New Cliente With {.Nombre = "Ana"})
```

### Organización de proyectos

```
Solucion.sln
├── Ventas.UI          (Windows Forms)
├── Ventas.Negocio     (servicios)
├── Ventas.Datos       (repositorios)
└── Ventas.Pruebas     (pruebas unitarias)
```

## Casos de Uso

- Aplicaciones de negocio con interfaz, lógica y datos diferenciados.
- Sustitución de la base de datos sin afectar la lógica de negocio.
- Pruebas unitarias de la capa de negocio sin acceso real a datos.
- Desarrollo en paralelo por equipos según capa.
- Reutilización de la lógica de negocio desde varias interfaces.

## Ejemplo de Código Mínimo

```vb
Imports System.Collections.Generic
Imports System.Linq

Public Class Producto
    Public Property Nombre As String
    Public Property Precio As Decimal
End Class

Public Interface IProductoRepositorio
    Sub Guardar(producto As Producto)
    Function Todos() As IEnumerable(Of Producto)
End Interface

Public Class ProductoServicio
    Private ReadOnly _repositorio As IProductoRepositorio

    Public Sub New(repositorio As IProductoRepositorio)
        _repositorio = repositorio
    End Sub

    Public Function ObtenerCaros(umbral As Decimal) As IEnumerable(Of Producto)
        Return _repositorio.Todos().Where(Function(p) p.Precio > umbral)
    End Function
End Class

Public Class ProductoRepositorioEnMemoria
    Implements IProductoRepositorio

    Private ReadOnly _datos As New List(Of Producto)

    Public Sub Guardar(producto As Producto) Implements IProductoRepositorio.Guardar
        _datos.Add(producto)
    End Sub

    Public Function Todos() As IEnumerable(Of Producto) Implements IProductoRepositorio.Todos
        Return _datos
    End Function
End Class

Module Modulo
    Sub Main()
        Dim repositorio As IProductoRepositorio = New ProductoRepositorioEnMemoria()
        Dim servicio As New ProductoServicio(repositorio)

        servicio.ObtenerCaros(20D)
    End Sub
End Module
```
