# BlueButton OAuth Node.js Sample Client Application
This application demonstrates how to connect to the BlueButton Sandbox using an OAuth client token and retrieve patient 
data from the server using a secured connection.

## Prerequisites

### Nodeclipse
This application includes a project file for running from within Eclipse using the [Nodeclipse plugin](http://www.nodeclipse.org/).  
Install the plugin from within Eclipse by selecting *Help -> Eclipse Marketplace...*.  
Type Nodeclipse in the *Find* field, select the plugin from the results and install it.  
Restart the Eclipse application to complete the plugin installation.  
Now simply import the sample application directory into a workspace by 
selecting *File -> Import... -> General -> Existing Projects into Workspace*.

### Install Library Dependencies
Library dependencies for this sample application are specified in the package.json file.  
These dependencies must be installed before attempting to run the sample application.  
From the *Project Explorer* in Eclipse open the *bluebutton-sample-client-nodejs* project.  
Locate the package.json file at the root of the project folder and right-click on it.  
Select *Run As... -> npm install* to install the library dependencies for this project.  
The console will show the installation progress and any warnings or errors it encounters.

### Register Application with Server
This application requires a [CMS Blue Button Developer Preview account](https://sandbox.bluebutton.cms.gov/v1/accounts/create).  
Once created, go to the [CMS Blue Button Developer Preview Application Registration Page](https://sandbox.bluebutton.cms.gov/v1/o/applications/register/) 
to register this application with the server.  
This step allows developers to create server-side OAuth credentials that can be used in their applications to provide 
clients the ability to gain secure access to their data(i.e. the Bluebutton Sandbox server).  
When registering the application leave all fields in their default state except for the *Name* and the *Redirect URI* fields.  
Set the *Name* field to something memorable as it is used on the server side to manage developer created application OAuth credentials.  
Set the *Redirect URI* field to **http://localhost:8001/redirect** if you are running this app from your local machine.  
This is the client side redirect URL that will be used by this sample application.  
Once the application is registered, **record the Client ID and the Client Secret for use in the next configuration step**.

If you are running this node app from a server replace the **http://localhost:8001/redirect** entry with the address of 
the server hosting the application. Include the port number if not using the standard port 80 for http:// or port 443
for https://. The redirect callback for this application is **/redirect**.

If you are running this app via a proxy or a tunnel. For example, using Apache or Nginx to act as a proxyserver then you
need to launch the node app using a tunnel parameter:

    node app.js -t=<external_url>

If the app is being served from a non-standard port add a port parameter: 

    node app.js -t=<external_url> -p=<port_number>
 
The tunnel (-t) and port (-p) values should match the redirect uri values provided when registering your application 
with the Blue Button API. For example if you are serving this app from https://your.domain.com the redirect uri would 
be: 

    https://your.domain.com/redirect
    
and you would launch the node app from the installed directory using the command:

    node app.js -t=https://your.domain.com 


### Configure serverAuth.js
Before running the application a **serverAuth.js** file must be created within the sample application's root directory.  
Create the file within Eclipse by navigating to the *Project Explorer* and right-clicking on *bluebutton-sample-client-nodejs* project.  
Select *New -> JavaScript File*, name the file **serverAuth.js** then copy and paste the following code into this new file:

	// BlueButton Registered Application Credentials
	const credentials = {
	    client: {
	        id: 'Enter Client ID from Application Registration Step Here',
	        secret: 'Enter Client Secret from Application Registration Step Here'
	    },
	    auth: {
	        tokenHost: 'https://sandbox.bluebutton.cms.gov',
	        authorizePath: '/v1/o/authorize/',
	        tokenPath: '/v1/o/token/'
	    }
	};

	exports.credentials = credentials;
	
**ATTENTION:** Be sure to replace the credentials.client.id and credentials.client.secret fields above with the values recorded 
from the previous registration step and save the file in Eclipse.

## Start Application
Once all the prerequisites are satisfied the application can be run from Eclipse by simply right-clicking on the app.js file 
in the workspace and select *Run As -> Node.js Application*.  
This will launch a local server on port 8001.  
Using your favorite browser, navigate to [http://localhost:8001](http://localhost:8001) to test the application out.