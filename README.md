# dnx-watch-iis
How to configure the save-and-refresh development experience for ASP.NET Core with a local IIS server on Windows 10.

> **This guide is for RC1 only**
>
> Please note that this guide is written for RC1 only. As RC2 gets closer, or when it is released, I plan to write an updated version detailing the steps to obtain the save-and-refresh development experience using RC2.

## Setting up IIS for all projects

Install the latest version of HttpPlatformHandler for your computer's architecture. These should be linked on the official ASP.NET docs for publishing to IIS: <https://docs.asp.net/en/latest/publishing/iis.html>

Use `Control Panel > Programs > Programs and Features > Turn Windows features on or off` to install the `IIS Management Console`, `World Wide Web Services` (default features are ok, and you can add any additional features desired), and the `Windows Process Activation Service`. The Windows Process Activation Service will install with a manual start required, so you will want to open the service manager, configure it to start automatically, and start it.

Make sure you have DNVM and DNX installed. Give `IIS_IUSRS` read/list/execute permissions on your user profile's `.dnx` folder.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/set-dnx-folder-permissions.png)

-----

Open IIS Manager, open the root server node and then open the configuration manager.

![IIS Manager](https://github.com/tuespetre/dnx-watch-iis/blob/master/6-go-to-configuration.png)

-----

In the configuration manager, go to the `system.webServer/handlers` section and click the link-button labelled 'Unlock Section'.

<img src="https://github.com/tuespetre/dnx-watch-iis/blob/master/unlock-webserver-handlers.png" />

Then, go to the `system.applicationHost/applicationPools` section and click the button to open the collection editor for `environmentVariables` under `applicationPoolDefaults`.

<blockquote><strong>Note:</strong> If you are not running Windows 10 and thus do not have IIS 10, you cannot set the environment variables for the application pool. You may instead set these up as system environment variables. Setting the <code>%DNX_PATH%</code> variable as an environment variable under the <code>httpPlatform</code> configuration section does not work.</blockquote>

<img src="https://github.com/tuespetre/dnx-watch-iis/blob/master/7-env-variables-option.png" />

-----

Modify the collection to resemble the values shown in the figure below. Alter the values as needed to reflect your own username. When done, close the collection editor and click 'apply changes'.

![Environment Variables Values](https://github.com/tuespetre/dnx-watch-iis/blob/master/8-env-variables-values.png)

-----

## Setting up your project for IIS

Give `IIS_USRS` read/list/execute permissions on your solution's containing folder.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/2-add-permissions.png)

-----

In your project's `wwwroot` folder, create a `log` folder, and give `IIS_IUSRS` write permissions on it.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/4-add-permissions.png)

-----

Modify the project's `web.config` to make use of the new `log` folder.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/5-modify-web-config.png)

-----

Modify the project's `project.json` to include a `watch` command and refer to `Microsoft.Dnx.Watcher` as a dependency. If you are using Visual Studio, the package will be loaded automatically; otherwise, run `dnu restore` to restore the package.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/5-modify-project-json.png)

-----

Create an application pool for your project.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/9-create-app-pool.png)

-----

Create a site or a virtual application for your project, pointing it to the `wwwroot` folder and assigning it to the application pool you created. In the example below, a virtual application is created under a site called `localhost` which is bound to ports 80 and 443. You may configure your site or virtual application to suit your own needs.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/10-create-site.png)

-----

Visit the site you created. 

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/11-visit-site.png)

-----

You can now edit your code, save, and refresh your browser.
