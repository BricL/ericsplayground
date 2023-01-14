# 目錄：

* [C# Reflection](#1)
* [C# 字串內插補點$ + string](#2)
* [C# 7.0 Function Return 多個值](#3)
* [C# gRPC起手式](#4)

---

<span id="1"></span>

## :sunny:C#  Reflection

.NET Framework 有二次編譯的概念，為了不同平台上使用，加上一層中間層，更靈活：

1. 透過VS編譯器　編譯成dll/exe
2. 點擊exe的時候，他有一個依賴的環境叫做CLR
3. dll/exe裡面包含兩大塊 IL（中間語言），metadata（元數據）

metadata會紀錄這 dll/exe 裡面有哪些東西，而反射主要在做這塊。

### 優點：

1. 反射提高了程序的靈活性和擴展性。
2. 降低耦合性，提高自適應能力。
3. 它允許程式創建和控制任何類的物件，無需提前硬編碼目標類。

### 缺点：

1. 性能問題：使用反射基本上是一種解釋操作，用於字段和方法接入時要遠慢於直接代碼。 因此反射機制主要應用在對靈活性和拓展性要求很高的系統框架上，普通程式不建議使用。
2. 使用反射會模糊程序內部邏輯; 程式師希望在原始程式碼中看到程式的邏輯，反射卻繞過了原始程式碼的技術，因而會帶來維護的問題，反射代碼比相應的直接代碼更複雜。

### 參考資源：

* C# 反射（Reflection)
  https://www.runoob.com/csharp/csharp-reflection.html
* 玩轉C#之【反射 Reflection 射爆妳】
  https://ithelp.ithome.com.tw/articles/10288591?sc=iThelpR
* REFLECTION-使用反射執行方法的7種方式
  https://blog.kkbruce.net/2017/01/reflection-method-invoke-7-ways.html
* C# - Reflection
  https://www.tutorialspoint.com/csharp/csharp_reflection.htm

<span id="2"></span>

## :sunny:C# 字串內插補點$ + string

以前在用C#處理字串中有變數時，已經習慣使用這種：

```csharp
string.Format("My name is {0}, I am {1} years old", szName, nAge)
```

但前一陣子在使用 Javascript 時，有看到可以用`xxxx`(非 single quote，Esc 下面那個)來夾住字串，中間可以直接填變數的寫法。

C#6 之後的版本也有類似的用法了，以後在填字串時可以直覺一些用法很簡單，在 "xxxx" 前加上$，例如：

```csharp
string.Format("My name is {0}, I am {1} years old", szName, nAge); 
// 可以寫成
$"My name is {szName}, I am {nAge} years old"
```

<span id="3"></span>

## :sunny:C# 7.0 Function Return 多個值

```csharp
private static (int ab1, int ab2) Add_Multiply (int a, int b) {
    return (a + b, a * b);
}

static void Main (string[] args) {
    int a = 10;
    int b = 20;
    (int a_plus_b, int a_mult_b) = Add_Multiply(a, b);
    Console.WriteLine(a_plus_b);
    Console.WriteLine(a_mult_b);
}
```

<span id="4"></span>

## :sunny:C#  gRPC起手式

### RPC 定義：

PRC(Remote Procedure Call) 簡單說就是 “用戶端” 在與 “服務端” 溝通時，感覺就像在呼叫本地端的 API 一樣，依據 funcion 定義傳入所需參數，等待回覆結果，如下：

```csharp
var reply = await client.SayHelloAsync( new HelloRequest { Name = "GreeterClient" });
```

從上面的範例看來，是不是完全不像跟 “服務端” 發出請求吧！一切就像個本地非同步 function 呼叫，這就是 PRC(Remote Procedure Call) 的精神。

### Protobuf：

由 Google 主導，一種描述式語法用來描述傳輸間的資料結構。有別於JSON的文字格式，Protobuf 在經過編譯(Protobuf Compiler)後，可以成為任何語言(JAVA、C#、Go...etc）的型別定義，藉此達到 序列化/反序列化，讓傳輸採資料更小速、度更快的二進位格式。

```protobuf
syntax = "proto3";

option csharp_namespace = "GrpcGreeter";

package greet;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply);
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings.
message HelloReply {
  string message = 1;
}
```

這裡 Greeter.proto 中定義了：

* message HelloRequest，向服務器端發出請求的格式
* message HelloReply，回覆客戶端請求的回覆格式
* service Greeter，一個 RPC 的 funciton call 的定義

這些定義再透過 Protobuf Compiler 編譯後，就能成為你指定想要使用的語言 class 與 funciton 定義。客戶端與服務端的實踐，可以是不同語言，但彼此間的溝通與傳輸的資料定義，都會遵守上面 *.proto 描述的定義。

### service：

作為一個 Remote Function Call 的定義，在經過編譯後會分別對 “客戶端”、“服務端” 產生 2 種不同的 Type。

對 “服務端” 來說：

* 會生成一個 base class 做為基底，你需要繼承這個基底 class 實踐 Greeter.proto 所描述的 RPC。

  ```csharp


  [grpc::BindServiceMethod(typeof(Greeter), "BindService")]
  public abstract partial class GreeterBase
  {
      [global::System.CodeDom.Compiler.GeneratedCode("grpc_csharp_plugin", null)]
      public virtual global::System.Threading.Tasks.Task<global::myGrpcTesting.HelloR
  eply> SayHello(global::myGrpcTesting.HelloRequest request, grpc::ServerCallContext context)
      {
          throw new grpc::RpcException(new grpc::Status(grpc::StatusCode.Unimplemented, ""));
      }
  }

  /// 以下為實作部分...

  using Grpc.Core;
  using myGrpcTesting;

  namespace myGrpcTesting.Services;

  public class GreeterService : Greeter.GreeterBase
  {
      private readonly ILogger<GreeterService> _logger;

      public GreeterService(ILogger<GreeterService> logger)
      {
          _logger = logger;
      }

      public override Task<HelloReply> SayHello(HelloRequest request, ServerCallContext context)
      {
          return Task.FromResult(new HelloReply
          {
              Message = "Hello " + request.Name
          });
      }
  }
  ```

對用戶端來說：

* 會生成一個呼叫 RPC 的類別，透過給予連線設定進行建立，然後呼叫等待結果
  ```csharp
  // The port number must match the port of the gRPC server.
  var channel = GrpcChannel.ForAddress("https://localhost:7042");
  var client = new Greeter.GreeterClient(channel);

  var reply = await client.SayHelloAsync(new HelloRequest { Name = "GreeterClient" });

  Console.WriteLine("Greeting: " + reply.Message);
  Console.WriteLine("Press any key to exit...");
  Console.ReadKey();
  ```

### message：

根據 *.proto 中的定義，透過編譯轉變成目標語言的 Type，且具備序列化WriteTo()/反序列化 Paser.PaseFrom() 的能力。

```csharp
using Google.Protobuf;
...
Person john = ...; // Code as before
using (var output = File.Create("john.dat"))
{
    john.WriteTo(output);
}
```

```csharp
Person john;
using (var input = File.OpenRead("john.dat"))
{
    john = Person.Parser.ParseFrom(input);
}
```

### 套件功能說明：

* gRPC Core
  最底層的實作
* gRPC C# API
  在gRPC Core之上的包裝，貼近NET開發者使用
* Grpc.Net.Client
  在gRPC C# API之上的包裝，最貼近NET開發者使用
* Grpc.Tools
  gRPC and Protocol Buffer compiler for managed C# and native C++ projects.
* Google.Protobuf

### protoc 編譯器：

負責編譯 .proto 內的描述語言成目標語言，如：Person.proto >> Person.cs。安裝方式請參考[官方網站](https://grpc.io/docs/protoc-installation/)。

### grpc_cshapr_plugin 外掛器：

protoc 對 c# 的編譯上不支持 service 關鍵字，需要用 [NuGet 下載外掛 grpc tools](//https://www.nuget.org/packages/Grpc.Tools/) 裡面包含 grpc_cshapr_plugin.exe 的 protoc 外掛，來完成 service 的編譯。

一個 person.proto 在編譯後會有 2 個檔案：

1. Person.cs (定義傳輸資料)
2. PersonGrpc.cs (定義遠端 function call 的呼叫) 在 Unity 與 Server 採 gRPC 連線下，用戶端同時需要這兩個檔案。

### Sample：

Test.proto 範例

```csharp
	syntax = "proto3";
	option csharp_namespace = "GrpcGreeter";
	package greet;
	// The greeting service definition.
	service Greeter {
	  // Sends a greeting
	  rpc SayHello (HelloRequest) returns (HelloReply);
	}
	// The request message containing the user's name.
	message HelloRequest {
	  string name = 1;
	}
	// The response message containing the greetings.
	message HelloReply {
	  string message = 1;
	}
```

編譯指令

> tools\protoc.exe  -I protos protos\test.proto
> --csharp_out=output
> --grpc_out=output
> --plugin=protoc-gen-grpc=tools\grpc_csharp_plugin.exe

---

<br>

[返回目錄](https://github.com/BricL/ericsplayground/blob/main/README.md)
