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

Abre las carpetas del proyecto (clic derecho → Show in Explorer).

🔹 PASO 4: Importar archivos

Extrae las texturas del archivo .zip dentro de la carpeta Textures.

Arrastra a Animations los archivos:

- Personaje_Unity.fbx
- Idle.fbx
- Running.fbx
- LeftStrafeWalk.fbx


Espera a que Unity genere los archivos .meta.

Si aparece el mensaje Fix Now de Normal Maps → haz clic en Fix Now.

🔹 PASO 5: Configurar tipo de animación y esqueleto

Selecciona todos los .fbx (Idle, Running, Left) excepto el modelo principal.

En el panel Inspector → Rig:

Animation Type: Humanoid

Avatar Definition: Copy From Other Avatar

Source: arrastra el modelo principal (Personaje_Unity.fbx)

Pulsa Apply.

🔹 PASO 6: Ajustar bucle y pose de animaciones

Selecciona cada animación (Idle, Running, Left).

En la pestaña Animation:

Activa Loop Time.

Marca Bake Into Pose en Root Transform Rotation, Position (Y), y Position (XZ).

En Based Upon, elige Original.

Haz clic en Apply en cada una.

🔹 PASO 7: Agregar personaje a la escena

Arrastra el archivo Personaje_Unity.fbx a la vista Scene.

Crea un suelo:

Clic derecho → 3D Object → Plane.

En el Inspector → Reset Transform.

Cambia el nombre del personaje a Player.

🔹 PASO 8: Agregar componentes de física

Con el objeto Player seleccionado:

Añade componente RigidBody.

Añade componente Capsule Collider.

Ajusta el collider:

Usa la vista lateral y ajusta para cubrir el cuerpo.

En el RigidBody → Freeze Rotation (X, Y, Z) ✅.

🔹 PASO 9: Crear el script de movimiento

En la carpeta Scripts:

Clic derecho → Create → C# Script → PlayerMove.cs.

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

🔹 PASO 10: Crear el Animation Controller

En carpeta Animations:

Clic derecho → Create → Animator Controller → AnimationPlayer.controller.

Abre el Animator (Window → Animation → Animator).

Clic derecho → Create State → From New Blend Tree.

Doble clic sobre el BlendTree.

🔹 PASO 11: Configurar el Blend Tree

En el Inspector:

Blend Type: 2D Freeform Directional.

En Parameters, crea dos variables tipo Float:

VelX

VelY

Asigna al BlendTree los parámetros VelX y VelY.

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

🔹 PASO 12: Asignar Animation Controller

Selecciona el objeto Player.

En el componente Animator, arrastra el AnimationPlayer.controller al campo Controller.

Arrastra también el Animator del Player al campo Animator del script PlayerMove.

🔹 PASO 13: Ajustar cámara

Crea un objeto Main Camera si no existe.

Posiciónala detrás del Player.

Arrastra la cámara dentro del objeto Player (para que lo siga al moverse).

🔹 PASO 14: Corrección de texturas transparentes (si ocurre)

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
