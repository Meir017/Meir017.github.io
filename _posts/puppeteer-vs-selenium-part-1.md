---
title: "Puppeteer Sharp vs Selenium"
published: true
---

Notice that the selenium api is sync because under the covers for each http request they send they call `.GetAwaiter().GetResult()`

selenium source code:

creating the request and getting the response:
https://github.com/SeleniumHQ/selenium/blob/master/dotnet/src/webdriver/Remote/HttpCommandExecutor.cs#L206-L244
```cs
private HttpResponseInfo MakeHttpRequest(HttpRequestInfo requestInfo)
{
    HttpWebRequest request = HttpWebRequest.Create(requestInfo.FullUri) as HttpWebRequest;
    
    ...

    try
    {
        webResponse = request.GetResponse() as HttpWebResponse;
    ...
```

corefx source code:
https://github.com/dotnet/corefx/blob/master/src/System.Net.Requests/src/System/Net/HttpWebRequest.cs#L1005

```cs
return SendRequest().GetAwaiter().GetResult();
```

So in this blog post I'll assume that the methods are async 

* For Beginners

- Clicking:

Selenium:

```cs
IWebDriver driver = ...;
IWebElement element = driver.FindElement("some selector");
element.Click();
```

Puppeteer:

```cs
Page page = ...;

// a.
await page.ClickAsync("some selector");

// b.
ElementHandle element = await page.QuerySelectorAsync("some selector");
await element.ClickAsync();
```

- Typing:

- Select: