
# Installation guide for ASP.NET developers


1. [Download and unzip the package](/download) or [install the NuGet package](/docs/nuget). If you're not using ASP.NET, [read this guide](/docs/howto/use-without-asp-net).
2. In Visual Studio, right click your project and choose "Add reference". Browse to the extracted folder, and choose `dlls\release\ImageResizer.dll`. Or just copy it into the 'bin' folder. Whichever you prefer. (Compatible with .NET 2-4).
3. Modify or create the /Web.Config file for your site:
	
		<?xml version="1.0" encoding="utf-8" ?>
		<configuration>
			<configSections>
				<section name="resizer" type="ImageResizer.ResizerSection,ImageResizer" />
			</configSections>

			<resizer>
				<!-- Unless you (a) use Integrated mode, or (b) map all reqeusts to ASP.NET, 
				     you'll need to add .ashx to your image URLs: image.jpg.ashx?width=200&height=20 -->
				<pipeline fakeExtensions=".ashx" />

				<plugins>
					<!-- <add name="DiskCache" /> -->
					<!-- <add name="PrettyGifs" /> -->
				</plugins>	
			</resizer>

			<system.web>
				<httpModules>
					<!-- This is for IIS5, IIS6, and IIS7 Classic, and Cassini/VS Web Server-->
					<add name="ImageResizingModule" type="ImageResizer.InterceptModule"/>
				</httpModules>
			</system.web>

			<system.webServer>
				<validation validateIntegratedModeConfiguration="false"/>
				<modules>
					<!-- This is for IIS7+ Integrated mode -->
					<add name="ImageResizingModule" type="ImageResizer.InterceptModule"/>
				</modules>
			</system.webServer>
		</configuration>
	
4. Start your web site, then visit [/resizer.debug.ashx](/plugins/diagnostics) to verify you've done everything correctly. If you ever encounter issues, simply revisit that page to access the self-diagnostics. If you need help, [just ask](/support)! ASP.NET MVC is fully supported, [although you may have to add 2 IgnoreRoute statements](/docs/mvc), depending upon your route table.