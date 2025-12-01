### Vista de información de la comarca

La vista de información de la comarca tenía la peculiaridad de que se trataba de un widget con estado, ya que necesita mantener el índice de la pestaña que se está visualizando.

Con el fin de instanciar la clase, se proporcionaba el nombre la comarca en el constructor:

```dart
class InfoComarcaScreen extends StatefulWidget {

  const InfoComarcaScreen({super.key, required this.nomComarca});

  final String nomComarca;

  @override
  State<InfoComarcaScreen> createState() => _InfoComarcaScreenState();
}
```

Ahora en el método `build`, **pero en este caso del estado**, obtendremos la referencia al `ProviderComarcas`, y lo primero que haremos será actualizar la comarca actual por aquella que se ha proporcionado al widget:

```dart
@override
Widget build(BuildContext context) {
    var comarcasProvider = Provider.of<ComarcasProvider>(context);
    comarcasProvider.nomComarcaActual = widget.nomComarca;
    ...
    }
```

Este `build` nos devolvía un `Scaffold`, con toda la estructura de la barra inferior de navegación. En su `body` especificábamos una lista de widgets, para seleccionar lo que se correspondiera en el `index`:

```dart
   body: <Widget>[
      InfoComarcaGeneral(),
      InfoComarcaDetall()][indexPantallaActual]
    ...
```

Pasamos ahora a la implementación de este primer widget `InfoComargaGeneral()`.

Este widget, como el resto, importa las librerías necesarias del *Provider*, y en su método `build` obtiene la instancia del `ComarcasProvider`. Esta instancia se utilizará para obtener la información de la comarca, y además, ser notificada cuando cambie, en cuyo caso, redibujará el widget. El código resultante (con algunas partes omitidas), quedará de la siguiente forma:

```dart
class InfoComarcaGeneral extends StatelessWidget {
  const InfoComarcaGeneral({super.key});

// Ahora no recibimos la comarca como argumento...

@override
  Widget build(BuildContext context) {
    // Creamos una instancia del Provider
    var comarcasProvider = Provider.of<ComarcasProvider>(context);

    if (comarcasProvider.comarcaActual == null) {
      return const Center(child: CircularProgressIndicator());
    } else {
      return SingleChildScrollView(
        child: Column(...
          children: [
            Image.network(comarcasProvider.comarcaActual?.img ?? ""),
            ...
            Text(comarcasProvider.comarcaActual?.comarca ?? ""),
            ...
            Text('Capital: ${comarcasProvider.comarcaActual?.capital ?? ""}',...),
            ...
            Text(comarcasProvider.comarcaActual?.desc ?? "",...)
                ],
              ),
            ),
      ...
    }
  }
```

Dejamos como tarea la adaptación de la vista del tiempo.