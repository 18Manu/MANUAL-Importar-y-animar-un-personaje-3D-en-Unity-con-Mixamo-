MANUAL: Importar y animar un personaje 3D en Unity (con Mixamo)

OBJETIVO

Importar un modelo 3D con animaciones desde Mixamo, integrarlo en Unity, y controlarlo con teclado usando código y un Animation Controller.

PASO 1: Crear cuenta y descargar personaje desde Mixamo

Entra a https://www.mixamo.com

Inicia sesión o crea una cuenta (correo, nombre, contraseña).
En el menú superior, selecciona “Characters”.
Escoge un personaje de la galería (cualquiera funciona).
Haz clic en Download.
Configura:
Format: Collada (.dae)
Pose: Da igual (A-T o T-Pose)
Pulsa Download y guarda el archivo.
<img width="1069" height="711" alt="image" src="https://github.com/user-attachments/assets/5bffc709-464b-4674-a270-f90d166571d4" />

Descarga el mismo personaje de nuevo pero ahora:
Format: FBX for Unity
Pose: Da igual (A-T o T-Pose)
<img width="1074" height="717" alt="image" src="https://github.com/user-attachments/assets/269ad4a6-e001-43d3-97eb-480a7627f3ee" />

Guarda ambos archivos.

PASO 2: Descargar animaciones desde Mixamo

Repite los siguientes pasos tres veces (una por animación):
Cambia a la pestaña Animations.
Busca las siguientes animaciones:
° Idle (personaje quieto)
° Running (correr)
° Left Strafe Walk (caminar a la izquierda)

En cada animación selecciona:

° Format: FBX for Unity
° Frames: 30 fps
° With Skin: ✅

Haz clic en Download y guarda las tres animaciones.
<img width="1470" height="767" alt="image" src="https://github.com/user-attachments/assets/7c31db6c-4f3f-4d83-a91b-f6b70898ba95" />
<img width="1476" height="759" alt="image" src="https://github.com/user-attachments/assets/c4d8564f-d083-47af-b9d7-2d12e241847f" />
<img width="1466" height="762" alt="image" src="https://github.com/user-attachments/assets/9a563bae-2bfc-4f3a-b17c-cdfb6f87b37a" />


Al final tendrás:
- Personaje_Collada.zip
- Personaje_Unity.fbx
- Idle.fbx
- Running.fbx
- LeftStrafeWalk.fbx
<img width="694" height="190" alt="image" src="https://github.com/user-attachments/assets/ed97c5db-3fd0-4084-99bc-ace8ac927371" />


PASO 3: Organizar proyecto en Unity

Abre Unity Hub → New Project → 3D Core Template.

Nómbralo “Personaje3D”.

En la ventana Project, crea carpetas:
° Animations
° Textures
° Scripts
<img width="804" height="388" alt="image" src="https://github.com/user-attachments/assets/0113b81a-062a-4358-976f-7e292a5d7d51" />

Abre las carpetas del proyecto (clic derecho → Show in Explorer).

PASO 4: Importar archivos

Extrae las texturas del archivo .zip dentro de la carpeta Textures.
<img width="1031" height="313" alt="image" src="https://github.com/user-attachments/assets/911458bd-198e-4bed-9b55-07fbfeb241b9" />

Arrastra a Animations los archivos:
- Personaje_Unity.fbx
- Idle.fbx
- Running.fbx
- LeftStrafeWalk.fbx
<img width="1033" height="315" alt="image" src="https://github.com/user-attachments/assets/e77acbe8-6cea-4446-b684-1b8b9fc31ee8" />

Espera a que Unity genere los archivos .meta.
<img width="540" height="437" alt="image" src="https://github.com/user-attachments/assets/48716144-1ede-45aa-9911-fcd11e4d2820" />

Si aparece el mensaje Fix Now de Normal Maps → haz clic en Fix Now.


PASO 5: Configurar tipo de animación y esqueleto

Selecciona todos los .fbx (Idle, Running, Left) excepto el modelo principal.

En el panel Inspector → Rig:
Animation Type: Humanoid
Avatar Definition: Copy From Other Avatar
Source: arrastra el modelo principal (Personaje_Unity.fbx)
<img width="463" height="477" alt="image" src="https://github.com/user-attachments/assets/0b2c5f30-bb7f-41ef-ab55-e566b003176d" />

Pulsa Apply.

PASO 6: Ajustar bucle y pose de animaciones

Selecciona cada animación (Idle, Running, Left).

En la pestaña Animation:

Activa Loop Time.
Marca Bake Into Pose en Root Transform Rotation, Position (Y), y Position (XZ).
En Based Upon, elige Original.
Haz clic en Apply en cada una.
<img width="453" height="673" alt="image" src="https://github.com/user-attachments/assets/3602c8a4-07ea-445d-a2e8-fe00fec828a3" />

PASO 7: Agregar personaje a la escena

Arrastra el archivo Personaje_Unity.fbx a la vista Scene.
Crea un suelo:
Clic derecho → 3D Object → Plane.
En el Inspector → Reset Transform.
<img width="1918" height="581" alt="image" src="https://github.com/user-attachments/assets/c785d2c9-6bae-4b65-95fb-5d60d251b31e" />

