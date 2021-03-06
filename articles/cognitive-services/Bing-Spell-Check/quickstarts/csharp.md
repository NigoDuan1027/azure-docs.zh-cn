---
title: 快速入门：使用必应拼写检查 REST API 和 C# 检查拼写
titlesuffix: Azure Cognitive Services
description: 开始使用必应拼写检查 REST API 检查拼写和语法。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: a33dfe2e20cdb6c1944d4be89692ec1da5a5482e
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889658"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-c"></a>快速入门：使用必应拼写检查 REST API 和 C# 检查拼写

根据此快速入门中的说明对必应拼写检查 REST API 进行第一次调用。 此简单的 C# 应用程序将向 API 发送请求并返回一系列建议的更正。 虽然此应用程序是使用 C# 编写的，但 API 是一种 RESTful Web 服务，与大多数编程语言兼容。 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingAutosuggestv7.cs) 上提供了此应用程序的源代码。

## <a name="prerequisites"></a>先决条件

* 任何版本的 [Visual Studio 2017](https://www.visualstudio.com/downloads/)。
* [Json.NET](https://www.newtonsoft.com/json) 框架，可以 NuGet 包的形式提供。
* 如果使用的是 Linux/MacOS，则可使用 [Mono](http://www.mono-project.com/) 运行此应用程序。

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>创建并初始化项目

1. 在 Visual Studio 中创建一个名为 `SpellCheckSample` 的新控制台解决方案。 然后将以下命名空间添加到主代码文件。
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Newtonsoft.Json;
    ```

2. 为 API 终结点、订阅密钥和要进行拼写检查的文本创建变量。

    ```csharp
    namespace SpellCheckSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/spellcheck?";
            static string key = "enter your key here";
            //text to be spell-checked
            static string text = "Hollo, wrld!";
        }
    }
    ```

3. 为搜索参数创建变量。 将市场代码追加​​到 `mkt=`，并将拼写检查模式追加​​到 `&mode=`。
    
    ```csharp
    static string params_ = "mkt=en-US&mode=proof";
    ```

## <a name="create-and-send-a-spell-check-request"></a>创建和发送拼写检查请求

1. 创建名为 `SpellCheck()` 的异步函数，以便向 API 发送请求。 创建 `HttpClient`，并将订阅密钥添加到 `Ocp-Apim-Subscription-Key` 标头。 然后在函数中执行以下步骤。

    ```csharp
    async static void SpellCheck()
    {
        HttpClient client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

        HttpResponseMessage response = new HttpResponseMessage();
        //...
    }

2. Create the URI for your request by appending your host, path, and parameters. 
    
    ```csharp
    string uri = host + path + params_;
    ```

3. 创建含有 `KeyValuePair` 对象（其中包含文本）的列表，并将其用于创建 `FormUrlEncodedContent` 对象。 设置标头信息，并使用 `PostAsync()` 发送请求。

    ```csharp
    List<KeyValuePair<string, string>> values = new List<KeyValuePair<string, string>>();
    values.Add(new KeyValuePair<string, string>("text", text));
    
    using (FormUrlEncodedContent content = new FormUrlEncodedContent(values))
    {
        content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded");
        response = await client.PostAsync(uri, content);
    }
    ```

## <a name="get-and-print-the-api-response"></a>获取和打印 API 响应

### <a name="get-the-client-id-header"></a>获取客户端 ID 标头

如果响应包含 `X-MSEdge-ClientID` 标头，获取该值并将其打印。

``` csharp
string client_id;
if (response.Headers.TryGetValues("X-MSEdge-ClientID", out IEnumerable<string> header_values))
{
    client_id = header_values.First();
    Console.WriteLine("Client ID: " + client_id);
}
```

### <a name="get-the-response"></a>获取响应

从 API 获取响应。 反序列化 JSON 对象，并将其打印到控制台。

```csharp
string contentString = await response.Content.ReadAsStringAsync();

dynamic jsonObj = JsonConvert.DeserializeObject(contentString);
Console.WriteLine(jsonObj);
```

## <a name="call-the-spell-check-function"></a>调用拼写检查函数

在项目的主函数中，调用 `SpellCheck()`。

```csharp
static void Main(string[] args)
{
    SpellCheck();
    Console.ReadLine();
}
```

## <a name="example-json-response"></a>示例 JSON 响应

在 JSON 中返回成功的响应，如以下示例所示： 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [创建单页 Web 应用](../tutorials/spellcheck.md)

- [什么是必应拼写检查 API？](../overview.md)
- [必应拼写检查 API v7 参考](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
