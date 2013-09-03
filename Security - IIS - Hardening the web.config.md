Security hardening web.config
=============================

#### Disable tracing

```xml
<system.web>
  <trace enabled="false" />
</system.web>
```

#### Prevent applications running if `debug=true`

```xml
<system.web>
  <deployment retail=”true”/>
</system.web>
```

#### Disable debug

```xml
<system.web>
  <compilation debug="false" />
</system.web>
```

#### Rename the cookie to something more obscure ASP.NET_SessionId

```xml
<system.web>
  <sessionState cookieName="s" />
</system.web>
```

#### Disable ASP.NET version HTTP header

```xml
<system.web>
  <httpRuntime enableVersionHeader="false" />
</system.web>
```

#### Secure cookies on HTTPS deployments

http://msdn.microsoft.com/en-us/library/vstudio/ms228262(v=vs.100).aspx

```xml
<system.web>
  <httpCookies httpOnlyCookies="false" requireSSL="false" />
</system.web>
```

#### Remove unwanted http headers

```xml
<system.webServer>
  <httpProtocol>
      <customHeaders>
        <remove name="Server" />
        <remove name="X-Powered-By" />
        <remove name="X-AspNet-Version" />
        <remove name="X-AspNetMvc-Version" />
      </customHeaders>
    </httpProtocol>
</system.webServer>
```

#### Configure custome error handlers for specific HTTP error codes

```xml
<system.webServer>
  <httpErrors errorMode="Custom">
    <remove statusCode="502" subStatusCode="-1" />
    <remove statusCode="501" subStatusCode="-1" />
    <remove statusCode="500" subStatusCode="-1" />
    <remove statusCode="412" subStatusCode="-1" />
    <remove statusCode="406" subStatusCode="-1" />
    <remove statusCode="405" subStatusCode="-1" />
    <remove statusCode="404" subStatusCode="-1" />
    <remove statusCode="403" subStatusCode="-1" />
    <remove statusCode="401" subStatusCode="-1" />
    <remove statusCode="400" subStatusCode="-1" />
	
    <error statusCode="400" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="401" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="402" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="403" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="404" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="405" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="406" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="412" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="500" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="501" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="502" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
  </httpErrors>
</system.webServer>
```

#### Remove unwanted modules to reduce your attack surface

