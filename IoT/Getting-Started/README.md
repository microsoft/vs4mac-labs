<a name="Title"></a>
# Getting started targeting IoT devices in Visual Studio for Mac (Preview) #

<a name="Overview"></a>
## Overview ##

In this lab, we'll show what it takes to get started targeting IoT devices using Visual Studio for Mac. While we'll be using a Raspberry Pi as our target device, the approach applies to other IoT targets, enabling developers to build sophisticated solutions on virtually any platform running Linux and the Mono runtime.

The Xamarin IoT extension enables developers to create native Linux IoT applications using with the flexibility and elegance of a modern language (C#), the power of the .NET Base Class Library (BCL), and a first-class IDE in Visual Studio for Mac.

<a name="Objectives"></a>
## Objectives ##

- Learn about IoT development with Visual Studio for Mac

<a name="Prerequisites"></a>
## Prerequisites ##

- Visual Studio for Mac with the IoT development extension installed ([https://www.visualstudio.com/vs/visual-studio-mac](https://www.visualstudio.com/vs/visual-studio-mac))
  - Note that this requires [switching to the Alpha channel](https://docs.microsoft.com/en-us/visualstudio/mac/update#changing-the-updater-channel) in Visual Studio for Mac, as the Xamarin IoT extension is currently available as a preview.
  - To install the IoT development extension, click on **Extensions** in the **top menu** to find the **Internet of Things (IoT) development (Preview)** extension under the **IDE extensions** node on the **Gallery** tab.
- A Raspberry Pi 3 device with a push button wired up, configured with Raspbian on the same network as your Mac (see [https://pinout.xyz](https://pinout.xyz/) for guidance on attaching a button)

<a name="Intended Audience"></a>
## Intended Audience ##

This lab is intended for developers who are familiar with C# and Raspberry Pi.

<a name="Exercise1"></a>
## Exercise 1: Getting started targeting IoT devices in Visual Studio for Mac ##

<a name="Ex1Task1"></a>
### Task 1: Setting up your Raspberry Pi ###

1. Install a hardware button onto your Raspberry Pi. It shouldn't matter which button you use. Remember which GPIO you used for later. The code in this lab uses GPIO 3, although you can easily change that later.

    ![](https://user-images.githubusercontent.com/3944468/27368657-9e2459ec-5608-11e7-80be-15d5a420bd2d.png)

1. Although Raspbian is not an exclusive requirement to build and run IoT applications, this lab was designed and tested using it. The following steps are based on the Pixel version that includes a graphical interface.

1. Turn on the Raspberry Pi by plugging it in to a power source.

1. Connecting to the device from Visual Studio for Mac requires SSH to be configured. If you've already done so, skip to the next task. Otherwise, open a new console.

    ![](https://user-images.githubusercontent.com/3944468/27368658-9e25b99a-5608-11e7-9246-e53122029cf8.png)

1. Execute the command below to launch the configuration utility.

    ```
    sudo raspi-config
    ```
1. Select **Interfacing Options** and press **Enter**.

    ![](https://user-images.githubusercontent.com/3944468/27368655-9e1454e8-5608-11e7-969d-cda2279e7f5a.png)

1. Select **SSH** and press **Enter**.

    ![](https://user-images.githubusercontent.com/3944468/27368656-9e17081e-5608-11e7-9e45-a9387d298d1a.png)

1. Select **Yes** and press **Enter** to confirm the SSH server installation.

    ![](https://user-images.githubusercontent.com/3944468/27368626-9d9bf9bc-5608-11e7-8f89-c3715686f1b1.png)

1. After installation has completed, select **Ok** and press **Enter**.

    ![](https://user-images.githubusercontent.com/3944468/27368628-9d9f2312-5608-11e7-8907-9907d6ca77a1.png)

1. Select **Finish** and press **Enter**.

    ![](https://user-images.githubusercontent.com/3944468/27368627-9d9f0e18-5608-11e7-9ca2-ce1ce33d842b.png)

1. Execute the command below in the console to get information about the IP configuration. Although the device should be automatically detected by Visual Studio for Mac, having the IP handy will help if it doesn't.

    ```
    ifconfig
    ```
1. The information you need is in the **inet addr** field. If you're using a wired connection, it will be next to **eth0**. If wireless, use **wlan0**.

    ![](https://user-images.githubusercontent.com/3944468/27368629-9daf1cfe-5608-11e7-8b5b-be47a85a8e3f.png)

<a name="Ex1Task2"></a>
### Task 2: Creating an IoT app in Visual Studio for Mac ###

1. Launch **Visual Studio for Mac**.

1. Select **File > New Solution**.

1. Select the **IoT > App** category and the **Xamarin IoT Application** template. Click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/27368630-9daf4bac-5608-11e7-907f-3da8028e3b0f.png)

1. Enter a **Project Name** of **"IotLab"** and click **Create**. This will generate a simple project designed to be deployed to a device like the Raspberry Pi.

    ![](https://user-images.githubusercontent.com/3944468/27368625-9d996c9c-5608-11e7-938b-9535329c7115.png)

1. Select **Tools > Device Manager**. This window provides a way to select the target device to deploy to.

1. If the device you configured is there, select it and click **Connect**. Otherwise use the **Add Device** option to select it manually.

    ![](https://user-images.githubusercontent.com/3944468/27368631-9dafcc76-5608-11e7-8502-86a1677e0e0b.png)

1. Close the dialog.

1. Note that the target device is now configured next to the **Run** button. The IP address should also match the one from the console on the device itself.

    ![](https://user-images.githubusercontent.com/3944468/27368632-9db3edba-5608-11e7-950d-127fec3f78ef.png)

1. The default project structure is pretty simple. The solution and nested project will be named **IoTLab**. The project will have a node for **Dependencies** that includes references to any necessary assemblies and SDKs. There is only one code file: **Program.cs**. Open it.

    ![](https://user-images.githubusercontent.com/3944468/27368637-9dc5c8b4-5608-11e7-9bd2-e0f12d899fc3.png)

1. By default, all the file does is print a simple message to the console. When debugging, Visual Studio for Mac will be connected to the device and its console output will be redirected back into the IDE. Set a breakpoint on the console line by clicking in its margin or setting the cursor on the line and pressing **F9**.

    ![](https://user-images.githubusercontent.com/3944468/27368633-9db887b2-5608-11e7-9875-9aaa437d1e8c.png)

1. Click the **Run** button to build, deploy, and run the project.

    ![](https://user-images.githubusercontent.com/3944468/27368638-9dc63358-5608-11e7-92da-70052426d2e7.png)

    ![](https://user-images.githubusercontent.com/3944468/27368636-9dc3767c-5608-11e7-888b-6f8a0d7a998a.png)

1. There are several ways to execute the current line. You can press **F10** to step over the current line. Alternatively, you can right-click the next line (a closing brace here) and select **Run To Cursor** to continue execution until that line is reached.

    ![](https://user-images.githubusercontent.com/3944468/27368634-9dc32f64-5608-11e7-8268-ea7a4bf85fdc.png)

1. Expand the **Application Output** pad and see the "**Hello World!"** message printed.

    ![](https://user-images.githubusercontent.com/3944468/27368644-9de072ae-5608-11e7-8ecc-d436e246771b.png)

1. Remove the breakpoint from the code. You can do this through the same process you used to add it.

1. Press **Continue** to finish execution.

    ![](https://user-images.githubusercontent.com/3944468/27368639-9dcccbfa-5608-11e7-987e-813a8ef64443.png)

<a name="Ex1Task3"></a>
### Task 3: Extending the IoT app with Xamarin.IoTSharp.Components ###

1. There are quite a few ways to integrate with the Raspberry Pi. One of the easiest ways is to add a NuGet package that contains an integration library. From **Solution Explorer**, right-click the **Dependencies** node and select **Add Packages**.

    ![](https://user-images.githubusercontent.com/3944468/27368643-9ddd7770-5608-11e7-8435-2a34fd193177.png)

1. Search for **"raspberryio"**, a community-provided MIT-licensed package that offers some very useful integration support. This lab doesn't use this library, but it's great to know that there are always so many options on NuGet and GitHub for Visual Studio developers. Click **Close** to continue without adding the library.

    ![](https://user-images.githubusercontent.com/3944468/27368646-9de9bc6a-5608-11e7-864c-35242b1d9668.png)

1. Open a browser window to [https://github.com/netonjm/iotsharp-components](https://github.com/netonjm/iotsharp-components). This repo includes the **IoTSharp.Components**, an open source library that enables developers to interact easily with components connected to the GPIO of a device.

1. Select **Clone or download > Download ZIP**. Download to a location that's easy to get back to.

    ![](https://user-images.githubusercontent.com/3944468/27368642-9ddb7bfa-5608-11e7-91c8-379d20eb8366.png)

1. In **Visual Studio for Mac**, select **File > Open**. From the downloaded **iotsharp-components-master** folder, open **IotSharp.Components.sln**.

1. Expand the **IoTSharp.Components.Core** project and open the **IIoTButton** interface. This interface is implemented by any device-specific implementation of a button to be used within an app.

    ![](https://user-images.githubusercontent.com/3944468/27368641-9dda49ec-5608-11e7-898e-848700b7bd7f.png)

1. The interface includes the typical events you'd expect to see for a button, such as a **Clicked** handler. It also derives from **IIotComponent**, which you can easily navigate to by right-clicking its reference and selecting **IIotComponent > Go to Declaration**.

    ![](https://user-images.githubusercontent.com/3944468/27368645-9de55f12-5608-11e7-9044-ed2e5d26f68e.png)

1. The **IIoTComponent** is an **IDisposable** that requires one property and one method. The **Update** method should be regularly called within your application loop to raise events.

    ![](https://user-images.githubusercontent.com/3944468/27368648-9df51f4c-5608-11e7-9070-9e519d28c7b4.png)

1. Switch back to **IIoTButton.cs**.

1. Just as you easily navigated to the referenced **IIoTComponent** interface, you can also easily find all the places this interface is referenced, including classes that implement it. Right-click **IIoTButton** and select **Find References**.

    ![](https://user-images.githubusercontent.com/3944468/27368647-9dee7ab6-5608-11e7-9b7a-0b55129cc849.png)

1. In the **Search Results** pad you'll see a list of all places this interface is referenced, whether as a local variable, implementation, or parameter. Double-click one of the references if you would like to open it to explore deeper.

    ![](https://user-images.githubusercontent.com/3944468/27368652-9e03b0f2-5608-11e7-9ffb-f1357c8ec5c6.png)

1. Select **Build > Build All** to build the solution so that we can return to the original project and reference the produced assembly.

1. Select **File > Recent Solutions > IoTLab** to reopen that project.

1. Right-click the **Dependencies** node and select **Edit References**.

    ![](https://user-images.githubusercontent.com/3944468/27368649-9df681a2-5608-11e7-8651-4ec334f621e8.png)

1. Select the **.Net Assembly** tab and use the **Browse** button to navigate to and select the DLL created earlier. It will be named **IoTSharp.Components.dll** and be in the **bin/Debug** folder of the **IoTSharp.Components.Raspbian** project folder.

    ![](https://user-images.githubusercontent.com/3944468/27368650-9dfa5296-5608-11e7-9615-89e900585301.png)

1. If **Program.cs** is not open, open it now.

1. Add the code below to the end of the **Main** function. Scan the comments to get a feel for what the code does.

    ```csharp
    // The app will run until the button has been clicked 10 times.
    int count = 10;
    
    // The button is wired up on GPIO 3 here, but you can easily change that if you need.
    IoTButton button = new IoTButton(Connectors.GPIO3);
    
    // C#'s elegant syntax makes it easy to wire up an event handler that both decrements
    // the counter and prints a message to the console.
    button.Clicked += () => Console.WriteLine($"{--count} button clicks left");
    
    // The app loop will manage the button and sleep until there are no clicks left.
    while (count > 0)
    {
        button.Update();
        Thread.Sleep(150);
    }
    ```
1. You may notice that there are some red squiggles under certain parts of the code that indicate an error. For example, although the IoT assembly is referenced, there's an error when trying to use its components. Visual Studio for Mac can often help you quickly and easily resolve these problems via **Quick Fix** solutions. Right-click the first **IoTButton** reference and select **Quick Fix > using IoTSharp.Components;** to add a using statement to the file. This will resolve errors referencing classes from that namespace.

    ![](https://user-images.githubusercontent.com/3944468/27368651-9dfc283c-5608-11e7-953f-258e040e9647.png)

1. Repeat the process to add a **using** statement for the **Thread** reference error further down in the code. This should resolve all errors in the code.

1. Press **F5** or click the **Run** button to build and run the updated app on your Raspberry Pi.

    ![](https://user-images.githubusercontent.com/3944468/27368653-9e069c18-5608-11e7-85e1-62387bcdb456.png)

1. After the app has deployed, click the physical button attached to the board a few times to test it out.

1. In Visual Studio for Mac, expand the **Application Output** pad to see the logged messages. Note that you could alternatively set a breakpoint within the handler to confirm as well.

    ![](https://user-images.githubusercontent.com/3944468/27368654-9e09142a-5608-11e7-85c6-018da3d1b279.png)

1. Click the button until **count** reaches 0 and the application exits.

<a name="Summary"></a>
## Summary ##

In this lab, you've learned how to get started targeting IoT devices with Visual Studio for Mac.

