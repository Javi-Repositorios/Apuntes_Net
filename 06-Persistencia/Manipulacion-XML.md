# Manipulación XML en VB.NET

## Definición

.NET Framework ofrece varias API para leer, crear, modificar y transformar documentos XML, diferenciadas por su modelo de carga (DOM completo, LINQ sobre árbol o flujo secuencial).

## Sintaxis / Métodos

### API disponibles

| API | Namespace | Modelo |
|---|---|---|
| `XDocument` | `System.Xml.Linq` | LINQ to XML |
| `XmlDocument` | `System.Xml` | DOM |
| `XmlReader` / `XmlWriter` | `System.Xml` | Streaming |
| `XmlSerializer` | `System.Xml.Serialization` | Serialización de objetos |

### LINQ to XML: creación

```vb
Imports System.Xml.Linq

Dim doc As XDocument = New XDocument(
    New XElement("clientes",
        New XElement("cliente",
            New XAttribute("id", 1),
            New XElement("nombre", "Ana")),
        New XElement("cliente",
            New XAttribute("id", 2),
            New XElement("nombre", "Luis"))))

doc.Save("clientes.xml")
```

### LINQ to XML: consulta

```vb
Dim doc As XDocument = XDocument.Load("clientes.xml")

Dim nombres = From c In doc.Descendants("cliente")
              Select c.Element("nombre").Value

For Each n As String In nombres
    Console.WriteLine(n)
Next

Dim primero As String = doc.Root _
    .Elements("cliente") _
    .First(Function(c) CInt(c.Attribute("id")) = 1) _
    .Element("nombre").Value
```

### LINQ to XML: modificación

```vb
doc.Root.Add(New XElement("cliente",
    New XAttribute("id", 3),
    New XElement("nombre", "Marta")))
doc.Save("clientes.xml")
```

### XmlDocument (DOM)

```vb
Imports System.Xml

Dim doc As New XmlDocument()
doc.Load("clientes.xml")

Dim nodos As XmlNodeList = doc.SelectNodes("//cliente")
For Each nodo As XmlNode In nodos
    Console.WriteLine(nodo.SelectSingleNode("nombre").InnerText)
Next
```

### XmlReader (streaming)

```vb
Using lector As XmlReader = XmlReader.Create("grande.xml")
    While lector.Read()
        If lector.NodeType = XmlNodeType.Element AndAlso lector.Name = "cliente" Then
            Dim id As String = lector.GetAttribute("id")
        End If
    End While
End Using
```

### XmlSerializer

```vb
Imports System.Xml.Serialization

Dim serializador As New XmlSerializer(GetType(Persona))
Using flujo As IO.FileStream = IO.File.Create("persona.xml")
    serializador.Serialize(flujo, persona)
End Using
```

## Casos de Uso

- Ficheros de configuración y documentos de datos pequeños o medianos con `XDocument`.
- Consultas basadas en XPath sobre árboles XML con `XmlDocument`.
- Procesamiento de documentos grandes con memoria limitada mediante `XmlReader`/`XmlWriter`.
- Serialización y deserialización de objetos de dominio con `XmlSerializer`.
- Transformación de documentos XML mediante consultas LINQ.

## Ejemplo de Código Mínimo

```vb
Imports System.Xml.Linq

Module Modulo
    Sub Main()
        Dim doc As XDocument = New XDocument(
            New XElement("productos",
                New XElement("producto",
                    New XAttribute("id", 1),
                    New XElement("nombre", "Teclado"),
                    New XElement("precio", 25)),
                New XElement("producto",
                    New XAttribute("id", 2),
                    New XElement("nombre", "Ratón"),
                    New XElement("precio", 15))))

        doc.Save("productos.xml")

        Dim caros = From p In doc.Descendants("producto")
                    Where CInt(p.Element("precio")) > 20
                    Select p.Element("nombre").Value

        For Each nombre As String In caros
            Console.WriteLine(nombre)
        Next
    End Sub
End Module
```