Cambia el nombre del personaje a Player.
<img width="454" height="543" alt="image" src="https://github.com/user-attachments/assets/3c78f16b-b073-4bad-9972-14f1edb80d31" />

PASO 8: Agregar componentes de física

Con el objeto Player seleccionado:

Añade componente RigidBody.
Añade componente Capsule Collider.
<img width="461" height="672" alt="image" src="https://github.com/user-attachments/assets/36eb0883-0fc5-40d7-88ba-2e24b19b12bc" />

Ajusta el collider:
Usa la vista lateral y ajusta para cubrir el cuerpo.
<img width="452" height="345" alt="image" src="https://github.com/user-attachments/assets/f8f92cf7-19a3-42c6-bbeb-ed23013916f1" />

En el RigidBody → Freeze Rotation (X, Y, Z).

<img width="466" height="385" alt="image" src="https://github.com/user-attachments/assets/617eab31-e819-4415-b6a4-19ee361af9db" />

PASO 9: Crear el script de movimiento

En la carpeta Scripts:

Clic derecho → Create → C# Script → Player.cs

Arrastra el script al objeto Player.

Abre el script y reemplaza todo con lo siguiente:

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMove : MonoBehaviour
{
    public float runSpeed = 7f;
    public float rotationSpeed = 250f;
    public Animator animator;

    private float x, y;

    void Update()
    {
        x = Input.GetAxis("Horizontal");
        y = Input.GetAxis("Vertical");

        // Rotación
        transform.Rotate(0, x * Time.deltaTime * rotationSpeed, 0);

        // Movimiento
        transform.Translate(0, 0, y * Time.deltaTime * runSpeed);

        // Pasar los valores al Animator
        animator.SetFloat("VelX", x);
        animator.SetFloat("VelY", y);
    }
}


Guarda (Ctrl + S) y vuelve a Unity.
<img width="1022" height="801" alt="image" src="https://github.com/user-attachments/assets/fb82719b-f095-4d08-8ee1-ff9b94b85bdc" />
<img width="455" height="258" alt="image" src="https://github.com/user-attachments/assets/ce14463f-ce69-4e9f-aa51-2c6ba50f6bc7" />


PASO 10: Crear el Animation Controller

En carpeta Animations:

° Clic derecho → Create → Animator Controller → AnimationPlayer.controller
<img width="889" height="375" alt="image" src="https://github.com/user-attachments/assets/d4e812e4-3e2c-4ff3-b0fd-a4cbaa689077" />

° Abre el Animator (Window → Animation → Animator).
° Clic derecho → Create State → From New Blend Tree.
<img width="1118" height="541" alt="image" src="https://github.com/user-attachments/assets/70568118-57b6-4869-9649-6d1b8a1cdc63" />
° Doble clic sobre el BlendTree.

🔹 PASO 11: Configurar el Blend Tree

En el Inspector:

Blend Type: 2D Freeform Directional.
<img width="460" height="226" alt="image" src="https://github.com/user-attachments/assets/b067c280-81a1-4dce-918f-3d833de74d4c" />

En Parameters, crea dos variables tipo Float:
VelX
VelY

Asigna al BlendTree los parámetros VelX y VelY.
<img width="1732" height="542" alt="image" src="https://github.com/user-attachments/assets/2f069737-e0e8-4d83-987e-743abe58e0ce" />

En Motions, añade 7 animaciones (Add Motion Field 7 times).

Asigna posiciones:

(0,0) → Idle
(0,1) → Running
(0,-1) → Running (Speed -1)
(1,0) → Left (Mirror ✅)
(-1,0) → Left
(1,1) → Running (Mirror ✅)
(-1,-1) → Running

Activa Mirror donde sea necesario para invertir el movimiento.
<img width="449" height="308" alt="image" src="https://github.com/user-attachments/assets/13be68fc-f396-4551-be44-8489ab7904a5" />

PASO 12: Asignar Animation Controller

Selecciona el objeto Player.
En el componente Animator, arrastra el AnimationPlayer.controller al campo Controller.
Arrastra también el Animator del Player al campo Animator del script PlayerMove.
<img width="461" height="579" alt="image" src="https://github.com/user-attachments/assets/c2dafdce-b0cc-4679-a971-d1262c3b3816" />

PASO 13: Ajustar cámara

Crea un objeto Main Camera si no existe.
Posiciónala detrás del Player.
Arrastra la cámara dentro del objeto Player (para que lo siga al moverse).
<img width="428" height="230" alt="image" src="https://github.com/user-attachments/assets/2c684121-6103-48de-a23a-8ed30033a2e6" />

PASO 14: Corrección de texturas transparentes (si ocurre)

Si tu modelo se ve transparente:
En el Project, busca el Material del personaje.
Presiona Ctrl + D para duplicarlo.
En el material duplicado → cambia:
Rendering Mode: Opaque
Asigna este material nuevo al Skinned Mesh Renderer del Player.

🟢 PRUEBA FINAL

Pulsa Play ▶️
Usa W / S para moverte adelante / atrás.
Usa A / D para girar.

Verás cómo el personaje:
Se anima con Idle al estar quieto.
Corre con Running.
Gira con Left/Right.
Se anima con Idle al estar quieto.

Corre con Running.

Gira con Left/Right.
