# Sintaxis, Simbolos y Palabras Clave en VB.NET

## Definición

La sintaxis de VB.NET define las reglas de construccion del codigo: como se declaran identificadores, como se separan instrucciones, que simbolos y operadores se utilizan y que palabras clave tienen significado reservado para el compilador.

## Sintaxis / Métodos

### Estructura del codigo

Un archivo `.vb` contiene, opcionalmente, opciones del compilador, declaraciones `Imports` y uno o varios tipos (`Module`, `Class`, `Structure`, `Interface`, `Enum`).

```vb
Option Strict On
Option Explicit On
Option Infer On

Imports System
Imports System.Collections.Generic

Namespace Ventas
    Module Program
        Sub Main()
            Console.WriteLine("Inicio")
        End Sub
    End Module
End Namespace
```

### Identificadores

- Deben empezar por letra o guion bajo `_`.
- Admiten letras, digitos y guion bajo.
- No distinguen mayusculas de minusculas.
- Se pueden usar palabras clave como nombre encerrandolas entre corchetes: `[Class]`.

### Separacion de instrucciones

- Una instruccion por linea.
- Varias instrucciones en una linea con el separador `:`.
- Continuacion de linea con el caracter `_` al final o de forma implicita tras una coma, operador o parentesis abierto.

```vb
Dim a As Integer = 1 : Dim b As Integer = 2

Dim total As Integer = 10 +
                       20 +
                       30
```

### Comentarios

```vb
' Comentario de una linea

REM Comentario mediante palabra clave

''' <summary>Documentacion XML del miembro.</summary>
```

### Simbolos y operadores

| Grupo | Simbolos |
|---|---|
| Asignacion | `=` |
| Aritmeticos | `+`, `-`, `*`, `/`, `\`, `Mod`, `^` |
| Comparacion | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Is`, `IsNot`, `Like` |
| Concatenacion | `&`, `+` |
| Logicos | `And`, `Or`, `Not`, `Xor`, `AndAlso`, `OrElse` |
| Incremento | `+=`, `-=`, `*=`, `/=`, `\=`, `^=`, `&=` |
| Cadena / caracter | `"texto"`, `"` (comilla escapada en literal) |
| Fecha / decimal | `#1/1/2026#`, `19.99D` |
| Sufijos de literal | `S` (Short), `I` (Integer), `L` (Long), `F` (Single), `R` (Double), `D` (Decimal), `US`, `UI`, `UL` |
| Bloques | `(...)`, `{...}` (inicializadores), `<>` (genericos) |
| Acceso | `.` (miembros), `:` (nombre completo) |
| Interpolacion | `$"Total: {total}"` |

### Palabras clave por categoria

| Categoria | Palabras clave |
|---|---|
| Declaracion | `Dim`, `Const`, `Static`, `ReadOnly`, `ReDim` |
| Tipos | `Class`, `Structure`, `Module`, `Interface`, `Enum`, `Namespace`, `Delegate`, `Event` |
| POO | `Inherits`, `Implements`, `Overridable`, `Overrides`, `MustInherit`, `MustOverride`, `NotInheritable`, `Me`, `MyBase`, `MyClass`, `Shared` |
| Acceso | `Public`, `Private`, `Protected`, `Friend` |
| Control | `If`, `Then`, `ElseIf`, `Else`, `End`, `Select`, `Case`, `For`, `Next`, `For Each`, `While`, `Do`, `Loop`, `Exit`, `Continue`, `GoTo`, `Return`, `With` |
| Excepciones | `Try`, `Catch`, `Finally`, `Throw`, `When` |
| Conversion | `CType`, `DirectCast`, `TryCast`, `CInt`, `CDbl`, `CStr`, `CBool` |
| Modularidad | `Sub`, `Function`, `Property`, `Get`, `Set`, `AddHandler`, `RemoveHandler`, `Handles`, `RaiseEvent` |
| Tipos literales | `Nothing`, `True`, `False`, `Null` (no existe; se usa `Nothing`), `True` |
| Modificadores | `Overloads`, `Shadows`, `Optional`, `ByVal`, `ByRef`, `ParamArray`, `Async`, `Await`, `Iterator`, `Yield`, `Partial`, `WithEvents` |
| Opciones | `Option Strict`, `Option Explicit`, `Option Infer`, `Option Compare` |

### Operadores y metodos habituales

```vb
Dim suma As Integer = 10 + 5
Dim divisionEntera As Integer = 10 \ 3      ' 3
Dim modulo As Integer = 10 Mod 3            ' 1
Dim potencia As Double = 2 ^ 10             ' 1024
Dim texto As String = "Hola" & ", mundo"
Dim igual As Boolean = (a = b)
Dim rango As Boolean = (x Like "[A-Z]*")
```

### Conversion y comprobacion

```vb
Dim obj As Object = "texto"
Dim s As String = CStr(obj)
Dim directo As String = DirectCast(obj, String)
Dim posible As Button = TryCast(obj, Button)
If TypeOf obj Is String Then End If
```

### Alcance y bloques `With ... End With`

```vb
Dim r As New Rectangulo()
With r
    .Ancho = 4.0
    .Alto = 5.0
End With
```

## Casos de Uso

- `Option Strict On` para forzar conversiones explicitas y comprobacion de tipos.
- `Option Explicit On` para exigir la declaracion de todas las variables.
- `Option Infer On` para permitir la inferencia de tipos con `Dim`.
- `:` para compactar inicializaciones cortas; `_` para dividir expresiones largas.
- `$"..."` para cadenas interpoladas en lugar de concatenacion.
- `With ... End With` para inicializar varios miembros de un objeto.
- `AndAlso`/`OrElse` para evaluacion de cortocircuito en condiciones.

## Ejemplo de Código Mínimo

```vb
Option Strict On
Option Explicit On

Imports System

Namespace Demo
    Module Program
        Sub Main()
            Dim precio As Decimal = 19.99D      ' sufijo decimal
            Dim unidades As Integer = 3 : Dim total As Decimal = precio * unidades

            Dim etiqueta As String = $"Total: {total:C}"   ' interpolacion
            Dim aplicaDescuento As Boolean = (total > 50D) AndAlso (unidades >= 2)

            Console.WriteLine(etiqueta)
            Console.WriteLine("Descuento: " & aplicaDescuento)
        End Sub
    End Module
End Namespace
```
