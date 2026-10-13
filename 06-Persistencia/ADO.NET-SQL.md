# ADO.NET y SQL en VB.NET

## Definición

ADO.NET es el conjunto de clases de .NET Framework para el acceso a datos relacionales. Implementa un modelo desconectado y conectado mediante proveedores específicos de cada motor de base de datos.

## Sintaxis / Métodos

### Objetos del proveedor (SQL Server)

| Objeto | Clase | Función |
|---|---|---|
| Conexión | `SqlConnection` | Abre la conexión |
| Comando | `SqlCommand` | Ejecuta sentencias SQL |
| Lector | `SqlDataReader` | Lectura secuencial de filas |
| Adaptador | `SqlDataAdapter` | Rellena `DataSet`/`DataTable` |
| Parámetro | `SqlParameter` | Parámetros de consulta |

Otros proveedores: `SQLiteConnection`, `MySqlConnection`, `OleDbConnection`.

### Cadena de conexión

```vb
Imports System.Data.SqlClient

Dim cadena As String = "Server=localhost;Database=Ventas;Integrated Security=True;"
```

### Consulta con DataReader

```vb
Using conexion As New SqlConnection(cadena)
    conexion.Open()

    Using comando As New SqlCommand(
        "SELECT Id, Nombre FROM Clientes WHERE Ciudad = @ciudad", conexion)
        comando.Parameters.AddWithValue("@ciudad", "Madrid")

        Using lector As SqlDataReader = comando.ExecuteReader()
            While lector.Read()
                Console.WriteLine(lector.GetInt32(0) & ": " & lector.GetString(1))
            End While
        End Using
    End Using
End Using
```

### Inserción, actualización y borrado

```vb
Using comando As New SqlCommand(
    "INSERT INTO Clientes (Nombre, Ciudad) VALUES (@n, @c)", conexion)
    comando.Parameters.AddWithValue("@n", nombre)
    comando.Parameters.AddWithValue("@c", ciudad)
    Dim filas As Integer = comando.ExecuteNonQuery()
End Using
```

| Método | Retorno | Uso |
|---|---|---|
| `ExecuteNonQuery()` | Filas afectadas | INSERT/UPDATE/DELETE |
| `ExecuteScalar()` | Primer valor | COUNT/MAX/SUM |
| `ExecuteReader()` | `SqlDataReader` | SELECT |

### Modelo desconectado

```vb
Using adaptador As New SqlDataAdapter("SELECT * FROM Clientes", conexion)
    Dim tabla As New DataTable()
    adaptador.Fill(tabla)
    grid.DataSource = tabla
End Using
```

### Prevención de inyección SQL

```vb
comando.CommandText = "SELECT * FROM Usuarios WHERE Nombre = @n"
comando.Parameters.AddWithValue("@n", entradaUsuario)
```

Los valores se pasan siempre como parámetros, nunca concatenados.

### Transacciones

```vb
Using transaccion As SqlTransaction = conexion.BeginTransaction()
    Try
        comando.Transaction = transaccion
        comando.ExecuteNonQuery()
        transaccion.Commit()
    Catch
        transaccion.Rollback()
        Throw
    End Try
End Using
```

## Casos de Uso

- Operaciones CRUD sobre bases de datos relacionales.
- Enlace de resultados a controles `DataGridView` mediante `DataTable`.
- Procesos transaccionales con confirmación o reversión.
- Consultas parametrizadas de datos introducidos por el usuario.
- Acceso asíncrono (`OpenAsync`, `ExecuteReaderAsync`) desde la interfaz.

## Ejemplo de Código Mínimo

```vb
Imports System.Data.SqlClient

Module Modulo
    Sub Main()
        Dim cadena As String = "Server=localhost;Database=Ventas;Integrated Security=True;"

        Using conexion As New SqlConnection(cadena)
            conexion.Open()

            Using comando As New SqlCommand(
                "SELECT COUNT(*) FROM Clientes WHERE Ciudad = @ciudad", conexion)
                comando.Parameters.AddWithValue("@ciudad", "Madrid")
                Dim total As Integer = CInt(comando.ExecuteScalar())
                Console.WriteLine("Clientes en Madrid: " & total)
            End Using
        End Using
    End Sub
End Module
```
