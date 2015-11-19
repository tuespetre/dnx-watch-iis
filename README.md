# dnx-watch-iis
How to configure the save-and-refresh development experience for ASP.NET 5 with a local IIS server on Windows 10.

## Setting up IIS for all projects

Make sure you have DNVM and DNX installed. Give `IIS_IUSRS` read/list/execute permissions on your user profile's `.dnx` folder.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/set-dnx-folder-permissions.png)

-----

Open IIS Manager, open the root server node and then open the configuration manager.

![IIS Manager](https://github.com/tuespetre/dnx-watch-iis/blob/master/6-go-to-configuration.png)

-----

In the configuration manager, go to the `system.applicationHost/applicationPools` sections and click the button to open the collection editor for `environmentVariables` under `applicationPoolDefaults`.

<blockquote><strong>Note:</strong> If you are not running Windows 10 and thus do not have IIS 10, you cannot set the environment variables for the application pool. You may instead set these up as system environment variables.</blockquote>

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

Create a site or a virtual application for your project, pointing it to the `wwwroot` folder and assigning it to the application pool you created.

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/10-create-site.png)

-----

Visit the site you created. 

![](https://github.com/tuespetre/dnx-watch-iis/blob/master/11-visit-site.png)

-----

You can now edit your code, save, and refresh your browser.
