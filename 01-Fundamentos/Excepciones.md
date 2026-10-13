# Excepciones en VB.NET

## Definición

Una excepción es una instancia de `System.Exception` que representa un error en tiempo de ejecución. El bloque estructurado de control permite separar la lógica normal del tratamiento de errores y garantizar la liberación de recursos.

## Sintaxis / Métodos

### Try / Catch / Finally

```vb
Try
    Dim resultado As Integer = 10 \ Integer.Parse("0")
Catch ex As DivideByZeroException
    Console.WriteLine("División por cero: " & ex.Message)
Catch ex As FormatException
    Console.WriteLine("Formato inválido: " & ex.Message)
Catch ex As Exception
    Console.WriteLine("Error: " & ex.Message)
Finally
    Console.WriteLine("Bloque Finally")
End Try
```

### Propiedades de Exception

| Miembro | Descripción |
|---|---|
| `Message` | Descripción del error |
| `StackTrace` | Traza de llamadas |
| `InnerException` | Excepción original anidada |
| `Source` | Objeto o ensamblado origen |
| `GetBaseException()` | Excepción raíz |

### Lanzar excepciones

```vb
Public Sub Retirar(cantidad As Decimal)
    If cantidad <= 0D Then
        Throw New ArgumentOutOfRangeException(NameOf(cantidad), "Debe ser positiva")
    End If
End Sub
```

Re-lanzar conservando la pila:

```vb
Catch ex As Exception
    Registrar(ex)
    Throw
End Try
```

### Filtros de excepción (`When`)

```vb
Catch ex As SqlException When ex.Number = 2627
    Console.WriteLine("Clave duplicada")
End Try
```

### Excepciones personalizadas

```vb
Public Class SaldoInsuficienteException
    Inherits Exception

    Public Sub New(mensaje As String)
        MyBase.New(mensaje)
    End Sub
End Class
```

### Jerarquía habitual

```
Exception
├── SystemException
│   ├── ArgumentException
│   ├── InvalidOperationException
│   ├── NullReferenceException
│   └── IndexOutOfRangeException
└── ApplicationException
```

## Casos de Uso

- Validar argumentos de entrada y lanzar `ArgumentException` o `ArgumentNullException`.
- Capturar errores de E/S: `IOException`, `FileNotFoundException`, `UnauthorizedAccessException`.
- Gestionar errores de base de datos mediante `SqlException`.
- Liberar recursos (conexiones, flujos, handles) en `Finally`.
- Crear excepciones de dominio para reglas de negocio específicas.
- Filtrar excepciones por condición con `When` para no capturar de más.

## Ejemplo de Código Mínimo

```vb
Public Class CuentaBancaria
    Public Property Saldo As Decimal

    Public Sub Retirar(cantidad As Decimal)
        If cantidad <= 0D Then
            Throw New ArgumentOutOfRangeException(NameOf(cantidad), "Importe no válido")
        End If
        If cantidad > Saldo Then
            Throw New SaldoInsuficienteException("Saldo insuficiente")
        End If
        Saldo -= cantidad
    End Sub
End Class

Public Class SaldoInsuficienteException
    Inherits Exception
    Public Sub New(mensaje As String)
        MyBase.New(mensaje)
    End Sub
End Class

Module Modulo
    Sub Main()
        Dim cuenta As New CuentaBancaria With {.Saldo = 100D}
        Try
            cuenta.Retirar(150D)
        Catch ex As SaldoInsuficienteException
            Console.WriteLine(ex.Message)
        End Try
    End Sub
End Module
```
