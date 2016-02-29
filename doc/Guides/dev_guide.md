Bosch IoT XDK Demo App Developer Guide
======================================

This guide is a collection of useful HowTos, tips and tricks to ease up your hackathon life in no particular order.

<a name="nav_runlocally"></a>

Run the Demo App Locally
------------------------
When developing your cloud app it is usually necessary to see the changes you have made, without having to push your app to the cloud.  
The Bosch IoT XDK Demo App has two spring run time profiles. `application-prod.yml` is used, when running the app in the cloud. `application-dev.yml` is used, when running the app in all other scenarios. In this chapter we will show you how you can run the Bosch IoT XDK Demo App using the dev profile. 

> Note: When the app runs locally, it still uses the same cloud service instances of the Bosch IoT Suite services as the cloud instance of the app.   

You will need the following software additionally to the software required in the [Getting Started Guide](./Getting%20Started.html#nav_prereq):

- latest [MongoDB](https://www.mongodb.org/downloads) Community Edition (starting with 3.0)


To run your application do the following:

1. First complete the [Getting Started Guide](./Getting%20Started.html). 
	
	> Note: The cloud services have to be bound to the cloud instance of your application, in order for you to be able to see all needed environment variables for step 3 of this chapter. This means that you have to push your app to the cloud prior to running it locally.

2. Install and run MongoDB with standard settings as described in the [MongoDB documentation](https://docs.mongodb.org/manual/installation/) for your OS.
3. In "src/main/resources/config/application-dev.yml" make all necessary changes in the following part (replace everything in <> as described below):

		cr:
		  alias: CR
		  aliasPassword: <addMe>
		  boschIotCentralRegistryEndpointUrl: wss://events.apps.bosch-iot-cloud.com:443/
		  #proxyHost: <addMe>
		  #proxyPort: <addMe>
		  keyStorePath: /keystore/CRClient.jks
		  keyStorePassword: <addMe>
		  sslKeyStorePath: /keystore/bosch-iot-cloud.jks
		  sslKeyStorePassword: jks
		  apiKey: <addMe>
		  restUrl: https://cr.apps.bosch-iot-cloud.com/
		  crClientId: <addMe>
		  applicationNamespace: com.bosch.bcx.demo
		
		im3:
		  server: http://im3-api.apps.bosch-iot-cloud.com
		  clientId: <addMe>
		  clientSecret: <addMe>
		  tenantId: <addMe>
		  apiKey: <addMe>

     - You have set the `aliasPassword` and the `keyStorePassword` when generating the keystore and alias in the [Getting Started Guide](./Getting%20Started.html#nav_appsetup)
     - To get the `apiKey` and the `crClientId` of your `cr` configuration go to the [Developer Console](https://apps.sys.bosch-iot-cloud.com/) of the Bosch IoT Cloud, login, open your *space* and click on the cloud instance of your application.
        - open the **Services** tab to see all bound services
        - click on **Show credentials** of your Bosch IoT Central Registry service
        - the *api\_token* is your `apiKey` and the *solution\_id* is your `crClientId`
     - To get the details of the `im3` configuration click on **Show credentials** of your Bosch IoT Identity Management service
	     - *imApiKeyId* is the `apiKey`
	     - *imClientId* is the `clientId`
	     - *imClientSecret* is the `clientSecret`
	     - *imTenantId* is the `tenantId`
4. Run the App locally by starting the main() in `\CloudApp\src\main\java\com\bosch\demo\xdkcloudapp\Application.java`
5. The Bosch IoT XDK Demo App is now reachable under [http://localhost:8080](http://localhost:8080).

<a name="nav_noAdminInstall"></a>

Setup your Cloud Development Environment without Admin Rights (Windows)
-----------------------------------------------------------------------
> Note: If you don't have admin rights on your Windows system, you can still complete most of the Getting Started guide assuming you have an installed Oracle JDK 1.8. Without admin rights you won't be able to install the XDK Workbench thus you won't be able to reprogram or flash the XDK.

1. In order to have a working connection between the XDK and the Bosch IoT XDK Demo App, ask someone with admin rights to flash your XDK with the LWM2M firmware.

	> Note: Make sure you already have Java JDK 1.8 installed on your machine. If not, look for a portable Java 8 distribution and/or instructions to install Oracle Java JDK 1.8 without admin rights.

2. Install Java, your preferred IDE, Maven and git as described in the steps 1-4 in the [Setup your Cloud Development Environment Chapter](Getting%20Started.html#nav_devsetup) of the Getting Started guide. (These can be done without admin rights).

3. In a suitable place create the following folder structure *...\nodejs\node_modules\\*.

4. Download the node js Windows binary (node.exe) from [Node.js Download Site](https://nodejs.org/en/download/stable/) and copy it into *...\nodejs\node.exe*.

5. Download the Cloud Foundry Command Line Interface binary (starting with `cf-cli_6.15.0_winx64.zip`) from [Cloud Foundry GitHub Site](https://github.com/cloudfoundry/cli#downloads) and unpack the `cf.exe` to *...\nodejs\cf.exe*.

6. Download the latest release of npm (starting with `npm-3.7.1.zip`) from [NPM GitHub Site](https://github.com/npm/npm/releases).

7. Unzip the `npm-x.x.x` folder to *...\nodejs\node_modules\\* and rename it to get the following path `...\nodejs\node_modules\npm\bin\`.

8. Copy `...\nodejs\node_modules\npm\bin\npm.cmd` to *...\nodejs\node.cmd*. The resulting structure should look as in the screenshot below.  
![Folder strucutre for no-admin-install][img_node_folderstruct]

9. Add the following paths to your PATH environment variable:  
  - your `...\nodejs\` folder
  - path to your git.exe, e.g. `C:\Users\<windows_user>\AppData\Local\Programs\Git\cmd`

	> Note: The resulting PATH variable (system variable and user variable combined) cannot be longer than 1024 characters.

10. You can check your work by executing the following commands in a fresh terminal: (you should receive the version for each software) 
	- `npm -version`
	- `node --version`
	- `git --version`
	- `cf --version`

11. Resume with the next chapter in the [Getting Started guide](Getting%20Started.html#nav_cloudsetup).


[img_node_folderstruct]: ./resources/img/img_node_folderstruct.png