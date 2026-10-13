# LINQ to Objects en VB.NET

## Definición

LINQ (*Language Integrated Query*) es un conjunto de operadores integrados en el lenguaje para consultar secuencias que implementan `IEnumerable(Of T)`. LINQ to Objects opera sobre colecciones en memoria.

## Sintaxis / Métodos

```vb
Imports System.Linq
```

### Sintaxis de consulta

```vb
Dim mayores = From p In personas
              Where p.Edad >= 18
              Order By p.Nombre
              Select p.Nombre
```

### Sintaxis de método (fluyente)

```vb
Dim mayores = personas _
    .Where(Function(p) p.Edad >= 18) _
    .OrderBy(Function(p) p.Nombre) _
    .Select(Function(p) p.Nombre) _
    .ToList()
```

### Operadores habituales

| Operador (consulta) | Método | Función |
|---|---|---|
| `Where` | `Where` | Filtrado |
| `Select` | `Select` | Proyección |
| `Order By` | `OrderBy`/`OrderByDescending` | Ordenación |
| `Group By` | `GroupBy` | Agrupación |
| `Join` | `Join` | Combinación |
| `Distinct` | `Distinct` | Elimina duplicados |
| `Take`/`Skip` | `Take`/`Skip` | Paginación |

### Funciones de agregado

```vb
Dim total As Integer = numeros.Sum()
Dim media As Double = numeros.Average()
Dim minimo As Integer = numeros.Min()
Dim maximo As Integer = numeros.Max()
Dim cuenta As Integer = numeros.Count()
```

### Operadores de elemento y cuantificadores

```vb
Dim primero = numeros.First()
Dim primeroODefecto = numeros.FirstOrDefault()
Dim hayPares As Boolean = numeros.Any(Function(n) n Mod 2 = 0)
Dim todosPositivos As Boolean = numeros.All(Function(n) n > 0)
Dim soloUno = numeros.Single(Function(n) n = 5)
```

### Agrupación y proyección anónima

```vb
Dim porCiudad = From p In personas
                Group By Ciudad = p.Ciudad Into Total = Count()
                Select Ciudad, Total
```

### Unión

```vb
Dim resultado = From c In compras
                Join cl In clientes On c.ClienteId Equals cl.Id
                Select c.Producto, cl.Nombre
```

### Generadores y ejecución diferida

Los operadores devuelven `IEnumerable(Of T)` diferido; la ejecución se produce al enumerar.

```vb
Dim consulta = From n In numeros Where n Mod 2 = 0
For Each n In consulta
    Console.WriteLine(n)
Next
```

## Casos de Uso

- Filtrar, proyectar, ordenar y agrupar colecciones en memoria.
- Sustituir bucles `For Each` con lógica de filtrado por consultas declarativas.
- Calcular agregados sobre listas de objetos de dominio.
- Combinar colecciones en memoria mediante `Join`/`GroupJoin`.
- Aplicar paginación con `Skip`/`Take`.

## Ejemplo de Código Mínimo

```vb
Imports System.Linq

Public Class Producto
    Public Property Nombre As String
    Public Property Precio As Decimal
    Public Property Categoria As String
End Class

Module Modulo
    Sub Main()
        Dim productos As New List(Of Producto) From {
            New Producto With {.Nombre = "Teclado", .Precio = 25D, .Categoria = "HW"},
            New Producto With {.Nombre = "Ratón", .Precio = 15D, .Categoria = "HW"},
            New Producto With {.Nombre = "Curso", .Precio = 100D, .Categoria = "SW"}
        }

        Dim caros = From p In productos
                    Where p.Precio > 20D
                    Order By p.Precio Descending
                    Select p.Nombre

        For Each nombre In caros
            Console.WriteLine(nombre)
        Next

        Dim totalPorCategoria = From p In productos
                                Group By p.Categoria Into Total = Sum(p.Precio)
                                Select Categoria, Total

        For Each g In totalPorCategoria
            Console.WriteLine(g.Categoria & ": " & g.Total)
        Next
    End Sub
End Module
```
