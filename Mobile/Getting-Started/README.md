<a name="Title"></a>
# Getting started building cloud-connected mobile apps in Visual Studio for Mac #

<a name="Overview"></a>
## Overview ##

Visual Studio for Mac and Visual Studio Tools for Xamarin enable you to deliver native Android, iOS, and Windows apps, using existing development skills, teams, and code. You can then integrate those apps with backend cloud services powered by Azure App Service.

In this lab you'll use Visual Studio for Mac to create a simple to-do app that makes use of a portable cross-platform library and an Azure App Service backend.

<a name="Objectives"></a>
## Objectives ##

- Create a cross-platform mobile app using Xamarin.Forms
- Create a mobile backend using Azure App Service
- Build, deploy, and debug the mobile app

<a name="Prerequisites"></a>
## Prerequisites ##

- Visual Studio for Mac
- An Azure account (sign up for free at [https://azure.com/free](https://azure.com/free))
- If using a physical device, install the Xamarin Live Player from your device's app store
  - Note that Xamarin Live Player is currently in preview and will require you to [switch to the Alpha updater channel](https://docs.microsoft.com/en-us/visualstudio/mac/update) in Visual Studio for Mac

<a name="Intended Audience"></a>
## Intended Audience ##

This lab is intended for developers who are familiar with C#, although deep experience is not required.

<a name="Exercise1"></a>
## Exercise 1: Getting started building cloud-connected mobile apps in Visual Studio for Mac ##

<a name="Ex1Task1"></a>
### Task 1: Creating a cross-platform mobile app ###

1. Launch **Visual Studio for Mac**.

1. Select **File > New Solution**.

1. From the **Multiplatform > App** category, select the **Blank Forms App** template. Click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/27846749-7bf635a2-60ee-11e7-8246-06baeff22ac5.png)

1. Set the **App Name** to **"MobileCloudLab"**. Note that you have the option to select specific target platforms and whether to use **Shared Code** or a **Portable Class Library**. Leave the defaults and click **Next**.

    ![](https://user-images.githubusercontent.com/3944468/27846746-7becfce4-60ee-11e7-8402-5f8f63d0fbda.png)

1. Make sure the **Project Name** is set to **"MobileCloudLab"** and click **Create**.

    ![](https://user-images.githubusercontent.com/3944468/27846748-7bf48432-60ee-11e7-81d4-11f31fdeea42.png)

<a name="Ex1Task2"></a>
### Task 2: Creating an Azure mobile backend with Azure App Services ###

1. The **Getting Started** view should be visible by default. Click **Add an Azure Mobile Backend**.

    ![](https://user-images.githubusercontent.com/3944468/27846745-7bebbca8-60ee-11e7-9c08-6e2074079c5c.png)

1. Click **Add** to begin the process of setting up an Azure App Service and configuring the mobile app to connect to it.

    ![](https://user-images.githubusercontent.com/3944468/27846747-7befa6ec-60ee-11e7-9503-ad4d664114ec.png)

1. As part of the process, relevant NuGet packages will be added to the various projects. In addition to the portable project, you can also add these dependencies to the platform-specific project. Click **Continue**.

    ![](https://user-images.githubusercontent.com/3944468/27846744-7beb9548-60ee-11e7-8aa7-e6b4a8c43c83.png)

1. The **Microsoft.Azure.Mobile.Client** package contains the key functionality required to easily work with the backend being created. It is dependent on a few other packages, so click **Accept** to agree to the terms for all of them.

    ![](https://user-images.githubusercontent.com/3944468/27846750-7c0249fa-60ee-11e7-853c-cca517100fa0.png)

1. Click **Accept** to accept the terms of the **SQLiteStore** package.

    ![](https://user-images.githubusercontent.com/3944468/27846751-7c02a22e-60ee-11e7-8d61-479574b09102.png)

1. Now that the client projects have been configured, you'll turn your attention to setting up the Azure service. Click **Sign in with your Microsoft account** to get started.

    ![](https://user-images.githubusercontent.com/3944468/27846755-7c1a507c-60ee-11e7-8179-3c180ee1fe4a.png)

    _Note: if you already have an account associated with Visual Studio for Mac, you won't see this dialog and your services will show up automatically._

1. Sign in with your Azure account credentials. Once the account has been authenticated and is loaded with the avatar and Visual Studio license information, close the dialog.

1. Click **New** to create a new app service.

    ![](https://user-images.githubusercontent.com/3944468/27846756-7c1b0184-60ee-11e7-8137-bb93e663c625.png)

1. You'll need to set **App Service Name** to a globally unique name, so consider appending your name to **"MobileCloudLab"**, such as **"MobileCloudLabJohnDoe"** as shown below. You can also configure the subscription, resource group, and service plan settings (such as the **F1 - Free** pricing). Click **Create**.

    ![](https://user-images.githubusercontent.com/3944468/27846752-7c0c36ea-60ee-11e7-98d4-1d6a17565bd9.png)

1. Click **OK** to begin the provisioning.

    ![](https://user-images.githubusercontent.com/3944468/27846753-7c0c3f96-60ee-11e7-9333-53fa916e7038.png)

1. Once the service has been provisioned, select it from the list. Note the URL provided in the sample code below. You will need this later to configure the **MobileServiceClient** in code.

    ![](https://user-images.githubusercontent.com/3944468/27846759-7c2b80ae-60ee-11e7-9da6-31eb452b2354.png)

<a name="Ex1Task3"></a>
### Task 3: Configuring Easy tables in an Azure App Service ###

1. Open a browser window to **[https://portal.azure.com](https://portal.azure.com/)** and log in.

1. Search for the app service you just created and select it when shown.

    ![](https://user-images.githubusercontent.com/3944468/27846754-7c18e4e4-60ee-11e7-8fce-5349a3db6475.png)

1. This lab uses **Easy tables**, a feature that makes it incredibly easy to set up and integrate with backend table data. Scroll down in the list under the Search box to find Easy tables and select it.

    ![](https://user-images.githubusercontent.com/3944468/27846758-7c26087c-60ee-11e7-9b10-c529a101753b.png)

1. If this is the first time you're configuring this service, you will need to configure it. Click the **Need to configure...** link to continue.

    ![](https://user-images.githubusercontent.com/3944468/27846757-7c24e992-60ee-11e7-9f40-c7bdc24ad904.png)

1. To use **Easy tables** you will need a database. Click the **You need a database...** link to continue.

    ![](https://user-images.githubusercontent.com/3944468/27846760-7c2ca5c4-60ee-11e7-968d-f4366a8adecf.png)

1. Click **Add** to add a data connection.

    ![](https://user-images.githubusercontent.com/3944468/27846761-7c2eee2e-60ee-11e7-9ebb-4ea58c2caa4a.png)

1. Ensure the **Type** is set to **SQL Database** and click **Configure required settings**.

    ![](https://user-images.githubusercontent.com/3944468/27846762-7c30d5f4-60ee-11e7-9cde-e14153ed7c6c.png)

1. Click **Create a new database**.

    ![](https://user-images.githubusercontent.com/3944468/27846763-7c3d1fc6-60ee-11e7-8e4b-368ec4ed0a5b.png)

1. Set the **Name** to **"mobilecloudlab"** and click **Configure required settings**.

    ![](https://user-images.githubusercontent.com/3944468/27846769-7c51d9f2-60ee-11e7-9aa7-a920bf2ae84c.png)

1. You may opt here to select an existing server if you have one you want to use. Otherwise, click **Create a new server**.

    ![](https://user-images.githubusercontent.com/3944468/27846764-7c4253ba-60ee-11e7-984a-198a23c7385a.png)

1. Fill out the required fields and click **Select**. You will not need these settings again in the lab since the Easy Tables data connection will handle everything for you transparently.

    ![](https://user-images.githubusercontent.com/3944468/27846766-7c442e42-60ee-11e7-9291-ca0007e079e0.png)

1. Click **Pricing tier** to select a **Pricing tier** (such as the **Free** option) and click **Select**.

    ![](https://user-images.githubusercontent.com/3944468/27846765-7c431282-60ee-11e7-9731-ca16d7dc8f78.png)

1. Click to configure the **Connection string** and click **OK**. You can use the default name.

    ![](https://user-images.githubusercontent.com/3944468/27846767-7c4aea52-60ee-11e7-80b2-7a0d436b56b8.png)

1. Click **OK** to finish setting up the data connection.

    ![](https://user-images.githubusercontent.com/3944468/27846768-7c516f8a-60ee-11e7-9725-064a1536f85d.png)

1. It will take a minute or so for the resources to be created and initialized. Once ready, you will be returned to the mobile app blade.

1. Click the **Need to configure...** link. You may need to refresh the browser window for this option to become visible.

    ![](https://user-images.githubusercontent.com/3944468/27846771-7c59ab32-60ee-11e7-9f32-7bdc945a2f77.png)

1. Now that you have a data connection, you can check the acknowledgment box and click **Initialize App**.

    ![](https://user-images.githubusercontent.com/3944468/27846770-7c5760ca-60ee-11e7-993b-5e3cb84f7d9d.png)

1. Once the app has been initialized, click **Add** to add a table.

    ![](https://user-images.githubusercontent.com/3944468/27846773-7c611656-60ee-11e7-88cf-61f77100ee1a.png)

1. Set the **Name** to **"todoitem"** and click **OK**. Note that you'll be using the default **"Allow anonymous access"** for all operations, which is okay for the purposes of this lab. However, you will want to consider the options more carefully for a production app.

    ![](https://user-images.githubusercontent.com/3944468/27846772-7c609d34-60ee-11e7-98fd-64dcaf7e928c.png)

<a name="Ex1Task4"></a>
### Task 4: Integrating the mobile app with the cloud service ###

1. Leave the browser window open to the portal and return to **Visual Studio for Mac**.

1. This solution has three projects: a portable **MobileCloudLab** project, an Android platform-specific **MobileCloudLab.Droid**, and a **MobileCloudLab.iOS** project. The portable project should contain as much functionality as possible so that it can be easily shared across the other platform projects without requiring additional work. However, when there is platform-specific code or functionality required, it can be either put in the portable project within platform checks or added directly to the platform-specific project. The platform-specific projects are also responsible for producing the appropriate output for their respective platforms so that you don't need to worry about those details when developing your business logic and data access in the portable library.

    ![](https://user-images.githubusercontent.com/3944468/27846775-7c67d7a2-60ee-11e7-8936-23c744101bc5.png)

1. From **Solution Explorer**, right-click the **MobileCloudLab** project node and select **Add > New File**.

    ![](https://user-images.githubusercontent.com/3944468/27846774-7c66fb8e-60ee-11e7-808c-17d8b2384b8d.png)

1. From the **General** category, select the **Empty Class** template. Set the **Name** to **"TodoItem"** and click **New**.

    ![](https://user-images.githubusercontent.com/3944468/27846776-7c6de3ea-60ee-11e7-835e-714072e5e66d.png)

1. The **TodoItem** class will represent the model for this application. It will include the data fields that are sent back and forth to the backend service. Replace the class definition with the code below. It's a simple object with **Id**, **Name**, **Done**, and **Version** properties. Also note that **JsonProperty** is used to specify how exactly the property names should be serialized.

    ```csharp
    public class TodoItem
    {
        string id;
        string name;
        bool done;
    
        [JsonProperty(PropertyName = "id")]
        public string Id
        {
            get { return id; }
            set { id = value; }
        }
    
        [JsonProperty(PropertyName = "text")]
        public string Name
        {
            get { return name; }
            set { name = value; }
        }
    
        [JsonProperty(PropertyName = "complete")]
        public bool Done
        {
            get { return done; }
            set { done = value; }
        }
    
        [Version]
        public string Version { get; set; }
    }
    ```
1. Since we started with an empty class, there are some compile errors in our pasted code (red squiggles). Right-click one of the **JsonProperty** attributes to see what **Quick Fix** options there are from Visual Studio. Click **Using Newtonsoft.Json;** to add an appropriate **using** statement to the top of the file.

    ![](https://user-images.githubusercontent.com/3944468/27846777-7c707772-60ee-11e7-9002-bdaa1be9940a.png)

1. Do the same for the **Version** attribute further down. Now there should be no more errors.

    ![](https://user-images.githubusercontent.com/3944468/27846778-7c778c60-60ee-11e7-95a9-53edfb0d8ad2.png)

1. Add another file to the **MobileCloudLab** project.

    ![](https://user-images.githubusercontent.com/3944468/27846779-7c7b5a3e-60ee-11e7-8282-fd3b19b48c95.png)

1. Name this class **TodoItemManager** and click **New**.

    ![](https://user-images.githubusercontent.com/3944468/27846781-7c7e5324-60ee-11e7-81c3-a234bd199065.png)

1. The **TodoItemManager** class will be responsible for managing communication with the backend service. Replace the default body of the **TodoItemManager** class with the code below. Be sure to update the service URL set in the constructor call to the **MobileServiceClient** constructor to match the URL of the service you created earlier. The remaining methods provide access to retrieve all open items and a way to insert/update an item.

    ```csharp
    static TodoItemManager defaultInstance = new TodoItemManager();
    MobileServiceClient client;
    IMobileServiceTable<TodoItem> todoTable;
    
    private TodoItemManager()
    {
        // TODO: Replace this URL with the URL to the service you created earlier.
        this.client = new MobileServiceClient(
            "https://mobilecloudlabjohndoe.azurewebsites.net");
        this.todoTable = client.GetTable<TodoItem>();
    }
    
    // Managing the to-do items via singleton instance.
    public static TodoItemManager DefaultManager
    {
        get { return defaultInstance; }
        private set { defaultInstance = value; }
    }
    
    // Retrieves all items that have not been completed.
    public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync()
    {
        IEnumerable<TodoItem> items = await todoTable
            .Where(todoItem => !todoItem.Done)
            .ToEnumerableAsync();
    
        return new ObservableCollection<TodoItem>(items);
    }
    
    // Saves a task by inserting or updating it, depending on whether it's new.
    public async Task SaveTaskAsync(TodoItem item)
    {
        if (item.Id == null)
        {
            await todoTable.InsertAsync(item);
        }
        else
        {
            await todoTable.UpdateAsync(item);
        }
    }
    ```
1. Use the **Quick Fix** feature from earlier to add **using** statements for each of the compile errors (red squiggles).

1. Open **MobileCloudLabPage.xaml**. This XAML file defines the first (and only) page of our app.

    ![](https://user-images.githubusercontent.com/3944468/27846780-7c7bb8ee-60ee-11e7-9b4c-7b09e4c45503.png)

1. Replace the default **Label** element with the markup below (inside the outermost **ContentPage** tags). This XAML defines a UI with a text box at the top for accepting new to-do items, which are added with a **+** button. The rest of the screen is filled with a list of existing to-do items. Each of those items may be marked as complete using their context behavior. There are also a few outstanding methods the XAML expects to call in its code-behind C# class (**OnAdd**, **OnSelected**, and **OnComplete**).

    ```xml
    <ContentPage.Content>
      <Grid RowSpacing="0">
        <Grid.RowDefinitions>
          <RowDefinition Height="Auto" />
          <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <StackLayout Grid.Row="0" BackgroundColor="#5ABAFF" Padding="10,30,10,5">
          <Label TextColor="#555555" Text="Mobile Cloud Lab" />
          <Grid>
            <Grid.ColumnDefinitions>
              <ColumnDefinition/>
              <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            <Entry x:Name="newItemName" Placeholder="Item name" />
            <StackLayout x:Name="buttonsPanel" Grid.Column="1" Orientation="Horizontal"
              HorizontalOptions="StartAndExpand">
              <Button Text="+" MinimumHeightRequest="30" Clicked="OnAdd" />
            </StackLayout>
          </Grid>
        </StackLayout>
        <ListView x:Name="todoList" ItemSelected="OnSelected" Grid.Row="1">
          <ListView.ItemTemplate>
            <DataTemplate>
              <ViewCell>
                <ViewCell.ContextActions>
                  <MenuItem Clicked="OnComplete" Text="Complete"
                    CommandParameter="{Binding .}"/>
                </ViewCell.ContextActions>
                <StackLayout HorizontalOptions="StartAndExpand" Orientation="Horizontal"
                  Padding="15,5,0,0">
                  <StackLayout Padding="5,0,0,0" VerticalOptions="StartAndExpand"
                    Orientation="Vertical">
                    <Label Text="{Binding Name}"  />
                  </StackLayout>
                </StackLayout>
              </ViewCell>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>
      </Grid>
    </ContentPage.Content>
    ```
1. Open **MobileCloudLabPage.xaml.cs**. This is the code-behind class that fulfills imperative initialization and event handlers for the XAML page you just updated.

    ![](https://user-images.githubusercontent.com/3944468/27846782-7c8383e4-60ee-11e7-9c96-59a803310709.png)

1. Replace the body of the **MobileCloudLabPage** class with the code below. It obtains an instance of the **TodoItemManager** class and uses it to relay calls from the UI to the backend service. The code should be easy to walk through.

    ```csharp
    TodoItemManager manager;
    
    public MobileCloudLabPage()
    {
        InitializeComponent();
        manager = TodoItemManager.DefaultManager;
    }
    
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        await RefreshItems();
    }
    
    async Task AddItem(TodoItem item)
    {
        await manager.SaveTaskAsync(item);
        todoList.ItemsSource = await manager.GetTodoItemsAsync();
    }
    
    async Task CompleteItem(TodoItem item)
    {
        item.Done = true;
        await manager.SaveTaskAsync(item);
        todoList.ItemsSource = await manager.GetTodoItemsAsync();
    }
    
    public async void OnAdd(object sender, EventArgs e)
    {
        var todo = new TodoItem { Name = newItemName.Text };
        await AddItem(todo);
        newItemName.Text = string.Empty;
        newItemName.Unfocus();
    }
    
    public async void OnComplete(object sender, EventArgs e)
    {
        var todo = (sender as MenuItem).CommandParameter as TodoItem;
        await CompleteItem(todo);
    }
    
    private async Task RefreshItems()
    {
        todoList.ItemsSource = await manager.GetTodoItemsAsync();
    }
    
    public void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        // prevents background getting highlighted
        todoList.SelectedItem = null;
    }
    ```
1. Use the **Quick Fix** feature from earlier to add using statements for each of the compile errors (red squiggles).

<a name="Ex1Task5"></a>
### Task 5: Running and debugging the application ###

1. If you have a physical device to test with, select **Tools > Manage Devices**. If not, skip ahead to the step 5 below to use the default emulator.

1. Launch **Xamarin Live Player** on your device and follow the directions to pair it with Visual Studio.

1. Once your device appears in the Visual Studio pairing dialog, close the dialog.

1. Your device should now be configured as the deployment target. If not, select it from the dropdown. You may have to change the project right next to the Play button from MobileCloudLab.iOS to MobileCloudLab.Android if you are deploying to an Android phone.

    ![](https://user-images.githubusercontent.com/3944468/27846783-7c85f7d2-60ee-11e7-95b2-c11a7e754e21.png)

1. Press **F5** to build, deploy, and run the project.

1. While the app is being deployed, open **MobileCloudLabPage.xaml.cs** and set a breakpoint on the first line of the **AddItem** method. You can do this by clicking in the margin next to the line or by setting the cursor on the line and pressing **F9**.

    ![](https://user-images.githubusercontent.com/3944468/27846784-7c8d7b6a-60ee-11e7-8ff1-9797019f6c59.png)

1. Once the app loads, type **"First item"** in the textbox and tap the **+** button to add it to the list.

    ![](https://user-images.githubusercontent.com/3944468/27846787-7c967d78-60ee-11e7-8000-f6a1c5c864e2.png)

1. The breakpoint will now trigger. Press **F10** to step into the method.

    ![](https://user-images.githubusercontent.com/3944468/27846785-7c919920-60ee-11e7-9a84-ef0950c4c02b.png)

1. You will now be in the **SaveTaskAsync** method of the **TodoItemManager** class. You can hover over the **item** parameter to view its properties.

    ![](https://user-images.githubusercontent.com/3944468/27846786-7c940b74-60ee-11e7-9202-fe7406e45cb1.png)

1. Press **F5** to continue execution.

1. The app should update with the new item added to the list of to-dos retrieved from the backend cloud service.

    ![](https://user-images.githubusercontent.com/3944468/27846789-7c9dadb4-60ee-11e7-84d0-27bc546a680d.png)

1. Return to the Azure portal browser window.

1. Click the **todoitem** table to view its data.

    ![](https://user-images.githubusercontent.com/3944468/27846788-7c9bff5a-60ee-11e7-834d-6be6ac9e74e6.png)

1. There will now be one record that matches the one added earlier. Note that you didn't even need to worry about the schema-it was all handled for you.

    ![](https://user-images.githubusercontent.com/3944468/27846791-7cb940ec-60ee-11e7-9ff5-3ad45e984f2c.png)

1. Back in the mobile app, slide the item left and tap **Complete** to mark the item as completed on iOS. For Android, tap and hold and tap on **Complete** in the top-right. This will update that item in the backend to have its **Completed** property set to true.

    ![](https://user-images.githubusercontent.com/3944468/27846790-7ca78cf8-60ee-11e7-871f-13d4997e94a0.png)

1. Return to the portal and click **Refresh** to see the updated field.

    ![](https://user-images.githubusercontent.com/3944468/27846792-7cbcc74e-60ee-11e7-994a-4e5748e5590c.png)

<a name="Summary"></a>
## Summary ##

In this lab, you've learned how to get started developing cloud-connected mobile apps with Visual Studio for Mac. To learn more about creating mobile apps with Xamarin, check out [Xamarin University](https://www.xamarin.com/university).

