<a name="Title"></a>
# Getting started building games with Unity in Visual Studio for Mac #

<a name="Overview"></a>
## Overview ##

Unity is a game engine that enables you to develop games in C#. In this lab, we'll show how to get started developing and debugging Unity games using Visual Studio for Mac and the Visual Studio for Mac Tools for Unity extension alongside the Unity environment.

Visual Studio for Mac Tools for Unity is a free extension (installed with Visual Studio for Mac) that enables Unity developers to take advantage of the productivity features of Visual Studio for Mac, including excellent IntelliSense support, debugging features, and more.

<a name="Objectives"></a>
## Objectives ##

- Learn about Unity development with Visual Studio for Mac

<a name="Prerequisites"></a>
## Prerequisites ##

- Visual Studio for Mac ([https://www.visualstudio.com/vs/visual-studio-mac](https://www.visualstudio.com/vs/visual-studio-mac))
- Unity 5.6.1 Personal Edition or higher ([https://store.unity.com](https://store.unity.com/), requires a unity.com account to run)

<a name="Intended Audience"></a>
## Intended Audience ##

This lab is intended for developers who are familiar with C#, although deep experience is not required.

<a name="Exercise1"></a>
## Exercise 1: Getting started building games with Unity in Visual Studio for Mac ##

<a name="Ex1Task1"></a>
### Task 1: Creating a basic Unity project ###

1. Launch **Unity**. Sign in if requested.

1. Click **New**.

    ![](https://user-images.githubusercontent.com/3944468/27367671-4b1a66ba-5603-11e7-94b9-4cbbc3011db1.png)

1. Set the **Project name** to **"UnityLab"** and select **3D**. Click **Create project**.

    ![](https://user-images.githubusercontent.com/3944468/27367687-655e7fde-5603-11e7-89fb-32c1fd477777.png)

1. You are now looking at the default Unity interface. It has the scene hierarchy with game objects on the left, a 3D view of the blank scene shown in the middle, a project files pane on the bottom, and inspector and services on the right. Of course, there's a lot more to it than that, but those are few of the more important components.

    ![](https://user-images.githubusercontent.com/3944468/27367714-742945e4-5603-11e7-9e8a-ec22dbae841d.png)

1. For those new to Unity, everything that runs in your app will exist within the context of a **scene**. A scene file is a single file that contains all sorts of metadata about the resources used in the project for the current scene and its properties. When you package your app for a platform, the resulting app will end up being a collection of one or more scenes, plus any platform-dependent code you add. You can have as many scenes as desired in a project.

1. The new scene just has a camera and a directional light in it. A scene requires a **camera** for anything to be visible and an **Audio Listener** for anything to be audible. These components are attached to a **GameObject**.

1. Select the **Main Camera** object from the **Hierarchy** pane.

    ![](https://user-images.githubusercontent.com/3944468/27367715-742c97da-5603-11e7-8f01-5aab97c8414e.png)

1. Select the **Inspector** pane from the right side of the window to review its properties. Note that camera properties include transform information, background, projection type, field of view, and so on. Note that an Audio Listener component was also added by default, which essentially renders scene audio from a virtual microphone attached to the camera.

    ![](https://user-images.githubusercontent.com/3944468/27367720-743dc46a-5603-11e7-9bf2-88d13fc8f581.png)

1. Select the **Directional Light** object. This provides light to the scene so that components like shaders know how to render objects.

    ![](https://user-images.githubusercontent.com/3944468/27367716-742ea6b0-5603-11e7-905b-c6c382359b1e.png)

1. Use the **Inspector** to see that it includes common lighting properties including type, color, intensity, shadow type, and so on.

    ![](https://user-images.githubusercontent.com/3944468/27367718-7435e682-5603-11e7-8dc9-94daef4a26f9.png)

1. It is important to point out that projects in Unity are a little different from their Visual Studio for Mac counterparts. In the **Project** tab on the bottom, right-click the **Assets** folder and select **Reveal in Finder**.

    ![](https://user-images.githubusercontent.com/3944468/27367717-74340128-5603-11e7-933f-bf8f22123882.png)

1. Projects contain **Assets**, **Library**, **ProjectSettings**, and **Temp** folders as you can see. However, the only one that shows up in the interface is the **Assets** folder. The **Library** folder is the local cache for imported assets; it holds all metadata for assets. The **ProjectSettings** folder stores settings you can configure. The **Temp** folder is used for temporary files from Mono and Unity during the build process. There is also a solution file that you can open in Visual Studio for Mac (**UnityLab.sln** here).

    ![](https://user-images.githubusercontent.com/3944468/27367719-743da930-5603-11e7-861d-9c35ccd57d6d.png)

1. Close the **Finder** window and return to **Unity**.

1. The **Assets** folder contains all your assets-art, code, audio, etc. It's empty now, but every single file you bring into your project goes here. This is always the top-level folder in the **Unity Editor**. But always add and remove files via the Unity interface (or Visual Studio for Mac) and never through the file system directly.

    ![](https://user-images.githubusercontent.com/3944468/27367721-7441d000-5603-11e7-8563-ceb06418eaa8.png)

1. The **GameObject** is central to development in Unity as almost everything derives from that type, including models, lights, particle systems, and so on. Add a new **Cube** object to the scene via the **GameObject > 3D Object > Cube** menu.

    ![](https://user-images.githubusercontent.com/3944468/27367722-7444cefe-5603-11e7-8648-f4310ece3356.png)

1. Take a quick look at the properties of the new **GameObject** and note that it has a name, tag, layer, and transform. These properties are common to all **GameObjects**. In addition, several components were attached to the **Cube** to provide needed functionality including mesh filter, box collider, and renderer.

    ![](https://user-images.githubusercontent.com/3944468/27367723-7448dd5a-5603-11e7-8ea9-c83b0870b243.png)

1. Rename the **Cube** object, which has the name **"Cube"** by default, to **"Enemy"**. Make sure to press **Enter** to save the change. This will be the enemy cube in our simple game.

    ![](https://user-images.githubusercontent.com/3944468/27367724-744df5ba-5603-11e7-9eeb-b258745395a4.png)

1. Add another **Cube** object to the scene using the same process as above, and name this one **"Player"**.

    ![](https://user-images.githubusercontent.com/3944468/27367729-7466d7a6-5603-11e7-90f4-088458e62c7d.png)

1. Tag the player object **"Player"** as well (see **Tag** drop-down control just under name field). We'll use this in the enemy script to help locate the player game object.

    ![](https://user-images.githubusercontent.com/3944468/27367727-7465864e-5603-11e7-9513-00148c5c46e2.png)

1. In the **Scene** view, move the player object away from the enemy object along the Z axis using the mouse. You can move along the Z axis by selecting and dragging the cube by its **red** panel toward the **blue** line. Since the cube lives in 3D space, but can only be dragged in 2D each time, the axis on which you drag is especially important.

    ![](https://user-images.githubusercontent.com/3944468/27367730-746ba59c-5603-11e7-8fcd-697d7642b646.png)

1. Move the cube downward and to the right along the axis. Note that this updates the **Transform.Position** property in the **Inspector**. Please be sure to drag to a location similarly to what's shown here to make later steps easier in the lab.

    ![](https://user-images.githubusercontent.com/3944468/27367725-7459e780-5603-11e7-8010-7e67be8aeaa8.png)

1. Now you can add some code to drive the enemy logic so that it pursues the player. Right-click the **Assets** folder in the **Project** pad and select **Create > C# Script**.

    ![](https://user-images.githubusercontent.com/3944468/27367726-7461cd6a-5603-11e7-89d0-145972b0ab76.png)

1. Name the new C# script **"EnemyAI"**.

    ![](https://user-images.githubusercontent.com/3944468/27367728-7465f1f6-5603-11e7-8895-b284d512953a.png)

1. Attaching scripts to game objects is as easy as dragging and dropping them onto the appropriate object in the **Hierarchy** pane. To do this, drag the newly created script onto the **Enemy** object in the **Hierarchy** pane. Now that object will use behaviors from this script.

    ![](https://user-images.githubusercontent.com/3944468/27367731-746e85dc-5603-11e7-8afc-5b1896091eec.png)

1. Select **File > Save Scenes** to save the current scene. Name it **"MyScene"**.

<a name="Ex1Task2"></a>
### Task 2: Working with Visual Studio for Mac Tools for Unity ###

1. The best way to edit C# code is to use Visual Studio for Mac. You can configure Unity to use Visual Studio for Mac as its default handler. Select **Unity > Preferences**.

1. Select the **External Tools** tab. From the **External Script Editor** dropdown, select **Browse** and select **Applications/Visual Studio.app**. Alternatively, if there's already a **Visual Studio** option, just select that.

    ![](https://user-images.githubusercontent.com/3944468/27367732-747808fa-5603-11e7-80cf-6a5b49143fd4.png)

1. Unity is now configured to use **Visual Studio for Mac** for script editing. Close the **Unity Preferences** dialog.

    ![](https://user-images.githubusercontent.com/3944468/27367733-747bd1d8-5603-11e7-9531-deb9df5d4cd2.png)

1. Double-click **EnemyAI.cs** to open it in **Visual Studio for Mac**.

    ![](https://user-images.githubusercontent.com/3944468/27367734-747ea142-5603-11e7-9087-aeb56e8e078b.png)

1. The Visual Studio solution is straightforward. It contains an **Assets** folder (the same one from **Finder**) and the **EnemyAI.cs** script created earlier. In more sophisticated projects, the hierarchy will likely look different than what you see in Unity.

    ![](https://user-images.githubusercontent.com/3944468/27367735-747ff420-5603-11e7-8269-c504a5558363.png)

1. **EnemyAI.cs** is open in the editor. The initial script just contains stubs for the **Start** and **Update** methods.

1. Replace the initial enemy code with the code below.

    ```csharp
    public class EnemyAI : MonoBehaviour
    {
        public float Speed = 50;
        private Transform _playerTransform;
        private Transform _myTransform;
        
        void Start()
        {
            var player = GameObject.FindGameObjectWithTag("Player");
            if (!player)
            {
                Debug.LogError(
                  "Could not find the main player. Ensure it has the player tag set.");
            }
            else
            {
                _playerTransform = player.transform;
            }
            _myTransform = this.transform;
        }
    
        void Update()
        {
            var moveAmount = Speed * Time.deltaTime;
            _myTransform.position = Vector3.MoveTowards(_myTransform.position,
                _playerTransform.position, moveAmount);
    
            if (_myTransform.position == _playerTransform.position)
            {
                _myTransform.position = Vector3.back * 10;
            }
        }
    }
    ```

1. Take a quick look at the simple enemy behavior that is defined here. In the **Start** method, we get a reference to the player object (by its tag), as well as its **transform**. In the **Update** method, which is called every frame, the enemy will move towards the player object. Note how the keywords and names use color coding to make it easier to understand the codebase in Visual Studio for Mac.

1. Save the changes to the enemy script in **Visual Studio for Mac**.

<a name="Ex1Task3"></a>
### Task 3: Debugging the Unity project ###

1. Set a breakpoint on the first line of code in the **Start** method. You can either click in the editor margin at the target line or place cursor on the line and press **F9**.

    ![](https://user-images.githubusercontent.com/3944468/27367737-74832e4c-5603-11e7-8aa4-a4f57c62f01b.png)

1. Click the **Start Debugging** button or press **F5**. This will build the project and attach it to Unity for debugging.

    ![](https://user-images.githubusercontent.com/3944468/27367736-74818272-5603-11e7-8311-6244bd76ed17.png)

1. Return to **Unity** and click the **Run** button to start the game.

    ![](https://user-images.githubusercontent.com/3944468/27367739-74911f66-5603-11e7-8bc7-cd393112925f.png)

1. The breakpoint should be hit and you can now use the Visual Studio for Mac debugging tools.

    ![](https://user-images.githubusercontent.com/3944468/27367740-7491f71a-5603-11e7-9c9f-4b490538dab0.png)

1. From the **Locals** pad, locate the **this** pointer, which references an **EnemyAI** obect. Expand the reference and note that you can browse the associated members like **Speed**.

    ![](https://user-images.githubusercontent.com/3944468/27367702-73eff636-5603-11e7-9fd5-9fe7528a6e66.png)

1. Remove the breakpoint from the **Start** method the same way it was added-by either clicking it in the margin or selecting the line and press **F9**.

    ![](https://user-images.githubusercontent.com/3944468/27367697-73e7b552-5603-11e7-8eb2-76309bcbc973.png)

1. Press **F10** to step over the first line of code that finds the **Player** game object using a tag as parameter.

1. Hover the mouse cursor over the **player** variable within the code editor window to view its associated members. You can even expand the overlay to view child properties.

    ![](https://user-images.githubusercontent.com/3944468/27367698-73e88540-5603-11e7-8a09-72f905de25d6.png)

1. Press **F5** or press the **Run** button to continue execution. Return to Unity to see the enemy cube repeatedly approach the player cube. Note that you may need to adjust the camera if it's not visible.

    ![](https://user-images.githubusercontent.com/3944468/27367701-73eb8420-5603-11e7-8eca-e59f94ca836e.png)

1. Switch back to **Visual Studio for Mac** and set a breakpoint on the first line of the **Update** method. It should be hit immediately.

    ![](https://user-images.githubusercontent.com/3944468/27367699-73e88004-5603-11e7-8d92-5ac233341005.png)

1. Suppose the speed is too fast and we want to test the impact of the change without restarting the app. Locate the **Speed** variable within the **Autos** or **Locals** window and then change it to **"10"** and press **Enter**.

    ![](https://user-images.githubusercontent.com/3944468/27367700-73e905f6-5603-11e7-9ef0-ac584b61f1b6.png)

1. Remove the breakpoint and press **F5** to resume execution.

1. Return to **Unity** to view the running application and note that the enemy cube is now moving at a fifth of the original speed.

    ![](https://user-images.githubusercontent.com/3944468/27367705-740373f0-5603-11e7-84bc-e9add56730f5.png)

1. Stop the Unity app by clicking the **Play** button again.

    ![](https://user-images.githubusercontent.com/3944468/27367704-73fead8e-5603-11e7-8242-8191775f2de8.png)

1. Return to **Visual Studio for Mac**. Stop the debugging session by clicking the **Stop** button.

    ![](https://user-images.githubusercontent.com/3944468/27367709-7414fb8e-5603-11e7-8abe-2a306ff79f64.png)

<a name="Ex1Task4"></a>
### Task 4: Exploring Unity features in Visual Studio for Mac ###

1. Visual Studio for Mac provides quick access to Unity documentation within the code editor. Place the cursor somewhere on the **Vector3** symbol within the **Update** method and press **⌘ Command + '**.

    ![](https://user-images.githubusercontent.com/3944468/27367703-73fe672a-5603-11e7-9688-2e7f163aaaf1.png)

1. This will open a new browser window to the documentation for **Vector3**. Close the browser window when satisfied.

    ![](https://user-images.githubusercontent.com/3944468/27367706-74052ab0-5603-11e7-8ab1-63ec70359cf7.png)

1. Visual Studio for Mac also provides some helpers to quickly create Unity behavior classes. From **Solution Explorer**, right-click **Assets** and select **Add > New MonoBehaviour**.

    ![](https://user-images.githubusercontent.com/3944468/27367707-7407c676-5603-11e7-8481-868329bf1dc4.png)

1. The newly created class provides stubs for the **Start** and **Update** methods. After the closing brace of the **Update** method, start typing **"onmouseup"**. As you type, notice that Visual Studio's IntelliSense quickly zeros in on the method you're planning to implement. Select it from the provided autocomplete list. It will fill out a method stub for you, including any parameters.

    ![](https://user-images.githubusercontent.com/3944468/27367708-7412b5b8-5603-11e7-8a3e-34983ca92f5b.png)

1. Inside the **OnMouseUp** method, type **"base."** to see all of the base methods available to call. You can also explore the different overloads of each function using the paging option in the top right corner of the IntelliSense flyout.

    ![](https://user-images.githubusercontent.com/3944468/27367710-74155340-5603-11e7-960b-ff9baa4fe90f.png)

1. Visual Studio for Mac also enables you to easily define new shaders. From **Solution Explorer**, right-click **Assets** and select **Add > New Shader**.

    ![](https://user-images.githubusercontent.com/3944468/27367711-7417bebe-5603-11e7-8e8c-7dabdd62ba9b.png)

1. The shader file format gets full color and font treatment to make it easier to read and understand.

    ![](https://user-images.githubusercontent.com/3944468/27367712-741c4efc-5603-11e7-8f58-b4a241d9c3f4.png)

1. Return to **Unity**. You'll see that since Visual Studio for Mac works with the same project system, changes made in either place are automatically synchronized with the other. Now it's easy to always use the best tool for the task.

    ![](https://user-images.githubusercontent.com/3944468/27367713-741e0242-5603-11e7-851a-9c9635bdba4e.png)

<a name="Summary"></a>
## Summary ##

In this lab, you've learned how to get started creating a game with Unity and Visual Studio for Mac. Please visit [https://unity3d.com/learn](https://unity3d.com/learn) to learn more about Unity.

