# Colecciones Genéricas en VB.NET

## Definición

Una colección genérica es un tipo del espacio de nombres `System.Collections.Generic` que almacena elementos de un tipo fijado en la declaración. Proporciona comprobación de tipos en compilación y evita el *boxing* presente en `System.Collections`.

## Sintaxis / Métodos

### Declaración

```vb
Imports System.Collections.Generic
```

| Tipo | Descripción | Acceso |
|---|---|---|
| `List(Of T)` | Lista dinámica indexada | Índice, O(1) |
| `Dictionary(Of TKey, TValue)` | Pares clave-valor | Clave, O(1) |
| `HashSet(Of T)` | Conjunto sin duplicados | Valor, O(1) |
| `Queue(Of T)` | Cola FIFO | Enqueue/Dequeue |
| `Stack(Of T)` | Pila LIFO | Push/Pop |
| `SortedList(Of TKey, TValue)` | Diccionario ordenado por clave | Clave |
| `LinkedList(Of T)` | Lista doblemente enlazada | Nodo |

### List(Of T)

```vb
Dim nombres As New List(Of String) From {"Ana", "Luis"}
nombres.Add("Marta")
nombres.Insert(0, "Pedro")
nombres.Remove("Luis")
nombres.RemoveAt(0)
Dim total As Integer = nombres.Count
Dim existe As Boolean = nombres.Contains("Ana")
nombres.Sort()
```

### Dictionary(Of TKey, TValue)

```vb
Dim edades As New Dictionary(Of String, Integer)
edades("Ana") = 30
edades.Add("Luis", 25)
edades.Remove("Luis")

Dim edad As Integer
If edades.TryGetValue("Ana", edad) Then
    Console.WriteLine(edad)
End If

For Each par As KeyValuePair(Of String, Integer) In edades
    Console.WriteLine(par.Key & ": " & par.Value)
Next
```

### HashSet(Of T)

```vb
Dim unicos As New HashSet(Of Integer) From {1, 2, 2, 3}
unicos.Add(4)
Dim union = unicos
Dim interseccion = unicos.Intersect(New Integer() {2, 3, 9})
```

### Queue(Of T) y Stack(Of T)

```vb
Dim cola As New Queue(Of String)
cola.Enqueue("primero")
Dim siguiente As String = cola.Dequeue()

Dim pila As New Stack(Of String)
pila.Push("base")
Dim cima As String = pila.Pop()
```

### Interfaces asociadas

```vb
Dim lista As IList(Of String) = New List(Of String)
Dim soloLectura As IEnumerable(Of Integer) = New List(Of Integer)
```

## Casos de Uso

- `List(Of T)` como colección general y para resultados de consultas.
- `Dictionary(Of TKey, TValue)` para búsquedas por clave e índices en memoria.
- `HashSet(Of T)` para eliminar duplicados y operaciones de conjuntos.
- `Queue(Of T)` para procesamiento FIFO (colas de trabajo).
- `Stack(Of T)` para deshacer/rehacer y recorrido en profundidad.
- Programar contra `IList(Of T)`/`IDictionary` para desacoplar la implementación.

## Ejemplo de Código Mínimo

```vb
Imports System.Collections.Generic

Module Modulo
    Sub Main()
        Dim inventario As New Dictionary(Of String, Integer)
        inventario("teclado") = 15
        inventario("ratón") = 8
        inventario("monitor") = 3

        Dim stock As Integer
        If inventario.TryGetValue("monitor", stock) Then
            Console.WriteLine("Stock de monitor: " & stock)
        End If

        Dim claves As New List(Of String)(inventario.Keys)
        claves.Sort()

        For Each clave As String In claves
            Console.WriteLine(clave & " -> " & inventario(clave))
        Next
    End Sub
End Module
```