```xml
<system.webServer>
  <handlers>
    <remove name="TraceHandler-Integrated-4.0" />
    <remove name="TraceHandler-Integrated" />
    <remove name="AssemblyResourceLoader-Integrated-4.0" />
    <remove name="AssemblyResourceLoader-Integrated" />
    <remove name="WebAdminHandler-Integrated-4.0" />
    <remove name="WebAdminHandler-Integrated" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-2.0-64" />
    <remove name="svc-ISAPI-4.0_32bit" />
    <remove name="ScriptHandlerFactoryAppServices-Integrated-4.0" />
    <remove name="ScriptResourceIntegrated-4.0" />
    <remove name="svc-ISAPI-4.0_64bit" />
    <remove name="svc-Integrated-4.0" />
    <remove name="vbhtm-ISAPI-4.0_32bit" />
    <remove name="vbhtm-ISAPI-4.0_64bit" />
    <remove name="vbhtm-Integrated-4.0" />
    <remove name="vbhtml-ISAPI-4.0_32bit" />
    <remove name="vbhtml-ISAPI-4.0_64bit" />
    <remove name="vbhtml-Integrated-4.0" />
    <remove name="xamlx-ISAPI-4.0_32bit" />
    <remove name="xamlx-ISAPI-4.0_64bit" />
    <remove name="xamlx-Integrated-4.0" />
    <remove name="xoml-ISAPI-4.0_32bit" />
    <remove name="xoml-ISAPI-4.0_64bit" />
    <remove name="xoml-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-rem-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-2.0" />
    <remove name="rules-ISAPI-4.0_32bit" />
    <remove name="rules-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-soap-Integrated" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-2.0" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-4.0_64bit" />
    <remove name="HttpRemotingHandlerFactory-soap-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-4.0_32bit" />
    <remove name="rules-ISAPI-4.0_64bit" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-2.0-64" />
    <remove name="HttpRemotingHandlerFactory-rem-Integrated" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-4.0_32bit" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-4.0_64bit" />
    <remove name="AXD-ISAPI-2.0-64" />
    <remove name="cshtml-ISAPI-4.0_64bit" />
    <remove name="cshtml-Integrated-4.0" />
    <remove name="cshtm-Integrated-4.0" />
    <remove name="cshtml-ISAPI-4.0_32bit" />
    <remove name="cshtm-ISAPI-4.0_64bit" />
    <remove name="cshtm-ISAPI-4.0_32bit" />
    <remove name="AXD-ISAPI-4.0_64bit" />
    <remove name="AXD-ISAPI-2.0" />
    <remove name="AXD-ISAPI-4.0_32bit" />
    <remove name="PageHandlerFactory-ISAPI-2.0-64" />
    <remove name="PageHandlerFactory-ISAPI-2.0" />
    <remove name="PageHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="PageHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="aspq-ISAPI-4.0_64bit" />
    <remove name="aspq-Integrated-4.0" />
    <remove name="WebServiceHandlerFactory-ISAPI-2.0" />
    <remove name="aspq-ISAPI-4.0_32bit" />
    <remove name="WebServiceHandlerFactory-Integrated-4.0" />
    <remove name="WebServiceHandlerFactory-Integrated" />
    <remove name="SimpleHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="SimpleHandlerFactory-Integrated-4.0" />
    <remove name="SimpleHandlerFactory-Integrated" />
    <remove name="SimpleHandlerFactory-ISAPI-2.0" />
    <remove name="SimpleHandlerFactory-ISAPI-2.0-64" />
    <remove name="WebServiceHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="WebServiceHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="WebServiceHandlerFactory-ISAPI-2.0-64" />
    <remove name="SimpleHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="ISAPI-dll" />
    <remove name="ExtensionlessUrlHandler-ISAPI-4.0_64bit" />
    <remove name="ExtensionlessUrlHandler-ISAPI-4.0_32bit" />
    <remove name="OPTIONSVerbHandler" />
    <remove name="TRACEVerbHandler" />
  </handlers>
</system.webServer>
```

#### Restrict which HTTP verbs your application will work with

```xml
<system.webServer>
  <security>
    <requestFiltering allowHighBitCharacters="false">
      <verbs allowUnlisted="false">
        <add verb="GET" allowed="true" />
        <add verb="POST" allowed="true" />
      </verbs>
    </requestFiltering>
  </security>
</system.webServer>
```

## Hardened Web.Config - Complete template

