# Nuget加入ASP.NET MVC

![Dashboard](https://github.com/YiHsuanLi0113/ClinicManagement/blob/master/Content/images/nuget-1.png)

# 加入Controllers & Views資料夾，並加入Controller及對應的檢視

![Dashboard](https://github.com/YiHsuanLi0113/ClinicManagement/blob/master/Content/images/addView-1.png)

# 加入Global.asax，設定路由

![Dashboard](https://github.com/YiHsuanLi0113/ClinicManagement/blob/master/Content/images/globalasax.png)

- 於`Global.asax`加入以下

  ```C#
  using System.Web.Routing;
  ```
- 於專案中新增`App_Start`資料夾

- 再於`App_Start`中新增類別`RouteConfig.cs`

    ```C#
    using System.Web.Routing;
    using System.Web.Mvc;
   ```
   ```C#
    public static void RegisterRoutes(RouteCollection routes)
    {
      routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

      routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional}
      );
    }
   ```
- 再回到`Global.asax`於`Application_Start()`加入以下

  ```C#
  protected void Application_Start(object sender, EventArgs e)
  {
    RouterConfig.RegisterRoutes(RouteTable.Routes);
  }
  ```
  
- 再來大膽按下F5，以為可以成功呈現`Home/Index`了嗎？ 不XD

# 遇到的問題記錄

1. CS0103: The name 'ViewBag' does not exist in the current context

   解決方法參考：https://stackoverflow.com/questions/4136703/razor-htmlhelper-extensions-or-other-namespaces-for-views-not-found/4136773#4136773
   
   - 我的解決步驟
   
   1. 於`Views`資料夾加入`Web.config`
   
   2. `Web.config`加入內容如下
   
   ```C#
   <configSections>
    <sectionGroup name="system.web.webPages.razor" type="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
      <section name="host" type="System.Web.WebPages.Razor.Configuration.HostSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
      <section name="pages" type="System.Web.WebPages.Razor.Configuration.RazorPagesSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
    </sectionGroup>
   </configSections>
   
   <system.web.webPages.razor>
    <host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
    <pages pageBaseType="System.Web.Mvc.WebViewPage">
      <namespaces>
        <add namespace="System.Web.Mvc" />
        <add namespace="System.Web.Mvc.Ajax" />
        <add namespace="System.Web.Mvc.Html" />
        <add namespace="System.Web.Routing" />
        <!-- Your namespace here -->
      </namespaces>
    </pages>
   </system.web.webPages.razor>
   ```
   

2. 每個組態檔只允許一個 `<configSections>` 元素，而且 (如果有) 它必須是根 `<configuration>` 元素的第一個子系。
   
   解決方法參考：https://maidot.blogspot.com/2013/11/visual-c.html
   
   - 我的解決步驟
   
   1. 注意`<configSections></configSection>`務必出現在`<configuration></configuration>`中，且必為第一個區塊
   
   
3. 建立 `system.web.webPages.razor/host` 的組態區段處理常式時發生錯誤: 無法載入檔案或組件 `'System.Web.WebPages.Razor, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'` 或其相依性的其中之一。 找到的組件資訊清單定義與組件參考不符。 (發生例外狀況於 HRESULT: 0x80131040)

   解決方法參考：https://dotblogs.com.tw/skychang/2013/01/04/86729
   
   - 我的解決步驟
   
   1. 查看`System.Web.Razor`屬性
   
   ![Dashboard](https://github.com/YiHsuanLi0113/ClinicManagement/blob/master/Content/images/razorVersion.png)
   
   2. 修改`Views/Web.config`中的Razor version
   
   ```C#
   <sectionGroup name="system.web.webPages.razor" type="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
      <section name="host" type="System.Web.WebPages.Razor.Configuration.HostSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
      <section name="pages" type="System.Web.WebPages.Razor.Configuration.RazorPagesSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
    </sectionGroup>
   ```
   
# 成功導到`Home/Index`

![Dashboard](https://github.com/YiHsuanLi0113/ClinicManagement/blob/master/Content/images/homeIndex.png)
