# Herencia e Interfaces en VB.NET

## Definición

La herencia permite que una clase derivada reutilice y especialice los miembros de una clase base (relación *es-un*). Una interfaz define un contrato de miembros que las clases implementadoras deben proporcionar, sin definir su implementación (relación *puede-hacer*).

## Sintaxis / Métodos

### Herencia

```vb
Public Class Animal
    Public Property Nombre As String
    Public Overridable Sub Hablar()
        Console.WriteLine("...")
    End Sub
End Class

Public Class Perro
    Inherits Animal

    Public Overrides Sub Hablar()
        Console.WriteLine("Guau")
    End Sub
End Class
```

Palabras clave:

| Clave | Función |
|---|---|
| `Inherits` | Indica la clase base |
| `Overridable` | Miembro sobrescribible |
| `Overrides` | Sobrescribe un miembro base |
| `NotOverridable` | Impide sobrescribir de nuevo |
| `MustInherit` | Clase abstracta |
| `MustOverride` | Miembro abstracto |
| `NotInheritable` | Clase sellada |
| `MyBase` | Acceso a la clase base |

### Clases abstractas y selladas

```vb
Public MustInherit Class Forma
    Public MustOverride Function Area() As Double
End Class

Public NotInheritable Class Circulo
    Inherits Forma

    Public Property Radio As Double

    Public Overrides Function Area() As Double
        Return Math.PI * Radio * Radio
    End Function
End Class
```

### Interfaces

```vb
Public Interface IRepositorio
    Sub Guardar(dato As String)
    Function Obtener(id As Integer) As String
End Interface

Public Class RepositorioSql
    Implements IRepositorio

    Public Sub Guardar(dato As String) Implements IRepositorio.Guardar
    End Sub

    Public Function Obtener(id As Integer) As String Implements IRepositorio.Obtener
        Return ""
    End Function
End Class
```

Una clase admite herencia simple y múltiples interfaces.

```vb
Public Class Servicio
    Implements IDisposable, IComparable
End Class
```

### Polimorfismo

```vb
Dim a As Animal = New Perro With {.Nombre = "Rex"}
a.Hablar()   ' resuelve en ejecución
```

### Shadowing y comprobación de tipos

```vb
If TypeOf a Is Perro Then
End If

Dim p As Perro = TryCast(a, Perro)
```

## Casos de Uso

- Herencia para jerarquías con comportamiento común y especialización.
- `MustInherit`/`MustOverride` para plantillas que obligan a implementar.
- Interfaces para desacoplar componentes y permitir múltiples implementaciones.
- Interfaces para pruebas unitarias mediante dobles de prueba.
- `NotInheritable` para tipos de valor seguro que no deben extenderse.

## Ejemplo de Código Mínimo

```vb
Public Interface IFigura
    Function CalcularArea() As Double
End Interface

Public MustInherit Class FiguraBase
    Implements IFigura
    Public MustOverride Function CalcularArea() As Double Implements IFigura.CalcularArea
End Class

Public Class Triangulo
    Inherits FiguraBase

    Public Property Base As Double
    Public Property Altura As Double

    Public Overrides Function CalcularArea() As Double
        Return (Base * Altura) / 2.0
    End Function
End Class

Module Modulo
    Sub Main()
        Dim figura As IFigura = New Triangulo With {.Base = 6.0, .Altura = 4.0}
        Console.WriteLine("Área: " & figura.CalcularArea())
    End Sub
End Module
```
