# Ciclo de Vida del Formulario en VB.NET

## Definición

Un formulario de Windows Forms es una instancia de `System.Windows.Forms.Form` que transita por una secuencia de estados desde su creación hasta su liberación. Cada transición expone eventos que permiten insertar lógica en el momento adecuado.

## Sintaxis / Métodos

### Punto de entrada

```vb
Module Program
    <STAThread>
    Sub Main()
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New FrmPrincipal())
    End Sub
End Module
```

`Application.Run` inicia el bucle de mensajes que procesa la interacción del usuario.

### Orden de eventos principales

1. `Load` — inicialización de controles y datos.
2. `Shown` — el formulario es visible.
3. `Activated` / `Deactivate` — obtiene o pierde el foco.
4. `Paint` — dibujo del formulario.
5. `FormClosing` — antes del cierre; cancelable.
6. `FormClosed` — tras el cierre.
7. `Dispose` — liberación de recursos.

```vb
Public Class FrmPrincipal
    Inherits Form

    Private Sub FrmPrincipal_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        CargarDatos()
    End Sub

    Private Sub FrmPrincipal_FormClosing(sender As Object, e As FormClosingEventArgs) _
        Handles MyBase.FormClosing

        If HayCambiosSinGuardar() Then
            Dim respuesta = MessageBox.Show("¿Salir sin guardar?", "Aviso",
                                            MessageBoxButtons.YesNo)
            If respuesta = DialogResult.No Then
                e.Cancel = True
            End If
        End If
    End Sub
End Class
```

### Formularios modales y no modales

```vb
Using dlg As New FrmDetalle()
    If dlg.ShowDialog() = DialogResult.OK Then
        Dim resultado = dlg.Resultado
    End If
End Using

Dim otra As New FrmSecundario()
otra.Show()
```

### Aplicación y cierre

| Miembro | Efecto |
|---|---|
| `Application.Run(frm)` | Inicia el bucle de mensajes |
| `Application.Exit()` | Finaliza la aplicación |
| `Me.Close()` | Cierra el formulario |
| `e.Cancel = True` | Cancela el cierre en `FormClosing` |

### Eventos de control

Un control expone `Enter`, `Leave`, `Validating`, `Validated`, `KeyPress` para la validación de entrada.

## Casos de Uso

- Cargar datos en `Load` para evitar trabajo antes de mostrar el formulario.
- Confirmar el cierre con cambios pendientes en `FormClosing`.
- Validar la entrada en `Validating`/`Validated`.
- Liberar recursos en `Dispose` o `FormClosed`.
- Abrir formularios de edición modales con `ShowDialog`.

## Ejemplo de Código Mínimo

```vb
Public Class FrmPrincipal
    Inherits Form

    Private btnCerrar As Button
    Private cambiosPendientes As Boolean = True

    Public Sub New()
        Me.Text = "Ciclo de vida"
        Me.Size = New Size(300, 150)

        btnCerrar = New Button With {.Text = "Cerrar", .Left = 20, .Top = 20}
        AddHandler btnCerrar.Click, Sub(sender, e) Me.Close()
        Me.Controls.Add(btnCerrar)
    End Sub

    Private Sub FrmPrincipal_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Console.WriteLine("Formulario cargado")
    End Sub

    Private Sub FrmPrincipal_FormClosing(sender As Object, e As FormClosingEventArgs) _
        Handles MyBase.FormClosing

        If cambiosPendientes Then
            Dim r = MessageBox.Show("Hay cambios sin guardar. ¿Salir?",
                                    "Confirmar", MessageBoxButtons.YesNo)
            If r = DialogResult.No Then e.Cancel = True
        End If
    End Sub
End Class
```
