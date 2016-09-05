# NaverTranslator
NAVER TRANSLATE API INTERFACE

```cs
[Service("v1/language")]
public interface TranslateAPIInterface
{
    string clientId { get; set; }
    string clientSecret { get; set; }

    [Post]
    [Resource("translate")]
    [JsonPath("message.result.translatedText")]
    Task<string> Translate([RequestUri]string source, [RequestUri]string target, [RequestUri]string text);
}

[ProcessorTarget(new Type[] { typeof(TranslateAPIInterface) })]
public class ClientAuthProcessor : IRequestProcessor
{
    public void OnRequest(object api, HttpRequest request)
    {
        request.headers["Content-Type"] = "application/x-www-form-urlencoded";
        request.headers["X-Naver-Client-Id"] = ((TranslateAPIInterface)api).clientId;
        request.headers["X-Naver-Client-Secret"] = ((TranslateAPIInterface)api).clientSecret;
    }
}

public class TranslateAPI
{
    public static TranslateAPIInterface Create(string clientId, string clientSecret)
    {
        var api = RemotePoint.Create<TranslateAPIInterface>("https://openapi.naver.com");

        api.clientId = clientId;
        api.clientSecret = clientSecret;

        return api;
    }
}
```
```cs
public class TargetLanguages
{
    public static readonly string Korean = "ko";
    public static readonly string English = "en";
    public static readonly string Chinese = "zh-CN";
    public static readonly string Japanese = "ja";
}
```

Usage
----
```cs
var tr = TranslateAPI.Create("CLIENT_ID", "CLIENT_SECRET");

var translated = await tr.Translate("ko", "ja", "안녕");
```
