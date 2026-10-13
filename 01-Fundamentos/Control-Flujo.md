# Control de Flujo en VB.NET

## Definición

Las estructuras de control de flujo determinan el orden de ejecución de las instrucciones: selección condicional, iteración y transferencia incondicional.

## Sintaxis / Métodos

### If / Then / ElseIf / Else

```vb
Dim nota As Integer = 7
If nota >= 9 Then
    Console.WriteLine("Sobresaliente")
ElseIf nota >= 5 Then
    Console.WriteLine("Aprobado")
Else
    Console.WriteLine("Suspenso")
End If
```

Forma de una línea: `If condicion Then instruccion`.

Operador ternario `If`:

```vb
Dim etiqueta As String = If(activo, "Activo", "Inactivo")
```

### Select Case

```vb
Dim dia As String = "lun"
Select Case dia
    Case "sab", "dom"
        Console.WriteLine("Fin de semana")
    Case "lun" To "vie"
        Console.WriteLine("Laborable")
    Case Else
        Console.WriteLine("Desconocido")
End Select
```

### Bucles

```vb
For i As Integer = 0 To 4
    Console.WriteLine(i)
Next

For i As Integer = 10 To 0 Step -2
    Console.WriteLine(i)
Next

For Each item As String In coleccion
    Console.WriteLine(item)
Next

Dim j As Integer = 0
While j < 5
    j += 1
End While

Dim k As Integer = 0
Do
    k += 1
Loop While k < 5

Do Until k >= 10
    k += 1
Loop
```

### Transferencia

- `Exit For` / `Exit While` / `Exit Do`: salida del bucle.
- `Continue For` / `Continue While` / `Continue Do`: siguiente iteración.
- `Return`: salida del procedimiento.
- `GoTo`: salto a etiqueta.

### Operadores lógicos y de comparación

```vb
Dim valido As Boolean = (edad >= 18) AndAlso tienePermiso
Dim alternativo As Boolean = esAdmin OrElse esSupervisor
Dim negado As Boolean = Not activo
Dim igual As Boolean = (a = b)
Dim distinto As Boolean = (a <> b)
```

`AndAlso` y `OrElse` aplican evaluación de cortocircuito.

## Casos de Uso

- `If`/`ElseIf` para decisiones con pocas ramas y condiciones complejas.
- `Select Case` para seleccionar entre múltiples valores discretos de una variable.
- `For...Next` cuando se conoce el número de iteraciones.
- `For Each...Next` para recorrer colecciones y arrays.
- `While`/`Do...Loop` cuando la condición de parada depende del estado interno.
- `Continue` para omitir elementos no válidos sin anidar condicionales.

## Ejemplo de Código Mínimo

```vb
Module Modulo
    Sub Main()
        Dim numeros() As Integer = {3, 8, 15, 4, 23}
        Dim sumaPares As Integer = 0

        For Each n As Integer In numeros
            If n Mod 2 <> 0 Then Continue For
            sumaPares += n
        Next

        Console.WriteLine("Suma de pares: " & sumaPares)

        Select Case sumaPares
            Case Is > 20
                Console.WriteLine("Resultado alto")
            Case 10 To 20
                Console.WriteLine("Resultado medio")
            Case Else
                Console.WriteLine("Resultado bajo")
        End Select
    End Sub
End Module
```
