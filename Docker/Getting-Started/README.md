<a name="Title"></a>
# Getting started integrating with Docker containers in Visual Studio for Mac #

<a name="Overview"></a>
## Overview ##

Microsoft offers great support for building apps deployed to Docker containers in Visual Studio for Mac, that can be easily deployed and managed in Azure. With the power of the cloud behind your solution, you can easily compose powerful constellations of these containers known as "microservices" that are independently managed and scaled. In this hands-on lab you will create a microservices solution that employs two ASP.NET Core apps hosted in separate Docker containers.

<a name="Objectives"></a>
## Objectives ##

- Create an ASP.NET Core web site
- Deploy the site in a Docker container
- Create and integrate an ASP.NET Core web API project in a Docker container

<a name="Prerequisites"></a>
## Prerequisites ##

- Visual Studio for Mac ([https://www.visualstudio.com/vs/visual-studio-mac](https://www.visualstudio.com/vs/visual-studio-mac))
  - Note that this lab currently requires [switching to the Alpha channel](https://docs.microsoft.com/visualstudio/mac/update) in Visual Studio for Mac, as Docker support is available as a preview
- Docker ([https://go.microsoft.com/fwlink/?linkid=847268](https://go.microsoft.com/fwlink/?linkid=847268))

<a name="Intended Audience"></a>
## Intended Audience ##

This lab is intended for developers who are familiar with C# and ASP.NET, although deep experience is not required.

<a name="Exercise1"></a>
## Exercise 1: Getting started integrating with Docker containers in Visual Studio for Mac ##

<a name="Ex1Task1"></a>
### Task 1: Creating an ASP.NET Core web site and enabling it for Docker ###

1. Launch **Visual Studio for Mac**.

1. Select **File > New Solution**.

1. Select the **.NET Core > App** category and the **ASP.NET Core Web App** template. Click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/29035189-97e1dcaa-7b4f-11e7-9800-0b7d8485b811.png)

1. If presented with a **Target Framework** selection screen for .NET Core, select the target framework you want to use, such as **.NET Core 1.1**. Click **Next**.

1. Enter a **Project Name** of **"DockerLab"** and click **Create**.

    ![](https://user-images.githubusercontent.com/3944468/29035195-98039fa2-7b4f-11e7-9f90-50c2bff50a71.png)

1. The created project contains all the basics you need to build and run an ASP.NET Core web site.

    ![](https://user-images.githubusercontent.com/3944468/29035190-97e818b8-7b4f-11e7-8ee7-c01296b60c11.png)

1. In **Solution Explorer**, right-click the **DockerLab** project node and select **Add > Add Docker Support**. This will add a new Docker-specific project to the solution (**docker-compose**), along with a Docker configuration file to the project.

    ![](https://user-images.githubusercontent.com/3944468/29035191-97f0f5a0-7b4f-11e7-9160-0d64b80ba304.png)

1. Open the newly added **Dockerfile** from the **DockerLab** project.

    ![](https://user-images.githubusercontent.com/3944468/29035192-97f4ca9a-7b4f-11e7-83c6-a8c86d64674f.png)

1. **Dockerfile** describes the application, including the base container, the port number to expose the application on, the entry point of the application, and more. You can learn more about this format [here](https://docs.docker.com/engine/reference/builder).

    ![](https://user-images.githubusercontent.com/3944468/29035193-97f8fc5a-7b4f-11e7-9221-51f79ebc04a9.png)

1. From the **docker-compose** project, open **docker-compose.yml**.

    ![](https://user-images.githubusercontent.com/3944468/29035196-9807c956-7b4f-11e7-9482-b87b5337fe85.png)

1. **docker-compose.yml** describes how the application should be composed of the required containers to set up a given solution. Right now, there's just one service for the **DockerLab** project created earlier. More will be added here as additional container apps become part of the solution.

    ![](https://user-images.githubusercontent.com/3944468/29035194-98007c64-7b4f-11e7-98ff-74bfb03a77d7.png)

1. Note that the **docker-compose** project has also become the default startup project. This makes it easy to launch multiple projects as part of the same debugging session.

    ![](https://user-images.githubusercontent.com/3944468/29035197-980b1ebc-7b4f-11e7-9288-ef4a10c2aae4.png)

1. Select **Run > Start Debugging** or press **F5** to build, deploy, and run the project in a Docker container. This may take a minute or so to complete.

1. Once the application has launched in Safari, note that the URL is to the specific localhost port the container is listening on (it may vary from the screenshot below).

    ![](https://user-images.githubusercontent.com/3944468/29035198-980d851c-7b4f-11e7-9f29-bf5039c09ed7.png)

1. Open a new instance of **Terminal**.

1. Execute the command below to get a list of all Docker containers running.

    ```
    docker ps
    ```
1. Unless you have other containers running, there should be only one. Note that the data wraps into three lines in the screenshot below. A key point of interest is the port relay that indicates that the container is listening on a given port (which should be the same as used by the browser above) and using port 80 to reach its internal web server. As far as the app knows, it's listening on port 80.

    ![](https://user-images.githubusercontent.com/3944468/29035199-980f6044-7b4f-11e7-91dd-8da61fdd00ca.png)

<a name="Ex1Task2"></a>
### Task 2: Creating an ASP.NET Core Web API and enabling it for Docker ###

1. Return to **Visual Studio for Mac** and stop debugging.

1. In **Solution Explorer**, right-click the **DockerLab** solution node (the top-most node) and select **Add > Add New Project**.

    ![](https://user-images.githubusercontent.com/3944468/29035203-9827096a-7b4f-11e7-8ac4-9dc176717b07.png)

1. Select the **.NET Core > App** category and the **ASP.NET Core Web API** project template. Rather than including some basic web application files to render HTML, this template includes a controller designed to handle RESTful requests. Click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/29035201-981bea58-7b4f-11e7-8580-9f31479cec40.png)

1. If presented with a **Target Framework** selection screen for .NET Core, select the target framework you want to use, such as **.NET Core 1.1**. Click **Next**.

1. Enter a project name of **"api"** and click **Create**.

    ![](https://user-images.githubusercontent.com/3944468/29035200-98198628-7b4f-11e7-8c08-467f5ace7bdd.png)

1. The project structure of the API project is similar to the web site project, except that it has fewer files since it doesn't need views or some of the client-side web components. It still uses MVC, so the **Controllers** folder is where the magic happens.

    ![](https://user-images.githubusercontent.com/3944468/29035205-982d8ec0-7b4f-11e7-9d16-e2fd3016476e.png)

1. Right-click the **api** project node and select **Add > Add Docker Support**. This will run the same process as before, but will now merge new settings for this project alongside the existing settings for the **DockerLab** project.

    ![](https://user-images.githubusercontent.com/3944468/29035206-9830fc68-7b4f-11e7-952d-9c5ccc4839e8.png)

1. Close and reopen **docker-compose.yml** to refresh the changes.

    ![](https://user-images.githubusercontent.com/3944468/29035202-9822b68a-7b4f-11e7-9ebc-2dd6160bf831.png)

1. Now you will see that the second API project has been added alongside the web application. When built and run, they will be deployed to separate Docker containers and able to access each other as configured.

    ![](https://user-images.githubusercontent.com/3944468/29035204-9829b75a-7b4f-11e7-8e2a-562673126ed0.png)

<a name="Ex1Task3"></a>
### Task 3: Integrating two container apps ###

1. From the **api** project, open **Controllers/ValuesController.cs**. This is the default controller that contains a configured API you can edit.

    ![](https://user-images.githubusercontent.com/3944468/29035207-9831d58e-7b4f-11e7-940a-61fc2d79ce57.png)

1. Replace the first **Get** method with the code below. This is just a minor change to make the API easier to consume for our lab purposes. Do not remove the **[HttpGet]** attribute.

    ```csharp
    public string Get()
    {
        string message = "API";
        return message;
    }
    ```
1. Your inserted code should look like this.

    ![](https://user-images.githubusercontent.com/3944468/29035208-98382574-7b4f-11e7-82df-1b50b3f179b5.png)

1. Set a breakpoint on the **return** line of code. You can do this by clicking in the margin or by setting the cursor somewhere on the line and pressing **F9**.

    ![](https://user-images.githubusercontent.com/3944468/29035209-983aa68c-7b4f-11e7-99b4-945baa93a9af.png)

1. From the **DockerLab** project, open **Controllers/HomeController.cs**. This controller manages the content rendered for the default pages in the project.

    ![](https://user-images.githubusercontent.com/3944468/29035210-983d1ade-7b4f-11e7-8eef-303c3082480a.png)

1. In the **About** method, replace the first line referencing **ViewData** with the code below. This code integrates with the API you just edited in the API project and displays the result. This is a practical approach for the purposes of this lab and not a proper practice for more robust applications.

    ```csharp
    System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
    var result = client.GetAsync("http://api/api/values").Result;
    string text = result.Content.ReadAsStringAsync().Result;
    ViewData["Message"] = "The value is " + text;
    ```
1. Set a breakpoint on the **return** line of this method.

    ![](https://user-images.githubusercontent.com/3944468/29035213-9846b4ae-7b4f-11e7-8835-8b7043e75fbb.png)

<a name="Ex1Task4"></a>
### Task 4: Debugging multi-container solutions ###

1. Press **F5** to build and run the project. When the browser window loads, two Docker containers will be running.

1. In **Terminal**, execute the command below to view the containers.

    ```
    docker ps
    ```
1. The two containers with their details will be displayed. Note that the data wraps to three lines each.

    ![](https://user-images.githubusercontent.com/3944468/29035211-98446122-7b4f-11e7-9501-d65f33e3f256.png)

1. In **the browser**, navigate to the **About** page.

    ![](https://user-images.githubusercontent.com/3944468/29035212-9845cfe4-7b4f-11e7-9b76-8f8b54eb773a.png)

1. Visual Studio for Mac will hit two breakpoints during this request. Press **F10** to step through each execution as the API request is returned to the web app (across Docker containers!) and rendered. You will eventually see the result of **"API"** that was returned from the API displayed from the web app.

    ![](https://user-images.githubusercontent.com/3944468/29035214-98489f44-7b4f-11e7-9fae-a99208bfe22e.png)

1. **Refresh the page in the browser**. This will hit the first breakpoint once again.

1. This time, open the **Locals** pad and locate the **message** variable. It should have the value of **"API"** since that was the most recently executed line of code.

    ![](https://user-images.githubusercontent.com/3944468/29035215-984e1938-7b4f-11e7-8759-8d034ad9703d.png)

1. Double-click the value and change it to **"changed"** (with quotes). Press **Enter** to apply.

    ![](https://user-images.githubusercontent.com/3944468/29035216-9852012e-7b4f-11e7-8b33-4b8bee1a03e4.png)

1. Press **F5** to continue execution.

1. The next stop will be in the container hosting the web app, where you will see the returned value. This time it should reflect **"changed"**. Press **F5** to continue execution once again.

1. The new API value should render out as HTML in the browser. This multi-container microservices solution is now ready to be extended and deployed.

    ![](https://user-images.githubusercontent.com/3944468/29035217-9858e868-7b4f-11e7-939a-7259cc624338.png)

<a name="Summary"></a>
## Summary ##

In this lab, you've learned how to get started integrating with Docker containers with Visual Studio for Mac.

