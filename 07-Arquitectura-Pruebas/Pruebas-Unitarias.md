# Pruebas Unitarias en VB.NET

## Definición

Una prueba unitaria verifica el comportamiento de una unidad de código de forma aislada y automática. Se ejecuta sobre un marco de pruebas y proporciona detección temprana de regresiones.

## Sintaxis / Métodos

### Marcos de pruebas

| Marco | Uso |
|---|---|
| MSTest | Marco oficial (`Microsoft.VisualStudio.TestTools.UnitTesting`) |
| NUnit | Marco maduro con amplio conjunto de aserciones |
| xUnit | Marco para .NET moderno |
| Moq / NSubstitute | Creación de dobles de prueba |
| FluentAssertions | Aserciones encadenadas |

### Proyecto de pruebas (línea de comandos)

```text
dotnet new mstest -n Ventas.Pruebas
dotnet add reference ..\Ventas.Negocio\Ventas.Negocio.vbproj
dotnet test
```

### Estructura Arrange-Act-Assert (MSTest)

```vb
Imports Microsoft.VisualStudio.TestTools.UnitTesting

<TestClass>
Public Class ClienteServicioTests

    <TestMethod>
    Public Sub Registrar_NombreVacio_LanzaExcepcion()
        ' Arrange
        Dim repositorio As New RepositorioFalso()
        Dim servicio As New ClienteServicio(repositorio)
        Dim cliente As New Cliente With {.Nombre = ""}

        ' Act y Assert
        Assert.ThrowsException(Of ArgumentException)(
            Sub() servicio.Registrar(cliente))
    End Sub

End Class
```

### Aserciones habituales

```vb
Assert.IsTrue(condicion)
Assert.AreEqual(esperado, real)
Assert.IsNull(objeto)
Assert.IsNotNull(objeto)
Assert.ThrowsException(Of InvalidOperationException)(Sub() accion())
```

### Pruebas parametrizadas (MSTest)

```vb
<TestMethod>
<DataRow(2, 3, 5)>
<DataRow(-1, 1, 0)>
Public Sub Sumar_DevuelveResultado(a As Integer, b As Integer, esperado As Integer)
    Assert.AreEqual(esperado, Calculadora.Sumar(a, b))
End Sub
```

### Pruebas con dobles (Moq)

```vb
Imports Moq

<TestMethod>
Public Sub Obtener_DevuelveClienteDelRepositorio()
    Dim mock As New Mock(Of IClienteRepositorio)()
    mock.Setup(Function(r) r.Obtener(1)) _
        .Returns(New Cliente With {.Id = 1, .Nombre = "Ana"})

    Dim servicio As New ClienteServicio(mock.Object)
    Dim cliente As Cliente = servicio.Obtener(1)

    Assert.AreEqual("Ana", cliente.Nombre)
    mock.Verify(Function(r) r.Obtener(1), Times.Once())
End Sub
```

### Preparación y limpieza

```vb
<TestInitialize>
Public Sub Inicializar()
End Sub

<TestCleanup>
Public Sub Limpiar()
End Sub
```

## Casos de Uso

- Verificación de reglas de negocio de la capa de servicio.
- Comprobación de límites y validación de argumentos.
- Pruebas de conversiones, cálculos y transformaciones de datos.
- Aislamiento de dependencias externas mediante dobles de prueba.
- Ejecución automática en integración continua con `dotnet test`.

## Ejemplo de Código Mínimo

```vb
Imports Microsoft.VisualStudio.TestTools.UnitTesting

Public Class Calculadora
    Public Shared Function Dividir(a As Integer, b As Integer) As Integer
        If b = 0 Then Throw New DivideByZeroException()
        Return a \ b
    End Function
End Class

<TestClass>
Public Class CalculadoraTests

    <TestMethod>
    Public Sub Dividir_ValoresValidos_DevuelveCociente()
        Assert.AreEqual(5, Calculadora.Dividir(10, 2))
    End Sub

    <TestMethod>
    Public Sub Dividir_DivisorCero_LanzaExcepcion()
        Assert.ThrowsException(Of DivideByZeroException)(
            Sub() Calculadora.Dividir(10, 0))
    End Sub

End Class
```
