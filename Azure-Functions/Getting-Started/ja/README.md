# Visual Studio for Mac で Azure Functions を作成する

## 概要
Azure Functions は開発を加速するための、クラウドにおけるイベントベースのサーバレスコンピューティング体験です。アプリケーション全体やそれを実行するインフラを心配することなく、問題に必要なコードだけ集中することができます。Functions は開発をより生産的にすることができ、C#、F#、Node.js、Python、PHP などの開発言語を使用することができます。費用が発生するのはコードが実行されている時間だけで、必要に応じてスケーリングを Azure に任せることができます。Azure Functions を使用すると、Microsoft Azure でサーバーレスアプリケーションを開発できます。

このラボでは、Visual Studio for Mac を使用して Azure Functions を作成する方法を学習します。また、Azure Functions の開発者が利用できる多くの種類のバインディングとトリガーの1つである Azure ストレージテーブルと統合します。

## 目標
* ローカル Azure Functions の作成とデバッグ
* Web と Azure ストレージリソースとの統合
* 複数の Azure Functions を含むワークフローの編成

## 前提条件
* Azure Functions 開発拡張機能をインストールした Visual Studio for Mac(https://www.visualstudio.com/vs/visual-studio-mac)
 * Azure Functions サポートはプレビューとして利用可能なため、現在このラボは Visual Studio for Mac を[アルファチャネルに切り替える](https://docs.microsoft.com/ja-jp/visualstudio/mac/update)必要があることにご注意ください。
 * Azure Functions の開発拡張機能をインストールするためには、トップメニューの **拡張機能...** をクリックして、**ギャラリー** タブの **IDE Extensions** ノードの **Azure Functions development（preview）** 拡張を見つけます。
 * Azure サブスクリプション（https://azure.microsoft.com/ja-jp/free/ で無料利用可能です）


## 対象者
このラボは、C# に精通した開発者を対象としていますが、深い経験は必要ありません。

## エクササイズ1: Visual Studio for Mac で Azure Functions を作成する

### タスク1: Azure Functions プロジェクトの作成

1\. **Visual Studio for Mac** を起動してください。

2\. ファイル > 新しいソリューション...を選択してください。

3\. **クラウド** > **全般** カテゴリーから Azure Functions テンプレートを選択してください。Azure Functions をホストする .NET クラスライブラリを作成するには C# を使用します。**次へ** をクリックしてください。

<img src=https://user-images.githubusercontent.com/7035446/31535704-fe5f255c-b036-11e7-8fcf-cf57692063ef.png width="620px">

4\. **プロジェクト名** を **"AzureFunctionsLab"** に設定し、**作成**をクリックします。

<img src=https://user-images.githubusercontent.com/7035446/31535876-a84b33d0-b037-11e7-96fb-f8e27d43daee.png width="620px">

5\. **ソリューションエクスプローラ**でノードを展開してください。デフォルトのプロジェクトテンプレートには、さまざまな Azure WebJob パッケージや Newtonsoft.Json パッケージへの NuGet 参照が含まれています。デフォルトで2つの空の接続文字列設定があります。

<img src=https://user-images.githubusercontent.com/7035446/31535890-b3d95736-b037-11e7-9422-f8754001a004.png width="275px">

### タスク2: Azure ストレージアカウントの作成

1\. ブラウザを開き、https://portal.azure.com で自分のアカウントにログインします。

2\. **新規**をクリックし、**storage**を検索して **enter** をクリックします。

<img src=https://user-images.githubusercontent.com/7035446/31535904-c1707924-b037-11e7-8c96-a2d089043be2.png width="335px">

3\. **ストレージアカウント - blob, file, table, queue**をクリックして新しい Azure ストレージアカウントを作成してください。

<img src=https://user-images.githubusercontent.com/7035446/31535916-cdbe96b6-b037-11e7-9010-093679bea071.png width="550px">

4\. **作成**をクリックしてください。

<img src=https://user-images.githubusercontent.com/7035446/31535933-dc68751a-b037-11e7-92da-510268bf7fd0.png width="530px">

5\. **名前**にグローバルに一意な名前を入力し、**リソースグループ** に再利用します。**"azurefunctionshoge"** のような名前を使用できます。**作成** をクリックしてください。

<img src=https://user-images.githubusercontent.com/7035446/31535945-e6b95b06-b037-11e7-8138-88b81f112d25.png width="275px">

<img src=https://user-images.githubusercontent.com/7035446/31535961-f1c2582c-b037-11e7-99a3-457f4f112a9c.png width="275px">

6\. アカウントの作成には1分ほどかかります。成功通知をクリックしてダッシュボードを開いてください。

<img src=https://user-images.githubusercontent.com/7035446/31535979-00186c0e-b038-11e7-8a77-e8ff734c6764.png width="375px">

7\. **Access keys**タブを選択してください。

<img src=https://user-images.githubusercontent.com/7035446/31535990-0c7b4bd8-b038-11e7-964d-6266aa1d2147.png width="215px">

8\. 1つ目の **接続文字列** をコピーしてください。これは後ほど Azure ストレージと Azure Functions を統合するために使用します。

<img src=https://user-images.githubusercontent.com/7035446/31536002-15ee990e-b038-11e7-998e-26eba7111077.png width="460px">

9\. **Visual Studio for Mac** に戻り、**local.settings.json** に **AzureWebJobsStorage** の設定としてコネクションストリングをペーストしてください。これで、リソースにアクセスする必要のある Functions の属性の設定名を簡単に参照できます。

<img src=https://user-images.githubusercontent.com/7035446/31536013-1ff30ade-b038-11e7-84a3-849079146e07.png width="495px">

### タスク3: Azure Function の作成とデバッグ

1\. これでコードを追加する準備が整いました。 .NET クラスライブラリで作業する場合、Azure Functions は静的メソッドとして追加されます。ソリューションエクスプローラで **AzureFunctionsLab** プロジェクトノードを右クリックし、 **追加** > **新しいファイル...** を選択してください。

<img src=https://user-images.githubusercontent.com/7035446/31536026-2b11934a-b038-11e7-9d41-91247abbdfdc.png width="475px">

2\. **General** カテゴリーから **空のクラス** テンプレートを選択してください。**名前** を **"Functions"** に設定し、**新規(N)** をクリックしてください。

<img src=https://user-images.githubusercontent.com/7035446/31536047-3cd2df62-b038-11e7-8c51-5bca76d06e05.png width="510px">

3\. 新しいファイルの先頭に、以下の using ステートメントを追加します。これらは、 LINQ や HTTP などの機能や、Azure Functions スタックのオブジェクトを扱うためのさまざまなヘルパーメソッドへのアクセスを提供します。

```csharp
using System.Linq;
using System.Net;
using System.Net.Http;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
```

4\. 以下のメソッドを最初の Azure Function としてクラスに追加します。

```csharp
[FunctionName("Add")]
public static int Add(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)]
    HttpRequestMessage req,
    TraceWriter log)
{
    int x = 1;
    int y = 2;

    return x + y;
}
```

5\. メソッド定義を1つずつ見ていきましょう。最初はこのメソッドを **Azure Function** としてマークする **FunctionName** 属性です。単一のパラメータを取り、関数のパブリックな名称を表します。実際のメソッド名自体と一致する必要はありません（ここでは一致させています）。

<img src=https://user-images.githubusercontent.com/7035446/31536058-49ea2886-b038-11e7-8d7a-e1cae0b95c1c.png width="460px">

6\. 次に、メソッドは **public static** メソッドとしてマークされていますがこれは必須です。また、戻り値が **int** であることに気づくでしょう。メソッド属性を使用して別途指定されない限り、Azure Function の void でない戻り値はレンダリングされ、テキストとしてクライアントに返されます。デフォルトでは XML として返されますが、簡単に JSON に変更できます。これについては後でラボで行います。

<img src=https://user-images.githubusercontent.com/7035446/31536081-5ccd199a-b038-11e7-96a9-b61d2b13e099.png width="460px">

7\. 最初のパラメータには、**HttpTrigger** 属性が設定されています。これは、このメソッドが **HTTP** リクエストによって呼び出されるを示します。この属性は、メソッドの認可レベルとそれがサポートする動詞（この場合は **GET** のみ）も指定します。オプションで、メソッドへのパスをオーバーライドする **ルート** を定義し、パスから変数を自動的に抽出する方法を提供することもできます。ルートはここでは null なので、このメソッドのパスはデフォルトで **/api/Add** になります。

<img src=https://user-images.githubusercontent.com/7035446/31536086-66c27d5a-b038-11e7-99d7-8702984e3273.png width="460px">


8\. メソッドの最後のパラメータは、診断とエラーのメッセージを記録するために使用できる **TraceWriter** です。

<img src=https://user-images.githubusercontent.com/7035446/31536108-7007df2c-b038-11e7-9718-ef99845717e9.png width="460px">

9\. メソッドの **return** 行にブレークポイントを設定するには、行の余白をクリックするか、行にカーソルを合わせて **F9** キーを押します。

<img src=https://user-images.githubusercontent.com/7035446/31536116-78d3d8ea-b038-11e7-8e1e-4f6b3b840de1.png width="565px">

10\. デバッグセッションでプロジェクトをビルドして実行するには、**F5** キーを押すか、**実行 > デバッグの開始** を選択します。代わりに**実行**ボタンをクリックしても大丈夫です。これらはすべて同じタスクを実行します。このラボの残りの部分は**F5** を使用しますが、一番快適な方法で実行してください。

<img src=https://user-images.githubusercontent.com/7035446/31536123-81807c28-b038-11e7-9e06-97560fcc5a5b.png width="270px">

11\. **表示 > パッド > アプリケーション出力 - AzureFunctionsLab** を選択してください。

12\. プロジェクトがビルドされた後、メソッドの属性とファイルの規約に基づいて、Azure Function を検出するプロセスを順を追って説明します。この場合、単一の Azure Function を検出し、1つのジョブ関数を「生成」します。

<img src=https://user-images.githubusercontent.com/7035446/31536160-99e326b2-b038-11e7-836f-ef276ba1d15c.png width="620px">

13\. 起動メッセージの一番下に、Azure Function ホストは HTTP トリガー API の URL を表示します。1つだけ表示されるべきです。そのURLを新しいブラウザタブで開いてください。このラボの残りでは、Azure ポータル用に1つのタブを開き、Azure Function コードをテストするために別のタブを開いておく必要があります。

<img src=https://user-images.githubusercontent.com/7035446/31536168-a3300ece-b038-11e7-9a0c-b8e204bf56f1.png width="470px">

14\. ブレークポイントはすぐにトリガーされます。Webリクエストは関数にルーティングされ、デバッグできるようになりました。x 変数の上にマウスを置くと値が表示されます。これで、Visual Studio デバッガの機能を完全に使用でき、プロジェクトを洗練するために必要なタスクに取り掛かることができます。

<img src=https://user-images.githubusercontent.com/7035446/31536181-ad5e4f96-b038-11e7-9ae6-e21e488c829e.png width="310px">

15\. 以前に追加したのと同じ方法でブレークポイントを削除します（マージンをクリックするか、行を選択して **F9** を押します）。

16\. **F5** キーを押して実行を続行します。

17\. ブラウザでは、メソッドのXML結果がレンダリングされます。期待どおり、ハードコーディングされた加算演算は妥当な和を生成します。Safari で「3」だけが表示されている場合は、**Safari > 環境設定 > 詳細**に進み、**メニューバーに"開発"メニューを表示**チェックボックスをオンにしてページをリロードします。

<img src=https://user-images.githubusercontent.com/7035446/31536187-b69ccbfa-b038-11e7-8d53-546a765f7ee3.png width="545px">

18\. **Visual Studio for Mac** で、**停止**ボタンをクリックしてデバッグセッションを終了します。テスト中のコードを変更する度に再起動（停止して実行）するのを忘れないでください。

<img src=https://user-images.githubusercontent.com/7035446/31536196-c0316482-b038-11e7-8e3c-a39a46cc124e.png width="270px">

19\. **Add** メソッドで **x** と **y** の定義を次のコードに置き換えてください。このコードは、URLのクエリ文字列から値を抽出し、提供されたパラメータに基づいて加算を動的に実行できるようにします。

```csharp
int x = int.Parse(req.GetQueryNameValuePairs()
            .FirstOrDefault(q => string.Compare(q.Key, "x", true) == 0)
                        .Value);
int y = int.Parse(req.GetQueryNameValuePairs()
            .FirstOrDefault(q => string.Compare(q.Key, "y", true) == 0)
                        .Value);
```

20\. **F5** キーを押してアプリケーションをビルドして実行します。

21\. ブラウザのウィンドウに戻り、文字列 "**/?x=2&y=3**" をURLに追加します。URL全体は "http://localhost:7071/api/Add?x=2&y=3" になります。新しいURLに移動してください。

22\. 今回は結果に新しいパラメータが反映されるはずです。異なる値で数回試してみてください。

<img src=https://user-images.githubusercontent.com/7035446/31536206-c94d227c-b038-11e7-87f2-247a70456248.png width="545px">

23\. デバッグセッションを停止します。

### タスク4: function.jsonの操作

1\. Visual Studio for Mac は、ライブラリに定義されている Azure Function のジョブ関数を「生成」していることに先ほど触れました。これは、Azure Functions が実行時にメソッド属性を実際に使用するのではなく、コンパイル時のファイルシステム規約を使用して、Azure Functions が利用可能になる場所と方法を設定するためです。**ソリューションエクスプローラ**で **AzureFunctionsLab** プロジェクトノードを右クリックし、**Finderで表示** を選択します。

<img src=https://user-images.githubusercontent.com/7035446/31536215-d1f6e886-b038-11e7-9ff1-dc8f42a9eefa.png width="375px">

2\. **bin/Debug/net461**が見つかるまでファイルシステムをたどってください。**Add** という名前のフォルダが見つかるでしょう。このフォルダは、C# コードの関数名属性に対応するように作成されています。そのフォルダを展開すると、 **function.json** が見つかるでしょう。これはランタイムが Azure Function 自体をホストして管理するために使用するファイルです。コンパイル時サポートがない他の言語モデル（ C# スクリプトや JavaScript など）では、これらのフォルダを手動で作成して管理する必要があります。C# 開発者向けにはビルド時に属性メタデータから自動的に生成されます。 **function.json** を開いてください。

<img src=https://user-images.githubusercontent.com/7035446/31536228-db2a5fbe-b038-11e7-98be-2b2cd72680d9.png width="615px">

3\. C# の属性に精通していると、この **JSON** はかなり理解しやすいはずです。しかし、以前に明示的に議論していなかった項目がいくつかあります。たとえば、各 **binding** は **direction** が設定されている必要があります。ご想像の通り、**in** はパラメータが入力されていることを意味し、**out** はパラメータが戻り値（ **$return** 経由）またはメソッドの **out** パラメータのいずれかであることを示します。また、アセンブリ内の **scriptFile** （相対パス）と **entryPoint** メソッド（ public と static ）を指定する必要があります。次のいくつかのステップでは、このモデルを使用してカスタム関数パスを追加するので、このファイルの内容をクリップボードにコピーします。

<img src=https://user-images.githubusercontent.com/7035446/31536241-e6a68ba6-b038-11e7-9000-99d9aeab1b4b.png width="530px">


4\. **ソリューションエクスプローラ**で **AzureFunctionsLab**プロジェクトノードを右クリックし、**追加 > 新しいフォルダー**を選択します。新しいフォルダに **Adder** という名前を付けます。デフォルトの規約では、このフォルダ名は **api/Adder** などの **API** へのパスを定義します。

<img src=https://user-images.githubusercontent.com/7035446/31536270-f3b6c63a-b038-11e7-8f47-8f1b0a3375ea.png width="555px">


5\. **Adder** フォルダを右クリックし、**追加 > 新しいファイル...** を選択してください。

<img src=https://user-images.githubusercontent.com/7035446/31536278-fbef63b6-b038-11e7-9ca4-579ee081bf35.png width="420px">

6\. Web カテゴリと **空の JSON ファイル** テンプレートを選択します。 **名前** を **function**に設定し、**新規(N)** をクリックしてください。

<img src=https://user-images.githubusercontent.com/7035446/31536292-05e9cd8e-b039-11e7-846f-cc7559cede97.png width="500px">

7\. 他の **function.json** の中身を新しく作成したファイルのデフォルトの中身にペーストしてを置き換えてください。

8\. 最初のバインディングの最後（ **"name"： "req"** 行の後）に、以下のプロパティを追加します。前の行にカンマを含めることを忘れないでください。これにより、デフォルトのルートが上書きされ、（存在する場合）パスから **int** パラメータが抽出され、**x** 、 **y**という名前のメソッドパラメータに配置されます。

```json
"route": "Adder/{x:int?}/{y:int?}"
```

9\. また、ファイルの下部にある **entryPoint** プロパティを更新して、以下に示すような **Add2** というメソッドを使用します。これは、パス **api/Adder...** が任意の名前（ここでは **Add2** ）を持つ適切なメソッドにマップできることを示します。

```json
"entryPoint": "AzureFunctionsLab.Functions.Add2"
```

10\. 最終的に **function.json** ファイルは次のようになります。

<img src=https://user-images.githubusercontent.com/7035446/31536307-105c5b38-b039-11e7-819c-a808fa106827.png width="375px">

11\. すべての作業を完了するために必要な最後のステップは、Visual Studio for Mac にこのファイルを変更するたびに出力ディレクトリの同じ相対パスにコピーするように設定することです。

<img src=https://user-images.githubusercontent.com/7035446/31536313-188cccc0-b039-11e7-9666-9a65835b0dee.png width="270px">

12\. **Functions.cs** に、期待される機能を実現するために次のメソッドを追加します。これは、属性を使用せず、**x** と **y** の明示的なパラメータを持つことを除けば、**Add** と非常に似ています。

```csharp
public static int Add2(
    HttpRequestMessage req,
    int x,
    int y,
    TraceWriter log)
{
    return x + y;
}
```

13\. **F5** を押してプロジェクトをビルド、実行してください。

14\. ビルドが完了し、プラットフォームがスピンアップすると、新しく追加されたメソッドにマッピングされるリクエストに対して利用可能な第2のルートが存在することを示します。

<img src=https://user-images.githubusercontent.com/7035446/31536325-23ca6502-b039-11e7-9327-35da5306c5a4.png width="440px">

15\. ブラウザに戻り、 **http://localhost:7071/api/Adder/3/5** にアクセスしてみてください。

16\. メソッドは再び動作し、パスからパラメータを引き出し、合計を生成します。

<img src=https://user-images.githubusercontent.com/7035446/31536338-2c24e966-b039-11e7-8649-c750f8a1e43a.png width="530px">

17\. **Visual Studio for Mac** に戻り、デバッグセッションを終了します。

### タスク5: Azureストレージテーブルの操作

1\. 多くの場合、構築するサービスは、これまでに構築したサービスよりはるかに複雑であり、実行にはかなりの時間とインフラストラクチャが必要になることがあります。その場合、リソースが利用可能になったときに処理待ちの要求を受け入れることが効果的であるかもしれません。Azure Functions はこれをサポートしています。他のケースでは、データを一元的に保存したい場合があります。Azure ストレージテーブルを使用すると、すぐにデータを保存できます。以下のクラスを **Functions.cs** に追加してください。それは名前空間の中に入るべきですが、**Functions** クラスの外側に記述します。

```csharp
public class TableRow : TableEntity
{
    public int X { get; set; }
    public int Y { get; set; }
    public int Sum { get; set; }
}
```

2\. **Functions** クラス内で、次のコードを追加して別の関数を導入します。これは **HTTP** レスポンスを伴わないという点でこれまでと異なることに注意してください。最終行はパラメタや合計だけでなく、後で簡単に取得できるようにする重要な情報（**PartitionKey** と **RowKey**）が集まった新しい **TableRow** を返します。メソッド内のコードは、関数がいつ実行されるかを簡単に知ることができるように **TraceWriter** を使用します。

```csharp
[FunctionName("Process")]
[return: Table("Results")]
public static TableRow Process(
    [HttpTrigger(AuthorizationLevel.Function, "get",
        Route = "Process/{x:int}/{y:int}")]
    HttpRequestMessage req,
    int x,
    int y,
    TraceWriter log)
{
    log.Info($"Processing {x} + {y}");

    return new TableRow()
    {
        PartitionKey = "sums",
        RowKey = $"{x}_{y}",
        X = x,
        Y = y,
        Sum = x + y
    };
}
```

3\. **F5** を押してプロジェクトをビルド、実行してください。

4\. ブラウザタブで **http://localhost:7071/api/Process/4/6** にアクセスしてください。これにより別のメッセージがキューに入れられ、最終的に別の行がテーブルに追加されます。

5\. **Visual Studio for Mac** に戻り、**4 + 6** のリクエストのアプリケーションログを監視します。

<img src=https://user-images.githubusercontent.com/7035446/31536351-36205a18-b039-11e7-92fb-f69ed866abe3.png width="610px">

6\. 同じ URL へのリクエストを更新するためにブラウザに戻ります。今回は、**Process** メソッドの後にエラーが表示されます。これは、コードが既に存在するパーティションと行のキーの組み合わせを使用して、Azure テーブルストレージテーブルに行を追加しようとしているためです。

7\. デバッグセッションを終了してください。

8\. エラーを修正するには、**TraceWriter** パラメーターの直前のメソッド定義に次のパラメーターを追加します。このパラメータは Azure Functions のプラットフォームに、結果を格納するために使用していた **PartitionKey** の **Results** テーブルから **TableRow** を取得するように指示します。しかし、真の魔法のいくつかは、まったく同じメソッドの他の **x** と **y** パラメータに基づいて **RowKey** が動的に生成されていることに気づいたときに発動します。その行がすでに存在する場合、メソッドが開発者が必要とする追加作業を必要とせずに開始されたとき、**tableRow** はその行を持ちます。行が存在しない場合は、null になります。このような効率性により、開発者はインフラストラクチャではなく、重要なビジネスロジックに集中することができます。それ以外の場合、関数は以前と同じように継続します。

```csharp
[Table("Results", "sums", "{x}_{y}")]
TableRow tableRow,
```

9\. 以下のコードをメソッドの先頭に追加します。**tableRow** が null でない場合、要求された操作の結果がすでにあるため、すぐに結果を返すことができます。これはデータを返す最も堅牢な方法ではないかもしれませんが、複数のスケーラブルな層に渡る非常に洗練された操作を、少数のコードで編成できることを示しています。

```csharp
if (tableRow != null)
{
    log.Info($"{x} + {y} already exists");
    return null;
}
```

10. **F5** を押してプロジェクトをビルド、実行してください。

11. ブラウザのタブで、 **http://localhost:7071/api/Process/4/6** で URL を更新してください。このレコードのテーブル行が存在するため、エラーを起こさずにすぐにリターンすべきです。HTTP 出力がないので、アプリケーション出力でログを見ることができます。

<img src=https://user-images.githubusercontent.com/7035446/31536360-3e84d832-b039-11e7-9c63-a6d07767eb6c.png width="500px">

12. **http://localhost:7071/api/Process/5/7** など、まだテストされていない組み合わせを反映するように URL を更新してください。アプリケーションログのメッセージに注意してください。これは、テーブルの行が（期待どおりに）見つからなかったことを示しています。

<img src=https://user-images.githubusercontent.com/7035446/31536367-48a94834-b039-11e7-9ff8-73a424919c9a.png width="500px">

13. **Visual Studio for Mac** に戻り、デバッグセッションを終了してください。

14. 最後に、複数の入力レコードの扱い見てみましょう。特定の **TableRow** を指定するのではなく、同じ属性を使用して **IQueryable** を要求することができます。ランタイムは、必要な適切なリソースを使用して入力します。下記のコードを追加して、現在作業している Azure テーブルに現在存在するすべてのアイテムをリストする **List** 関数を作成します。また、レスポンスの MIME タイプが **application/json** であることを指定しているため、ランタイムは自動的にJSONとしてレンダリングします。

```csharp
[FunctionName("List")]
public static HttpResponseMessage List(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)]
    HttpRequestMessage req,
    [Table("Results", "sums")]
    IQueryable<TableRow> table,
    TraceWriter log)
{
    return req.CreateResponse(HttpStatusCode.OK, table, "application/json");
}
```

15. **F5** を押してプロジェクトをビルド、実行してください。

16. ブラウザのタブで **http://localhost:7071/api/List** にアクセスしてください。これにより、Azure テーブルのすべてのアイテムのリストがプルダウンされ、JSON としてレンダリングされます。

<img src=https://user-images.githubusercontent.com/7035446/31536373-50ef4340-b039-11e7-819f-cf18abffd86d.png width="500px">

## まとめ
このラボでは、Visual Studio for Mac で Azure Functions を作成する方法を学びました。
