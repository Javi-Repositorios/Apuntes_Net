# Clases y Miembros en VB.NET

## Definición

Una clase es un tipo por referencia que define estado (campos y propiedades) y comportamiento (métodos y eventos). Es la unidad base de la programación orientada a objetos y admite encapsulación, herencia y polimorfismo.

## Sintaxis / Métodos

### Declaración

```vb
Public Class Persona
    Private _nombre As String
    Public Property Edad As Integer

    Public Sub New(nombre As String, edad As Integer)
        _nombre = nombre
        Me.Edad = edad
    End Sub

    Public Sub Saludar()
        Console.WriteLine("Hola, soy " & _nombre)
    End Sub
End Class
```

### Miembros

| Miembro | Declaración |
|---|---|
| Campo | `Private _valor As Integer` |
| Propiedad | `Public Property Nombre As String` |
| Método | `Public Sub` / `Public Function` |
| Constructor | `Public Sub New(...)` |
| Evento | `Public Event Cambio As EventHandler` |
| Propiedad por defecto | `Default Public Property Item(i As Integer)` |

### Propiedades

```vb
Public Property Nombre As String            ' autoimplementada

Private _saldo As Decimal
Public Property Saldo As Decimal
    Get
        Return _saldo
    End Get
    Set(value As Decimal)
        If value < 0D Then Throw New ArgumentException("Negativo")
        _saldo = value
    End Set
End Property

Public ReadOnly Property Descripcion As String
    Get
        Return Nombre & " (" & Edad & ")"
    End Get
End Property
```

### Modificadores de acceso

`Public`, `Private`, `Protected`, `Friend`, `Protected Friend`.

### Compartidos y constantes

```vb
Public Class Config
    Public Const Pi As Double = 3.1415
    Public Shared Contador As Integer
    Public ReadOnly Inicio As DateTime

    Shared Sub New()
        Contador = 0
    End Sub

    Public Sub New()
        Inicio = DateTime.Now
    End Sub
End Class
```

### Sobrecarga y parámetros opcionales

```vb
Public Overloads Function Sumar(a As Integer, b As Integer) As Integer
    Return a + b
End Function

Public Overloads Function Sumar(a As Double, b As Double) As Double
    Return a + b
End Function

Public Sub Dibujar(Optional color As String = "negro")
End Sub
```

### `Me`, `MyClass`, `NameOf`

- `Me`: referencia a la instancia actual.
- `MyClass`: referencia no virtual a la implementación actual.
- `NameOf(x)`: devuelve el nombre del símbolo como cadena.

## Casos de Uso

- Modelar entidades de dominio con propiedades y métodos.
- Encapsular estado mutable mediante propiedades con validación.
- Agrupar constantes y utilidades relacionadas con miembros `Shared`.
- Definir constructores para garantizar estados válidos.
- Exponer propiedades de solo lectura calculadas.

## Ejemplo de Código Mínimo

```vb
Public Class Rectangulo
    Public Property Ancho As Double
    Public Property Alto As Double

    Public Sub New(ancho As Double, alto As Double)
        Me.Ancho = ancho
        Me.Alto = alto
    End Sub

    Public ReadOnly Property Area As Double
        Get
            Return Ancho * Alto
        End Get
    End Property

    Public Function EsCuadrado() As Boolean
        Return Ancho = Alto
    End Function
End Class

Module Modulo
    Sub Main()
        Dim r As New Rectangulo(4.0, 5.0)
        Console.WriteLine("Área: " & r.Area)
        Console.WriteLine("¿Cuadrado? " & r.EsCuadrado())
    End Sub
End Module
```