```xml
<system.web>

  <!-- -->
  <trace enabled="false" />
  
  <!-- -->
  <deployment retail=”true”/>
  
  <!-- -->
  <compilation debug="false" />
  
  <!-- -->
  <sessionState cookieName="s" />
  
  <!-- -->
  <httpRuntime enableVersionHeader="false" />
  
  <!-- -->
  <httpCookies httpOnlyCookies="false" requireSSL="false" />

</system.web>

<system.webServer>

  <!-- -->
  <httpProtocol>
    <customHeaders>
      <remove name="Server" />
      <remove name="X-Powered-By" />
      <remove name="X-AspNet-Version" />
    </customHeaders>
  </httpProtocol>

  <!-- -->
  <httpErrors errorMode="Custom">
    <remove statusCode="502" subStatusCode="-1" />
    <remove statusCode="501" subStatusCode="-1" />
    <remove statusCode="500" subStatusCode="-1" />
    <remove statusCode="412" subStatusCode="-1" />
    <remove statusCode="406" subStatusCode="-1" />
    <remove statusCode="405" subStatusCode="-1" />
    <remove statusCode="404" subStatusCode="-1" />
    <remove statusCode="403" subStatusCode="-1" />
    <remove statusCode="401" subStatusCode="-1" />
    <remove statusCode="400" subStatusCode="-1" />
	
    <error statusCode="400" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="401" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="402" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="403" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="404" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="405" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="406" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="412" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="500" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="501" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    <error statusCode="502" subStatusCode="-1" path="/error-handler.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
  </httpErrors>

  <!-- -->
  <handlers>
    <remove name="TraceHandler-Integrated-4.0" />
    <remove name="TraceHandler-Integrated" />
    <remove name="AssemblyResourceLoader-Integrated-4.0" />
    <remove name="AssemblyResourceLoader-Integrated" />
    <remove name="WebAdminHandler-Integrated-4.0" />
    <remove name="WebAdminHandler-Integrated" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-2.0-64" />
    <remove name="svc-ISAPI-4.0_32bit" />
    <remove name="ScriptHandlerFactoryAppServices-Integrated-4.0" />
    <remove name="ScriptResourceIntegrated-4.0" />
    <remove name="svc-ISAPI-4.0_64bit" />
    <remove name="svc-Integrated-4.0" />
    <remove name="vbhtm-ISAPI-4.0_32bit" />
    <remove name="vbhtm-ISAPI-4.0_64bit" />
    <remove name="vbhtm-Integrated-4.0" />
    <remove name="vbhtml-ISAPI-4.0_32bit" />
    <remove name="vbhtml-ISAPI-4.0_64bit" />
    <remove name="vbhtml-Integrated-4.0" />
    <remove name="xamlx-ISAPI-4.0_32bit" />
    <remove name="xamlx-ISAPI-4.0_64bit" />
    <remove name="xamlx-Integrated-4.0" />
    <remove name="xoml-ISAPI-4.0_32bit" />
    <remove name="xoml-ISAPI-4.0_64bit" />
    <remove name="xoml-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-rem-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-2.0" />
    <remove name="rules-ISAPI-4.0_32bit" />
    <remove name="rules-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-soap-Integrated" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-2.0" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-4.0_64bit" />
    <remove name="HttpRemotingHandlerFactory-soap-Integrated-4.0" />
    <remove name="HttpRemotingHandlerFactory-soap-ISAPI-4.0_32bit" />
    <remove name="rules-ISAPI-4.0_64bit" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-2.0-64" />
    <remove name="HttpRemotingHandlerFactory-rem-Integrated" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-4.0_32bit" />
    <remove name="HttpRemotingHandlerFactory-rem-ISAPI-4.0_64bit" />
    <remove name="AXD-ISAPI-2.0-64" />
    <remove name="cshtml-ISAPI-4.0_64bit" />
    <remove name="cshtml-Integrated-4.0" />
    <remove name="cshtm-Integrated-4.0" />
    <remove name="cshtml-ISAPI-4.0_32bit" />
    <remove name="cshtm-ISAPI-4.0_64bit" />
    <remove name="cshtm-ISAPI-4.0_32bit" />
    <remove name="AXD-ISAPI-4.0_64bit" />
    <remove name="AXD-ISAPI-2.0" />
    <remove name="AXD-ISAPI-4.0_32bit" />
    <remove name="PageHandlerFactory-ISAPI-2.0-64" />
    <remove name="PageHandlerFactory-ISAPI-2.0" />
    <remove name="PageHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="PageHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="aspq-ISAPI-4.0_64bit" />
    <remove name="aspq-Integrated-4.0" />
    <remove name="WebServiceHandlerFactory-ISAPI-2.0" />
    <remove name="aspq-ISAPI-4.0_32bit" />
    <remove name="WebServiceHandlerFactory-Integrated-4.0" />
    <remove name="WebServiceHandlerFactory-Integrated" />
    <remove name="SimpleHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="SimpleHandlerFactory-Integrated-4.0" />
    <remove name="SimpleHandlerFactory-Integrated" />
    <remove name="SimpleHandlerFactory-ISAPI-2.0" />
    <remove name="SimpleHandlerFactory-ISAPI-2.0-64" />
    <remove name="WebServiceHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="WebServiceHandlerFactory-ISAPI-4.0_64bit" />
    <remove name="WebServiceHandlerFactory-ISAPI-2.0-64" />
    <remove name="SimpleHandlerFactory-ISAPI-4.0_32bit" />
    <remove name="ISAPI-dll" />
    <remove name="ExtensionlessUrlHandler-ISAPI-4.0_64bit" />
    <remove name="ExtensionlessUrlHandler-ISAPI-4.0_32bit" />
    <remove name="OPTIONSVerbHandler" />
    <remove name="TRACEVerbHandler" />
  </handlers>

  <!-- -->
  <security>
    <requestFiltering allowHighBitCharacters="false">
      <verbs allowUnlisted="false">
        <add verb="GET" allowed="true" />
        <add verb="POST" allowed="true" />
      </verbs>
    </requestFiltering>
  </security>

</system.webServer>
```
