# Windows Forms: Controles en VB.NET

## Definición

Windows Forms es la tecnología de interfaz gráfica de escritorio de .NET Framework. Cada elemento visual deriva de `System.Windows.Forms.Control` y se organiza dentro de un formulario que hereda de `System.Windows.Forms.Form`.

## Sintaxis / Métodos

### Formulario base

```vb
Public Partial Class FrmPrincipal
    Inherits Form

    Private WithEvents btnSaludar As Button
    Private txtNombre As TextBox

    Public Sub New()
        InitializeComponent()
        InicializarControles()
    End Sub

    Private Sub InicializarControles()
        Me.Text = "Aplicación"
        Me.Size = New Size(400, 200)

        txtNombre = New TextBox With {.Left = 20, .Top = 20, .Width = 200}
        btnSaludar = New Button With {.Text = "Saludar", .Left = 20, .Top = 60}

        Me.Controls.Add(txtNombre)
        Me.Controls.Add(btnSaludar)
    End Sub

    Private Sub btnSaludar_Click(sender As Object, e As EventArgs) Handles btnSaludar.Click
        MessageBox.Show("Hola " & txtNombre.Text)
    End Sub
End Class
```

### Controles habituales

| Control | Uso |
|---|---|
| `Label` | Texto informativo |
| `TextBox` | Entrada de texto |
| `Button` | Disparo de acciones |
| `CheckBox` | Opción booleana |
| `RadioButton` | Opción exclusiva |
| `ComboBox` | Lista desplegable |
| `ListBox` | Lista de elementos |
| `DataGridView` | Tabla de datos |
| `MenuStrip` | Menús |
| `Timer` | Acciones periódicas |
| `PictureBox` | Imágenes |

### Propiedades de disposición

```vb
control.Anchor = AnchorStyles.Top Or AnchorStyles.Left
control.Dock = DockStyle.Fill
control.Enabled = False
control.Visible = True
```

### Contenedores de disposición

`Panel`, `GroupBox`, `TabControl`, `TableLayoutPanel`, `FlowLayoutPanel`.

### Enlace de datos

```vb
comboCiudades.DataSource = listaCiudades
comboCiudades.DisplayMember = "Nombre"
comboCiudades.ValueMember = "Id"

grid.DataSource = tabla
```

## Casos de Uso

- Aplicaciones de escritorio con formularios y controles estándar.
- Interfaces de administración que editan registros de base de datos.
- Herramientas internas con poco requerimiento visual.
- Formularios con `DataGridView` enlazado a `DataTable` o listas.

## Ejemplo de Código Mínimo

```vb
Public Class FrmContador
    Inherits Form

    Private lblValor As Label
    Private btnIncrementar As Button
    Private contador As Integer

    Public Sub New()
        Me.Text = "Contador"
        Me.Size = New Size(260, 140)

        lblValor = New Label With {.Text = "0", .Left = 20, .Top = 20, .Width = 100}
        btnIncrementar = New Button With {.Text = "+1", .Left = 20, .Top = 50}

        AddHandler btnIncrementar.Click, AddressOf Incrementar

        Me.Controls.Add(lblValor)
        Me.Controls.Add(btnIncrementar)
    End Sub

    Private Sub Incrementar(sender As Object, e As EventArgs)
        contador += 1
        lblValor.Text = contador.ToString()
    End Sub
End Class
```
