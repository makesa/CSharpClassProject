﻿
      ██╗     ██╗   ██╗███╗   ██╗ ██████╗██╗  ██╗
      ██║     ██║   ██║████╗  ██║██╔════╝██║  ██║
      ██║     ██║   ██║██╔██╗ ██║██║     ███████║
      ██║     ██║   ██║██║╚██╗██║██║     ██╔══██║
      ███████╗╚██████╔╝██║ ╚████║╚██████╗██║  ██║
      ╚══════╝ ╚═════╝ ╚═╝  ╚═══╝ ╚═════╝╚═╝  ╚═╝
   enabling co-workers to argue about work, not food

----------  DONE  ----------
	  
I.   Create a New ASP.NET Application from Scratch (like Grandma used to do it)
	 
     A. Open Visual Studio. Click File -> New -> Project...
     B. Under Web, select ASP.NET Web Application (.NET Framework).
        1. Name the project Lunch. 
        2. Set the location to the place where you prefer to store your code repositories.
        3. Check the box that says "Create directory for solution".
        4. Check the box that says "Create new Git repository". This will run git init in the folder, adding
	    a .git folder, and a .gitignore file that will ignore common files and folders specific to Visual Studio.
        5. Click OK.
     C. From the "New ASP.NET Web Application" dialog, select Empty. We will start this without anything so
	    that we understand everything.
        1. Do not check any boxes under "Add folders and core references for".
        2. Do not check "Add unit tests".
        3. Leave Authentication set to "No Authentication".
        4. Click OK.
	 D. The application can be launched in the browser, but a 403.14 (Forbidden) error is thrown.
	 E. The following concepts can be introduced:
	    1. Creating a new project in Visual Studio
	    2. Viewing the project structure in Solution Explorer
	    3. Interacting with Git and GitHub with Visual Studio in Team Explorer
	    4. The anatomy of a C# application (solutions, projects, references, configuration, etc.)

----------  TODO  ----------
		 
II.  Wire-up ASP.NET MVC (Model-View-Controller)
   	 
     A. From the Tools menu, click NuGet Package Manager -> Manage NuGet Packages for Solution...
	 B. Make sure "Browse" is selected and enter MVC into the Search.
	 C. Select Microsoft.AspNet.Mvc by Microsoft, select Lunch, and click Install. Accept the terms, and complete
	    the install.
     D. Add new folders to the Lunch project named Controllers, Models, and Views.
	 E. Add a new folder under the Views folder named Shared.
	 F. Add a layout view.
	    1. Right-click the Views/Shared folder and select Add -> MVC 5 Layout Page (Razor).
	    2. For Item name, use _Layout.
	    3. Click OK. Note that the newly created _Layout.cshtml page includes the html, head, and body tags.
	       The body tag includes the server code @RenderBody() where the content of any child views will render.
	    4. Replace the body with the following HTML...
	 
	       <body>
               <div>This is the header from the layout view.</div>
               <div>@RenderBody()</div>
               <footer>This is the footer from the layout view.</footer>
           </body>
	 	  
	 G. Add a Razor ViewStart file.
	    1. Right-click the Views folder and select Add -> View...
	    2. For View name, enter _ViewStart.
	    3. For Template, select Empty (without model).
	    4. Uncheck "Use a layout page".
	    5. Click Add.
	    6. Replace the entire contents of the _ViewStart.cshtml with the following code...
	   
	       @{ Layout = "~/Views/Shared/_Layout.cshtml"; }
	 
	 H. Add a Home controller.
	    1. Right-click the Controllers folder and select Add -> Controller...
	    2. From the Add Scaffold dialog, select "MVC 5 Controller - Empty".
	    3. Click Add.
	    4. For Controller name, use HomeController.
	    5. Click Add. Note that this not only added a HomeController.cs file, but it also added a Home folder in the
	       Views folder. Also, the HomeController class has a method called Index() that returns an ActionResult. The
	 	  return statement returns the result of the controller's View() method.
	 I. Add a Home/Index view.
	    1. Right-click the Views/Home folder and select Add -> View...
	    2. For View name, enter Index.
	    3. Check the "Use a layout page" box, but don't enter anything in the box.
	    4. Click Add. Note that the view does not contain html, head, or body tags, but does contain an h2 heading tag
	       that is semantically incorrect unless it appears inside a body tag. This will come from the layout view.
	 J. Configure MVC routing.
	    1. Add a folder to the Lunch project named App_Start.
	    2. Right-click the App_Start folder and select Add -> Class...
	    3. Name the class RouteConfig.cs.
        4. Replace the contents of RouteConfig.cs with the following code...
	 
	       using System.Web.Mvc;
           using System.Web.Routing;
           
           namespace Lunch
           {
               public class RouteConfig
               {
                   public static void RegisterRoutes(RouteCollection routes)
                   {
                       routes.MapRoute(
                           "Default",
                           "{controller}/{action}/{id}",
                           new { controller = "Home", action = "Index", id = UrlParameter.Optional }
                       );
                   }
               }
           }
	 
	    5. Add a Global.asax file and wire it up to call RegisterRoutes() when the application starts.
	 	   a. Right-click the Lunch project and select Add -> New Item...
	 	   b. Under Visual C# -> Web -> General, select Global Application Class.
	 	   c. Click Add.
	 	   d. Modify the Application_Start() method with the following code...
	 
	 	      protected void Application_Start(object sender, EventArgs e)
              {
                  RouteConfig.RegisterRoutes(System.Web.Routing.RouteTable.Routes);
              }
	 
	 K. Add a Web.config file in the Views folder to wire up the default settings for all views.
	    1. Right-click the Views folder and select Add -> New Item...
	    2. Under Visual C# -> Web -> General, select Web Configuration File.
	    3. Click Add.
	    4. Replace the contents of the Views/Web.config file with the following XML code...
	 
           <?xml version="1.0"?>
           
           <configuration>
             
             <configSections>
               <sectionGroup name="system.web.webPages.razor" type="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
                 <section name="host" type="System.Web.WebPages.Razor.Configuration.HostSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
                 <section name="pages" type="System.Web.WebPages.Razor.Configuration.RazorPagesSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" />
               </sectionGroup>
             </configSections>
           
             <system.web.webPages.razor>
               <host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=5.2.3.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
               <pages pageBaseType="System.Web.Mvc.WebViewPage">
                 <namespaces>
                   <add namespace="System.Web.Mvc" />
                   <add namespace="System.Web.Mvc.Ajax" />
                   <add namespace="System.Web.Mvc.Html" />
                   <add namespace="System.Web.Routing" />
                 </namespaces>
               </pages>
             </system.web.webPages.razor>
           
             <system.webServer>
               <handlers>
                 <remove name="BlockViewHandler"/>
                 <add name="BlockViewHandler" path="*" verb="*" preCondition="integratedMode" type="System.Web.HttpNotFoundHandler" />
               </handlers>
             </system.webServer>
             
           </configuration>
	 
	 L. The application can be launched in the browser and the Home/Index view will be loaded, including the content
	    from the layout view.
	 E. The following concepts can be introduced:
	    1. The Model-View-Controller pattern
	    2. The concept of convention-over-configuration
		3. MVC routing and the conventions that link controller actions and views