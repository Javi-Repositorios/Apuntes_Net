# Eventos y Delegados en VB.NET

## Definición

Un delegado es un tipo que representa referencias a métodos con una firma determinada. Un evento es un miembro que expone un delegado para notificar a los suscriptores la ocurrencia de una acción, implementando el patrón observador.

## Sintaxis / Métodos

### Declaración de delegados

```vb
Public Delegate Function Operacion(a As Integer, b As Integer) As Integer

Module Modulo
    Function Sumar(a As Integer, b As Integer) As Integer
        Return a + b
    End Function

    Sub Main()
        Dim op As Operacion = AddressOf Sumar
        Console.WriteLine(op(3, 4))
    End Sub
End Module
```

### Delegados incorporados

```vb
Dim log As Action(Of String) = Sub(texto) Console.WriteLine(texto)
Dim suma As Func(Of Integer, Integer, Integer) = Function(a, b) a + b
Dim esPar As Predicate(Of Integer) = Function(n) n Mod 2 = 0
```

### Declaración de eventos

```vb
Public Class Termometro
    Public Event TemperaturaCambiada(sender As Object, valor As Decimal)

    Private _temperatura As Decimal
    Public Property Temperatura As Decimal
        Get
            Return _temperatura
        End Get
        Set(value As Decimal)
            _temperatura = value
            RaiseEvent TemperaturaCambiada(Me, value)
        End Set
    End Property
End Class
```

### Suscripción con AddHandler / RemoveHandler

```vb
Dim t As New Termometro()
AddHandler t.TemperaturaCambiada, AddressOf AlCambiar
RemoveHandler t.TemperaturaCambiada, AddressOf AlCambiar

Sub AlCambiar(sender As Object, valor As Decimal)
    Console.WriteLine("Ahora: " & valor)
End Sub
```

### Suscripción con WithEvents y Handles

```vb
Public WithEvents btn As Button

Private Sub btn_Click(sender As Object, e As EventArgs) Handles btn.Click
    MessageBox.Show("Pulsado")
End Sub
```

### Eventos con EventHandler(Of T)

```vb
Public Event Cambio As EventHandler(Of EventArgs)
```

## Casos de Uso

- Notificar cambios de estado sin acoplar el emisor a los receptores.
- Manejar la interacción de controles de Windows Forms.
- Inyectar callbacks y estrategias mediante `Action`/`Func`.
- Implementar patrones observador y publicador/suscriptor.
- Ejecutar lógica al finalizar procesos (eventos de finalización).

## Ejemplo de Código Mínimo

```vb
Public Class Contador
    Public Event AlLlegarA(limite As Integer)

    Private _valor As Integer
    Public Property Valor As Integer
        Get
            Return _valor
        End Get
        Set(nuevo As Integer)
            _valor = nuevo
            If _valor >= 5 Then
                RaiseEvent AlLlegarA(_valor)
            End If
        End Set
    End Property
End Class

Module Modulo
    Sub Main()
        Dim c As New Contador()
        AddHandler c.AlLlegarA, Sub(limite)
                                    Console.WriteLine("Límite alcanzado: " & limite)
                                End Sub

        For i As Integer = 1 To 6
            c.Valor = i
        Next
    End Sub
End Module
```
