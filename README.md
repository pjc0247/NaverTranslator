# NaverTranslator
NAVER TRANSLATE API INTERFACE w/ [CsRestClient](https://github.com/pjc0247/CsRestClient)

```cs
[Service("v1/language")]
public interface TranslateAPIInterface : WithCommonHeader
{
    [Post]
    [Resource("translate")]
    [JsonPath("message.result.translatedText")]
    Task<string> Translate([RequestUri]string source, [RequestUri]string target, [RequestUri]string text);
}

public class TranslateAPI
{
    public static TranslateAPIInterface Create(string clientId, string clientSecret)
    {
        var api = RemotePoint.Create<TranslateAPIInterface>("https://openapi.naver.com");

        api.commonHeaders = new Dictionary<string, string>()
        {
            ["X-Naver-Client-Id"] = clientId,
            ["X-Naver-Client-Secret"] = clientSecret
        };

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

var translated = await tr.Translate(
    TargetLanguages.Korean, TargetLanguages.English,
    "안녕");
```
