# Documentación de Become iOS SDK

Proceso de instalación de la librería Become iOS SDK.

---

## Agregar licencia al proyecto

1. Agregar archivos de licencia:

   - **com.become.document.key.txt**: Llaves de inicialización para la captura de documentos.
   - **com.become.finger.lic**: Llaves de inicialización para la captura de huellas.

Estos archivos deben estar incluidos en los recursos del proyecto.

2. El [`Bundle Identifier`](https://developer.apple.com/documentation/appstoreconnectapi/bundle_ids) del proyecto debe coincidir con la licencia asignada al cliente.

---

## Agregar Framework al proyecto

Se debe agregar el archivo **BecomeDigitalV.xcframework** en las configuraciones generales del proyecto en la sección **Frameworks, Libraries, and Embedded Content**.

Además, para el correcto funcionamiento de la SDK, se requiere el uso de los siguientes frameworks/librerías:

- `CaptureUX.xcframework`
- `CaptureCore.xcframework`

Estos deben adicionarse en la sección mencionada.

<p align="center">
<img src="https://github.com/Becomedigital/BecomeDigitalSDKAutDirectPro/blob/main/IMG_2.png">
</p>

### Instalación mediante CocoaPods

Agregar el siguiente contenido en su `Podfile`:

```ruby
platform :ios, '15.6'

plugin 'cocoapods-art', :sources => [
  'cocoapods-identy-finger'
]

target 'BecomeApp' do
  use_frameworks!

  pod 'Identy', '6.3.0'
end
```

**Instalación de cocoapods-art:**

```bash
sudo gem install cocoapods-art
```

**Nota:** Las credenciales para acceder al repositorio privado serán proporcionadas directamente por Become.

### Integración con Swift Package Manager

Es necesario agregar los siguientes paquetes para completar la configuración de la SDK:

1. Abra su proyecto en Xcode y vaya a **File > Add Packages**.

   ![Agregar dependencia del paquete](https://github.com/user-attachments/assets/f845c6f2-d235-43a8-a1e3-a796cc1426a4)

2. En la barra de búsqueda, ingrese la URL del repositorio de la biblioteca Amplify:  
   `https://github.com/aws-amplify/amplify-swift`

3. Seleccione la opción **Up to Next Major Version** y especifique **2.0.0** como versión mínima. Luego, haga clic en **Add Package**.

   ![Opciones de versión de dependencia](https://github.com/aws-amplify/amplify-swift/blob/main/readme-images/spm-setup-02-amplify-repo-options.png)

4. Seleccione las bibliotecas necesarias según sus requerimientos:
   - **Amplify** (Obligatorio)

   ![Seleccionar dependencias](https://github.com/aws-amplify/amplify-swift/blob/main/readme-images/spm-setup-03-select-dependencies.png)

5. Repita los pasos anteriores para agregar el siguiente paquete:
   - `https://github.com/aws-amplify/amplify-ui-swift-liveness`

---

## Configuraciones dentro de info.plist

Debe agregarse la descripción de uso de la cámara y del reconocimiento de voz:

```xml
<key>NSCameraUsageDescription</key>
<string>Esta aplicación hace uso de tu cámara</string>

<key>NSSpeechRecognitionUsageDescription</key>
<string>Esta aplicación necesita acceso al reconocimiento de voz para procesar comandos o analizar el audio del usuario.</string>
```

---

## Inicialización de la SDK

**1. Importar el SDK en su ViewController:**

```swift
import BDIdentityVerification
```

**2. Inicializar Become SDK:**

> ⚠️ **Nota importante:** Recuerde reemplazar los valores `<TU_CLIENT_ID>`, `<TU_CLIENT_SECRET>` y `<TU_CONTRACT_ID>` por los datos reales proporcionados para su integración.

```swift
@IBAction func startSDKAction(_ sender: Any) {

    let bdivConfig = BDIVConfig(
        clienId: "<TU_CLIENT_ID>",
        clientSecret: "<TU_CLIENT_SECRET>",
        contractId: "<TU_CONTRACT_ID>",
        documenTypes: [.DNI],
        userId: "<TU_USER_ID>",
        customerLogo: "becomeIcon",
        customLocalizationFileName: "Localizable"
    )

    let identityVerification = BecomeDigitalSDK(bdivConfig: bdivConfig, delegate: self)
    identityVerification.startVerification()
}
```

**Importante:**
- El parámetro `customLocalizationFileName` debe apuntar a un archivo **Localizable.strings** donde se encuentran todas las llaves de los textos a localizar.

**3. Delegado de la SDK:**

```swift
extension ViewController: BDIVDelegate {
    func BDIVResponseSuccess(bdivResult: AnyObject) {
        let idmResultFinal = bdivResult as! ResponseIV
        print(String(describing: idmResultFinal))
    }

    func BDIVResponseError(error: String) {
        print(error)
    }
}
```

---

## Respuesta de la SDK

La SDK proporcionará respuestas a través de los métodos del protocolo **BDIVDelegate**:

```swift
func BDIVResponseSuccess(bdivResult: AnyObject) {
    let idmResultFinal = bdivResult as! ResponseIV
    print(String(describing: idmResultFinal))
}

func BDIVResponseError(error: String) {
    print(error)
}
```

Los parámetros disponibles en el objeto de respuesta incluyen:

```swift
public var message: String = ""
public var responseURL: String = ""
public var responseStatus: typeEstatus = .PENDING

enum typeEstatus {        
  case SUCCES
  case ERROR
  case PENDING
}
```

---

## Posibles Errores

**Error por parámetros vacíos**

Es necesario que los siguientes parámetros tengan valor:

Parámetro | Valor
------------ | -------------
clientSecret | ""
clientID | ""
documenTypes | ""
contractID | ""
userID  | ""

Error mostrado:

```bash
parameters cannot be empty
```

---

## Localización

Descargue o cree el archivo `Localizable.strings` donde agregará los textos personalizados que usará la SDK. Este archivo se indica en el parámetro `customLocalizationFileName`.

---

## Requerimientos

- iOS 12 o superior

---

# ¡Listo!

Su SDK está lista para ser utilizada con la integración Become Digital.

