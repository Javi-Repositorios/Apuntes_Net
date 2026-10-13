# Task Parallel Library (TPL) en VB.NET

## Definición

La Task Parallel Library (`System.Threading.Tasks`) es el modelo de concurrencia basado en tareas de .NET Framework 4.5 y posteriores. Representa trabajo asíncrono mediante `Task` y `Task(Of TResult)`, con soporte de `Async`/`Await`.

## Sintaxis / Métodos

### Creación y ejecución de tareas

```vb
Dim tarea As Task = Task.Run(Sub()
                                 Console.WriteLine("Segundo plano")
                             End Sub)
tarea.Wait()
```

### Task(Of TResult)

```vb
Dim tarea As Task(Of Integer) = Task.Run(Function() Calcular())
Dim resultado As Integer = tarea.Result
```

### Async / Await

```vb
Public Async Function ObtenerDatoAsync() As Task(Of Integer)
    Await Task.Delay(1000)
    Return 42
End Function

Public Async Function EjecutarAsync() As Task
    Dim dato As Integer = Await ObtenerDatoAsync()
    Console.WriteLine(dato)
End Function
```

- `Async` modifica el método; `Await` suspende la ejecución sin bloquear el hilo.
- El método devuelve `Task`, `Task(Of T)` o `ValueTask`/`ValueTask(Of T)`.

### Ejecución en paralelo

```vb
Parallel.For(0, 10, Sub(i)
                        Console.WriteLine(i)
                    End Sub)

Parallel.ForEach(coleccion, Sub(item)
                                Procesar(item)
                            End Sub)

Parallel.Invoke(Sub() TareaA(), Sub() TareaB())
```

### Coordinación de tareas

```vb
Dim t1 As Task(Of Integer) = TareaAsync(1)
Dim t2 As Task(Of Integer) = TareaAsync(2)
Dim resultados() As Integer = Await Task.WhenAll(t1, t2)

Dim primera As Task(Of Integer) = Await Task.WhenAny(t1, t2)

Dim cts As New CancellationTokenSource()
cts.Cancel()
```

### Tratamiento de excepciones

```vb
Try
    Await Task.Run(Sub()
                       Throw New InvalidOperationException()
                   End Sub)
Catch ex As InvalidOperationException
    Console.WriteLine(ex.Message)
End Try
```

`Task.WhenAll` propaga las excepciones agregadas en `AggregateException`.

### Uso en Windows Forms

```vb
Private Async Sub btnCargar_Click(sender As Object, e As EventArgs) Handles btnCargar.Click
    btnCargar.Enabled = False
    Dim datos = Await ObtenerDatosAsync()
    grid.DataSource = datos
    btnCargar.Enabled = True
End Sub
```

`Await` captura el contexto de sincronización y retorna al hilo de interfaz.

## Casos de Uso

- Operaciones de E/S: red, disco y base de datos sin bloquear la interfaz.
- Cálculo intensivo paralelizable con `Parallel.For`/`ForEach`.
- Ejecución concurrente de múltiples operaciones independientes con `Task.WhenAll`.
- Cancelación cooperativa mediante `CancellationToken`.
- Sustitución de `Thread` y `BackgroundWorker` en código nuevo.

## Ejemplo de Código Mínimo

```vb
Imports System.Threading.Tasks

Module Modulo
    Async Function DescargarAsync(id As Integer) As Task(Of String)
        Await Task.Delay(500)
        Return "Dato " & id
    End Function

    Sub Main()
        MainAsync().GetAwaiter().GetResult()
    End Sub

    Async Function MainAsync() As Task
        Dim tarea1 As Task(Of String) = DescargarAsync(1)
        Dim tarea2 As Task(Of String) = DescargarAsync(2)

        Dim resultados() As String = Await Task.WhenAll(tarea1, tarea2)

        For Each r As String In resultados
            Console.WriteLine(r)
        Next
    End Function
End Module
```
