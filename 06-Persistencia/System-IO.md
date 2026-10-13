# System.IO en VB.NET

## Definición

`System.IO` es el espacio de nombres de .NET Framework que proporciona tipos para la manipulación de ficheros, directorios, rutas y flujos (*streams*) de entrada/salida.

## Sintaxis / Métodos

### Clases principales

| Clase | Función |
|---|---|
| `File` | Operaciones estáticas sobre ficheros |
| `Directory` | Operaciones estáticas sobre directorios |
| `FileInfo` | Fichero orientado a objetos |
| `DirectoryInfo` | Directorio orientado a objetos |
| `Path` | Manipulación de cadenas de ruta |
| `StreamReader`/`StreamWriter` | Texto |
| `FileStream` | Bytes |
| `MemoryStream` | Flujo en memoria |

### Lectura y escritura de texto

```vb
Dim ruta As String = "C:\datos\nota.txt"

File.WriteAllText(ruta, "Hola mundo")
Dim contenido As String = File.ReadAllText(ruta)

File.AppendAllText(ruta, Environment.NewLine & "Segunda línea")
Dim lineas() As String = File.ReadAllLines(ruta)
```

### StreamReader / StreamWriter

```vb
Using lector As New StreamReader(ruta)
    Dim linea As String
    Do While (linea = lector.ReadLine()) IsNot Nothing
        Console.WriteLine(linea)
    Loop
End Using

Using escritor As New StreamWriter(ruta, append:=True)
    escritor.WriteLine("Nueva línea")
End Using
```

`Using` garantiza la llamada a `Dispose` aunque se produzca una excepción.

### Ficheros binarios

```vb
Dim datos() As Byte = File.ReadAllBytes("imagen.png")
File.WriteAllBytes("copia.png", datos)
```

### Directorios y rutas

```vb
Directory.CreateDirectory("C:\datos\log")
Dim existe As Boolean = File.Exists(ruta)
Dim ficheros() As String = Directory.GetFiles("C:\datos", "*.txt")

Dim completo As String = Path.Combine("C:\", "datos", "nota.txt")
Dim extension As String = Path.GetExtension(completo)
Dim nombre As String = Path.GetFileNameWithoutExtension(completo)
```

### Serialización JSON

```vb
Imports System.Text.Json

Dim json As String = JsonSerializer.Serialize(persona)
Dim p As Persona = JsonSerializer.Deserialize(Of Persona)(json)
```

### Métodos asíncronos

`File.ReadAllTextAsync`, `File.WriteAllTextAsync`, `StreamReader.ReadLineAsync`.

## Casos de Uso

- Persistencia de configuración y registros (*logs*) en disco.
- Lectura y escritura de ficheros de texto plano.
- Copia y movimiento de ficheros binarios.
- Recorrido de directorios para procesos por lotes.
- Serialización de objetos a JSON o desde JSON.
- En interfaces: E/S asíncrona para no bloquear el hilo de UI.

## Ejemplo de Código Mínimo

```vb
Imports System.IO

Module Modulo
    Sub Main()
        Dim carpeta As String = Path.Combine(Path.GetTempPath(), "vbdemo")
        Directory.CreateDirectory(carpeta)
        Dim ruta As String = Path.Combine(carpeta, "registro.log")

        Using escritor As New StreamWriter(ruta)
            escritor.WriteLine("Inicio de proceso")
            escritor.WriteLine(DateTime.Now.ToString("s"))
        End Using

        Using lector As New StreamReader(ruta)
            Dim linea As String
            Do While (linea = lector.ReadLine()) IsNot Nothing
                Console.WriteLine(linea)
            Loop
        End Using
    End Sub
End Module
```
