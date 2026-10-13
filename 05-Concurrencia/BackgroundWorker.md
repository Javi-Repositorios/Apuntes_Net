# BackgroundWorker en VB.NET

## Definición

`System.ComponentModel.BackgroundWorker` es un componente de .NET Framework que ejecuta una operación en un hilo secundario y permite notificar progreso y finalización al hilo de la interfaz. Es la alternativa anterior a `Task`/`Async`-`Await`.

## Sintaxis / Métodos

### Miembros y eventos

| Miembro | Tipo | Descripción |
|---|---|---|
| `DoWork` | Evento | Ejecuta el trabajo en segundo plano |
| `ProgressChanged` | Evento | Reporta progreso (hilo de UI) |
| `RunWorkerCompleted` | Evento | Finalización (hilo de UI) |
| `WorkerReportsProgress` | Propiedad | Habilita `ReportProgress` |
| `WorkerSupportsCancellation` | Propiedad | Habilita cancelación |
| `RunWorkerAsync()` | Método | Inicia el trabajo |
| `RunWorkerAsync(arg)` | Método | Inicia con argumento |
| `CancelAsync()` | Método | Solicita cancelación |
| `ReportProgress(pct)` | Método | Notifica progreso |
| `CancellationPending` | Propiedad | Indica cancelación solicitada |
| `IsBusy` | Propiedad | Indica ejecución en curso |

### Uso declarativo con WithEvents y Handles

```vb
Public Class FrmTrabajo
    Inherits Form

    Private WithEvents worker As New BackgroundWorker()

    Public Sub New()
        worker.WorkerReportsProgress = True
        worker.WorkerSupportsCancellation = True
    End Sub

    Private Sub worker_DoWork(sender As Object, e As DoWorkEventArgs) Handles worker.DoWork
        For i As Integer = 0 To 100
            If worker.CancellationPending Then
                e.Cancel = True
                Return
            End If
            Trabajar()
            worker.ReportProgress(i)
        Next
        e.Result = "Completado"
    End Sub

    Private Sub worker_ProgressChanged(sender As Object, e As ProgressChangedEventArgs) _
        Handles worker.ProgressChanged

        progressBar.Value = e.ProgressPercentage
    End Sub

    Private Sub worker_RunWorkerCompleted(sender As Object, e As RunWorkerCompletedEventArgs) _
        Handles worker.RunWorkerCompleted

        If e.Cancelled Then
            MessageBox.Show("Cancelado")
        ElseIf e.Error IsNot Nothing Then
            MessageBox.Show("Error: " & e.Error.Message)
        Else
            MessageBox.Show(e.Result.ToString())
        End If
    End Sub
End Class
```

### Uso programático con AddHandler

```vb
Dim worker As New BackgroundWorker()
worker.WorkerReportsProgress = True
AddHandler worker.DoWork, AddressOf AlTrabajar
AddHandler worker.RunWorkerCompleted, AddressOf AlCompletar
worker.RunWorkerAsync()
```

### Reglas de hilos

- El controlador de `DoWork` se ejecuta en el hilo secundario y no debe acceder a controles.
- Los controladores de `ProgressChanged` y `RunWorkerCompleted` se ejecutan en el hilo de interfaz y pueden actualizar controles.
- `e.Cancel = True` en `DoWork` marca la operación como cancelada.

## Casos de Uso

- Procesos largos con barra de progreso en aplicaciones Windows Forms.
- Operaciones cancelables por el usuario.
- Código heredado de .NET Framework 2.0/3.5 que no dispone de `Async`/`Await`.
- Carga de datos o ficheros sin congelar la interfaz.

## Ejemplo de Código Mínimo

```vb
Imports System.ComponentModel
Imports System.Threading

Module Modulo
    Sub Main()
        Dim worker As New BackgroundWorker()
        worker.WorkerReportsProgress = True
        worker.WorkerSupportsCancellation = True

        AddHandler worker.DoWork,
            Sub(sender As Object, e As DoWorkEventArgs)
                For i As Integer = 1 To 3
                    Thread.Sleep(300)
                    DirectCast(sender, BackgroundWorker).ReportProgress(i * 33)
                Next
                e.Result = "Finalizado"
            End Sub

        AddHandler worker.ProgressChanged,
            Sub(sender As Object, e As ProgressChangedEventArgs)
                Console.WriteLine("Progreso: " & e.ProgressPercentage & "%")
            End Sub

        AddHandler worker.RunWorkerCompleted,
            Sub(sender As Object, e As RunWorkerCompletedEventArgs)
                Console.WriteLine(e.Result.ToString())
            End Sub

        worker.RunWorkerAsync()
        Console.ReadLine()
    End Sub
End Module
```
