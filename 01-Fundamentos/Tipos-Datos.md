# Tipos de Datos en VB.NET

## Definición

Un tipo de dato especifica el conjunto de valores que puede almacenar una variable, su tamaño en memoria y las operaciones permitidas. VB.NET es un lenguaje de tipado estático; el tipo se resuelve en tiempo de compilación salvo con `Option Strict Off`.

## Sintaxis / Métodos

### Tipos por valor (value types)

| Palabra clave | Tipo .NET | Rango / tamaño |
|---|---|---|
| `Byte` | `Byte` | 0 a 255 |
| `SByte` | `SByte` | −128 a 127 |
| `Short` | `Int16` | −32.768 a 32.767 |
| `UShort` | `UInt16` | 0 a 65.535 |
| `Integer` | `Int32` | −2.147.483.648 a 2.147.483.647 |
| `UInteger` | `UInt32` | 0 a 4.294.967.295 |
| `Long` | `Int64` | −9,2·10^18 a 9,2·10^18 |
| `ULong` | `UInt64` | 0 a 1,8·10^19 |
| `Single` | `Single` | IEEE 754 32 bits |
| `Double` | `Double` | IEEE 754 64 bits |
| `Decimal` | `Decimal` | 128 bits, base 10 |
| `Boolean` | `Boolean` | `True` / `False` |
| `Char` | `Char` | carácter UTF-16 |
| `Date` | `DateTime` | fecha y hora |
| `Object` | `System.Object` | referencia base |

### Tipos por referencia (reference types)

`String`, `Object`, clases, interfaces, delegados y matrices (arrays).

### Declaración de variables

```vb
Dim entero As Integer = 42
Dim doble As Double = 3.14
Dim texto As String = "dato"
Dim activo As Boolean = True
Dim fecha As Date = Date.Now
```

### Inferencia de tipos

```vb
Dim nombre = "Ana"          ' String
Dim numero = 10             ' Integer
```

### Tipos anulables

```vb
Dim edad As Integer? = Nothing
Dim valor As Nullable(Of Integer) = 5
If edad.HasValue Then
    Console.WriteLine(edad.Value)
End If
```

### Conversiones

```vb
Dim d As Double = 9.7
Dim i As Integer = CInt(d)          ' redondea a 10
Dim s As String = CStr(123)
Dim x As Integer = CType(d, Integer)
Dim obj As Object = "hola"
Dim cadena As String = DirectCast(obj, String)
Dim posible As Button = TryCast(obj, Button)   ' Nothing si falla
```

Métodos `TryParse`:

```vb
Dim numero As Integer
If Integer.TryParse("123", numero) Then
    Console.WriteLine(numero)
End If
```

### Recursos del objeto

`GetType()`, `TypeOf ... Is`, `Double.TryParse`, `Convert.ToInt32`, `String.Format`.

## Casos de Uso

- `Integer` y `Long` para contadores, índices e identificadores.
- `Decimal` para importes monetarios; evita los errores de redondeo de `Double`.
- `Double` para cálculos científicos y geométricos.
- `Date`/`DateTime` para marcas temporales.
- `String` con `StringBuilder` cuando se concatenan grandes volúmenes.
- `Nullable(Of T)` para columnas de base de datos que admiten NULL.
- `Structure` para datos pequeños inmutables que no requieren herencia.

## Ejemplo de Código Mínimo

```vb
Module Modulo
    Sub Main()
        Dim cantidad As Decimal = 19.99D
        Dim unidades As Integer = 3
        Dim total As Decimal = cantidad * unidades
        Console.WriteLine("Total: " & total.ToString("C"))

        Dim edad As Integer? = Nothing
        If Not edad.HasValue Then
            Console.WriteLine("Edad no especificada")
        End If

        Dim entrada As String = "250"
        Dim convertido As Integer
        If Integer.TryParse(entrada, convertido) Then
            Console.WriteLine(convertido * 2)
        End If
    End Sub
End Module
```
