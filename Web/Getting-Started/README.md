<a name="Title"></a>
# Getting started building ASP.NET Core applications in Visual Studio for Mac #

<a name="Overview"></a>
## Overview ##

ASP.NET Core is an open-source and cross-platform framework for building modern cloud based internet connected applications, such as web apps and services, IoT apps, and mobile backends. ASP.NET Core apps can run on [.NET Core](https://www.microsoft.com/net/core/platform) or on the .NET Framework runtimes. It was architected to provide an optimized development framework for apps that are deployed to the cloud or run on-premises. It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions. You can develop and run your ASP.NET Core apps cross-platform on Windows, Mac, and Linux. ASP.NET Core is open source at [GitHub](https://github.com/aspnet/home).

In this lab you will create and explore an ASP.NET Core application with Visual Studio for Mac.

<a name="Objectives"></a>
## Objectives ##

- Create an ASP.NET Core web app
- Explore the ASP.NET Core hosting, configuration, and middleware model
- Debug an ASP.NET Core web app

<a name="Prerequisites"></a>
## Prerequisites ##

- Visual Studio for Mac

<a name="Intended Audience"></a>
## Intended Audience ##

This lab is intended for developers who are familiar with C#, although deep experience is not required.

<a name="Exercise1"></a>
## Exercise 1: Getting started building .NET Core applications in Visual Studio for Mac ##

<a name="Ex1Task1"></a>
## Task 1: Creating a new ASP.NET Core application ##

1. Launch **Visual Studio for Mac**.

1. Select **File > New Solution**.

1. Select the **.NET Core > App** category and the **ASP.NET Core Web App (C#)** template. Click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/27835466-1a1ca478-6090-11e7-8b67-76b16cf015a5.png)

1. Enter a name of **"CoreLab"** and click **Create** to create the project. It will take a moment to complete.

    ![](https://user-images.githubusercontent.com/3944468/27835471-1a2b3420-6090-11e7-8cda-49d0647e9d52.png)

<a name="Ex1Task2"></a>
## Task 2: Touring the solution ##

1. The default template will produce a solution with a single ASP.NET Core project named **CoreLab**. Expand the project node to expose its contents.

    ![](https://user-images.githubusercontent.com/3944468/27835470-1a2a765c-6090-11e7-8f6a-fb0de748238e.png)

1. This project follows the model-view-controller (MVC) paradigm to provide a clear division of responsibilities between data (models), presentation (views), and functionality (controllers). Open the **HomeController.cs** file from the **Controllers** folder.

    ![](https://user-images.githubusercontent.com/3944468/27835467-1a2784d8-6090-11e7-80de-63ba41ff083b.png)

1. The **HomeController** class-by convention-handles all incoming requests that start with **/Home**. The **Index** method handles requests to the root of the directory (like http://site.com/Home) and other methods handle requests to their named path based on convention, such as **About()** handling requests to **http://site.com/Home/About**. Of course, this is all configurable. One special item of note is that the **HomeController** is the default controller in a new project, so requests to the root of the site (**http://site.com**) would go through **Index()** of the **HomeController** just like requests to **http://site.com/Home** or **http://site.com/Home/Index**.

    ![](https://user-images.githubusercontent.com/3944468/27835468-1a27b3fe-6090-11e7-8578-ce9b288f74a4.png)

1. The project also has a **Views** folder that contains other folders that map to each controller (as well as one for **Shared** views. For example, the view CSHTML file (an extension of HTML) for the **/Home/About** path would be at **Views/Home/About.cshtml**. Open that file.

    ![](https://user-images.githubusercontent.com/3944468/27835469-1a289e2c-6090-11e7-9e48-dc5c11603ad8.png)

1. This CSHTML file uses the Razor syntax to render HTML based on a combination of standard tags and inline C#. You can learn more about this in the [online documentation](https://docs.microsoft.com/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c).

    ![](https://user-images.githubusercontent.com/3944468/27835472-1a3093d4-6090-11e7-8543-d2e8c4e4fb7d.png)

1. The solution also contains a **wwwroot** folder that will be the root for your web site. You can put static site content, such as CSS, images, and JavaScript libraries, directly at the paths you'd want them to be at when the site is deployed.

    ![](https://user-images.githubusercontent.com/3944468/27835475-1a3decaa-6090-11e7-9ee7-89bd9933288f.png)

1. There are also a variety of configuration files that serve to manage the project, its packages, and the application at runtime. For example, the default application [configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration) is stored in **appsettings.json**. However, you can override some/all of these settings on a per-environment basis, such as by providing an **appsettings.Development.json** file for the **Development** environment.

    ![](https://user-images.githubusercontent.com/3944468/27835474-1a3cb6aa-6090-11e7-917a-747058343b09.png)

<a name="Ex1Task3"></a>
## Task 3: Understanding how the application is hosted ##

1. From **Solution Explorer**, open **Program.cs**. This is the bootstrapper that will run your application.

    ![]https://user-images.githubusercontent.com/3944468/27835473-1a3c7a5a-6090-11e7-9b6e-4e7842af10fc.png)

1. While there are only two lines of code here, they're pretty substantial. Let's break them down. First, a new **WebHostBuilder** is created. ASP.NET Core apps require a host in which to execute. A host must implement the **IWebHost** interface, which exposes collections of features and services, and a **Start** method. The host is typically created using an instance of a **WebHostBuilder**, which builds and returns a **WebHost** instance. The **WebHost** references the server that will handle requests.

    ![](https://user-images.githubusercontent.com/3944468/27835476-1a3e4e0c-6090-11e7-8c26-e63298e04a13.png)

1. While the **WebHostBuilder** is responsible for creating the host that will bootstrap the server for the app, it requires you provide a server that implements **IServer**. By default, this is **[Kestrel](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel)**, a cross-platform web server for ASP.NET Core based on **libuv**, which is a cross-platform asynchronous I/O library.

    ![](https://user-images.githubusercontent.com/3944468/27835477-1a3eda52-6090-11e7-8796-755df1b09e12.png)

1. Next, the server's content root is set. This determines where it searches for content files, like MVC View files. The default content root is the folder from which the application is run.

    ![](https://user-images.githubusercontent.com/3944468/27835482-1a57f7d0-6090-11e7-8515-95b8593026c5.png)

1. If the app must work with the Internet Information Services (IIS) web server, the **UseIISIntegration** method should be called as part of building the host. Note that this does not configure a server, like **UseKestrel** does. To use IIS with ASP.NET Core, you must specify both **UseKestrel** and **UseIISIntegration**. **Kestrel** is designed to be run behind a proxy and should not be deployed directly facing the internet. **UseIISIntegration** specifies IIS as the reverse proxy server, but it's only relevant when running on machines that have IIS. If you deploy your application to Windows, leave it in. It doesn't hurt otherwise.

    ![](https://user-images.githubusercontent.com/3944468/27835478-1a52742c-6090-11e7-9439-371a07ff236f.png)

1. It's a cleaner practice to separate the loading of settings from the application bootstrapping. To easily do this, **UseStartup** is called to specify that the **Startup** class is to be called for the loading of settings and other startup tasks, such as inserting middleware into the HTTP pipeline. You may have multiple **UseStartup** calls with the expectation that each one overwrites previous settings as needed.

    ![](https://user-images.githubusercontent.com/3944468/27835479-1a52aa0a-6090-11e7-92b5-7f1b10c2dc82.png)

1. The last step in creating the **IWebHost** is to call **Build**.

    ![](https://user-images.githubusercontent.com/3944468/27835480-1a53d51a-6090-11e7-9afd-2c173b423b97.png)

1. While **IWebHost** classes are required to implement the non-blocking **Start**, ASP.NET Core projects have an extension method called **Run** that wraps **Start** with blocking code so you don't need to manually prevent the method from exiting immediately.

    ![](https://user-images.githubusercontent.com/3944468/27835483-1a58a87e-6090-11e7-90b6-5d387a745e36.png)

<a name="Ex1Task4"></a>
## Task 4: Running and debugging the application ##

1. In **Solution Explorer**, right-click the **CoreLab** project node and select **Options**.

    ![](https://user-images.githubusercontent.com/3944468/27835481-1a54074c-6090-11e7-9e09-770ac346addd.png)

1. The **Project Options** dialog includes everything you need to adjust how the application is built and run. Select the **Run > Configurations > Default** node from the left panel.

1. Check **Run on external console** and uncheck **Pause console output**. Ordinarily the self-hosted application would not have its console visible, but would instead log its results to the **Output** pad. For the purposes of this lab we'll show it in a separate window as well, although you don't need to do that during normal development.

1. Click **OK**.

    ![](https://user-images.githubusercontent.com/3944468/27835485-1a66e6aa-6090-11e7-8085-3a705fc321d8.png)

1. Press **F5** to build and run the application. Alternatively, you can select **Run > Start Debugging**.

1. Visual Studio for Mac will launch two windows. The first is a console window that provides you a view into the self-hosted server application.

    ![](https://user-images.githubusercontent.com/3944468/27835484-1a666888-6090-11e7-8e83-74a1e878a8dd.png)

1. The second is a typical browser window to test the site. As far as the browser knows, this application could be hosted anywhere. Click **About** to navigate to that page.

    ![](https://user-images.githubusercontent.com/3944468/27835489-1a6db692-6090-11e7-9217-b3ef6b09582e.png)

1. Among other things, the about page renders some text set in the controller.

    ![](https://user-images.githubusercontent.com/3944468/27835486-1a684f2c-6090-11e7-8fb1-00957a92eddb.png)

1. Keep both windows open and return to Visual Studio for Mac. Open **Controllers/HomeController.cs** if it's not already open.

    ![](https://user-images.githubusercontent.com/3944468/27835488-1a6c1dfa-6090-11e7-9794-2a2328f3d073.png)

1. Set a breakpoint in the first line of the **About** method. You can do this by clicking in the margin or setting the cursor on the line and pressing **F9**. This line sets some data in the **ViewData** collection that is rendered in the CSHTML page at **Views/Home/About.cshtml**.

    ![](https://user-images.githubusercontent.com/3944468/27835490-1a6edaf4-6090-11e7-90ab-d4fa332793c7.png)

1. Return to the browser and refresh the about page. This will trigger the breakpoint in Visual Studio for Mac.

1. Mouse over the **ViewData** member to view its data. You can also expand its child members to see nested data.

    ![](https://user-images.githubusercontent.com/3944468/27835492-1a7cc6a0-6090-11e7-8ffa-68ae53da92fd.png)

1. Remove the application breakpoint using the same method you used to add it.

1. Open **Views/Home/About.cshtml**.

1. Change the text **"additional"** to **"changed"** and save the file.

    ![](https://user-images.githubusercontent.com/3944468/27835491-1a7b8664-6090-11e7-913d-05a6be32b636.png)

1. Press the **Continue** button to continue execution.

    ![](https://user-images.githubusercontent.com/3944468/27835493-1a7e941c-6090-11e7-9e85-1972ca4295e4.png)

1. Return to the browser window to see the updated text. Note that this change could be done at any time and didn't necessarily require a debugger breakpoint. Refresh the browser if you don't see the change reflected immediately.

    ![](https://user-images.githubusercontent.com/3944468/27835494-1a808b82-6090-11e7-8b10-ea3509aef2d6.png)

1. Close the test browser window and application console. This will stop debugging as well.

<a name="Ex1Task5"></a>
## Task 5: Reviewing application startup configuration ##

1. From **Solution Explorer**, open **Startup.cs**. You may notice some red squiggles initially as NuGet packages are being restored in the background and the Roslyn compiler is building a complete picture of the project dependencies.

    ![](https://user-images.githubusercontent.com/3944468/27835495-1a81a102-6090-11e7-90be-23781263c16b.png)

1. Locate the **Startup** method. This section defines the initial configuration for the application and is very densely packed. Let's break it down.

    ![](https://user-images.githubusercontent.com/3944468/27835496-1a8315fa-6090-11e7-817c-8dbd12db12ab.png)

1. The method starts off by initializing a **ConfigurationBuilder** and setting its base path.

    ![](https://user-images.githubusercontent.com/3944468/27835497-1a8fae96-6090-11e7-8418-3ecf2290e2e1.png)

1. Next, it loads a required **appsettings.json** file.

    ![](https://user-images.githubusercontent.com/3944468/27835498-1a9177a8-6090-11e7-8eaf-aad1e4a7a179.png)

1. After that, it attempts to load an environment-specific **appsettings.json** file, which would override existing settings. For example, this is a provided **appsettings.Development.json** file used for that specific environment. To read more about configuration in ASP.NET Core, check out [the docs](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration).

    ![](https://user-images.githubusercontent.com/3944468/27835499-1a941b5c-6090-11e7-80fa-4f39d88e2ef9.png)

1. Finally, the environment variables are added to the configuration builder and the configuration is built and set for usage.

    ![](https://user-images.githubusercontent.com/3944468/27835500-1a945fe0-6090-11e7-8d44-d6a3c9939c85.png)

<a name="Ex1Task6"></a>
## Task 6: Inserting application middleware ##

1. Locate the **Configure** method in the **Startup** class. This is where all the middleware is configured so that it can be inserted into the HTTP pipeline and used to process every request to the server. Note that while this method is called only once, the contents of the methods (such as **UseStaticFiles**) may be executed on every request.

    ![](https://user-images.githubusercontent.com/3944468/27835502-1a99437a-6090-11e7-8548-4b4c1cd05de6.png)

1. You can also add additional middleware to be executed as part of the pipeline. Add the code below after **app.UseStaticFiles** to automatically add an **X-Test** header to every outgoing response. Note that IntelliSense will help complete the code as you type.

    ```csharp
    app.Use(async (context, next) =>
    {
        context.Response.Headers.Add("X-Test", new[] { "Test value" });
        await next();
    });
    ```
1. Press **F5** to build and run the project.

1. We can use the browser to inspect the headers to verify they are added. The instructions below are for Safari, but you can do the same in [Chrome](https://stackoverflow.com/questions/4423061/view-http-headers-in-google-chrome) or [Firefox](https://stackoverflow.com/questions/33974595/in-firefox-how-do-i-see-http-request-headers-where-in-web-console).

1. Once the browser loads the site, select **Safari > Preferences**.

1. On the **Advanced** tab, check **Show Develop menu in menu bar** and close the dialog.

    ![](https://user-images.githubusercontent.com/3944468/27835501-1a96d50e-6090-11e7-89cf-4f3307cd7446.png)

1. Select **Develop > Show Page Resources**.

1. Refresh the browser window so that the newly opened developer tools can track and analyze the traffic and content.

1. The localhost HTML page rendered by the server will be the item selected by default.

    ![](https://user-images.githubusercontent.com/3944468/27835503-1aa5bcb8-6090-11e7-85e8-90f67ee1a97c.png)

1. Expand the **Details sidebar**.

    ![](https://user-images.githubusercontent.com/3944468/27835504-1aa61906-6090-11e7-864e-3a5b77725240.png)

1. Scroll to the bottom of the sidebar to see the response header added in code earlier.

    ![](https://user-images.githubusercontent.com/3944468/27835505-1aaadcc0-6090-11e7-80f0-ccabd8dd3cd5.png)

1. Close the browser window and console when satisfied.

<a name="Summary"></a>
## Summary ##

In this lab, you've learned how to get started developing ASP.NET Core apps with Visual Studio for Mac. If you'd like to explore developing a more complete movies database application, check out the tutorial at [https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc).

