# Deployment Steps

### Step 0 - Gather Pre-requisites:
Be sure to follow the pre-requisites guidance in the this document: [PREREQUISITES.md](https://github.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/blob/main/Documentation/PREREQUISITES.md) 

### Step 1 - Deploy "Back-end" Azure Resources:
This step entails the deployment of the Azure SQL database, an Azure  Data Factory for ingesting inputs, a Machine Learning batch processingenvironment, and reporting functionality.  Please follow these steps for 

Below is a sample view of the initial Azure deployment screen and the parameter values required:

![Azure Portal Deploy Step One](https://raw.githubusercontent.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/main/images/Deploy_Step_1.png "[Azure Portal Deploy Step One")


When you are reday to DEPLOY - Click the Image below:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMSUSSolutionAccelerators%2FSmart-Spaces-Sustainability-Solution-Accelerator%2Fmain%2Ftemplates%2Fmaster_accelerator_deployment.json)

### Step 2 - Deploy "Front-end" Azure IOTHub Simulator Resources:
This step entails the deployment of the IotHub "Simulator" resources; namely an IotHub with (4) devices, along with supporting Azure Functions, Logic Apps and an Azure Stream Anlytics job - which all work together to produce simulated temperature and HVAC cooling information readings for a representative "Smart Space".  

##### Note: #####
This deployment step relies on (2) artifacts from previous deployment in Step #1 above:
- The Key Vault Name that was previously provisioned.
- The Azure SQL Database info and credentials that were previously provisioned. 
   
#### Please make sure to review and confirm the Azure Key Vault Name, SQL database name, and associated SQL credentials are correctly specified in the #2 deployment script.

You can confirm these settings by retrieving the following Key Valut Secret: sqlConnectString
Once you have displayed and copied this value, 
you can paste the contents into your favorite editor (or notepad) and then look for the following properties in the SQL Connection string:
- Server=
- Database=
- Uid=
- Pwd= 

##### Note: #####
You will need these values for the steps below.

##### Key Vault:
- Key Vault Name:

##### Azure SQL:
- Server=
- Database=
- Uid=
- Pwd= 


Below is a sample view of the Azure deployment screen and the parameter values required:

![Azure Portal Deploy Step Two](https://raw.githubusercontent.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/main/images/Deploy_Step_2.png "[Azure Portal Deploy Step Two")


Click the link below to automatically navigate to the Azure Custom deployment template: 

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMSUSSolutionAccelerators%2FSmart-Spaces-Sustainability-Solution-Accelerator%2Fmain%2Ftemplates%2Fmaster_accelerator_deployment_IOTHub_Assets.json)

Once in the Azure Custom deployment template:
- UPDATE the Deployment Parameters to MATCH YOUR installation:

    ##### resourcegroup:
        - Select a NEW or EXISTING Resource Group in your desired deployment region 
    ##### Region: 
        - NOTE: This should AUTO-FILL after the Resource Group has been selected 
    ##### Key Vault Name:
        - This should MATCH your existing Key Valult Name.
    ##### Server Name:
        - This should match the values for the SERVER from the Azure SQL values retrieved from Key Vault above.
    ##### SQL DB Name: 
        - This should match the values for the SQL DB Name from the Azure SQL values retrieved from Key Vault above.   
    ##### Administrator Login: 
        - This should match the values for the User Name from the Azure SQL values retrieved from Key Vault above.   
    ##### Administrator Login Password:
        - This should match the values for the User Password from the Azure SQL values retrieved from Key Vault above.   

##### Note: The resources that are provisioned in this script should be deployed into the SAME resource group as Step #1 above.


##### REMEMBER to click on the [Review+create] button at the bottom!


##### Note: The resources that are provisioned in this script should be deployed into the SAME resource group as Step #1 above.

1. When done - click on "Review + create" icon in the lower left of the web form.
2. Next, the script will display the status "Running final validation".
3. Next, you will see a message displayed " You will need to Agree to the terms of service below to create this resource successfully."
3. Click on the "Create" icon and the deployment process will begin.
 
  - This script will then automatically deploy the following Azure resources into your Azure subscription:
      - (1) IoT Hub
      - (1) IoT Hub - Managed Identity
      - (1) Script Container
      - (1) Deployment script
      - (4) TOTHub Devices
      - (3) App Service Plans
      - (3) Application Insights
      - (3) Azure Functions
      - (6) Logic Apps
      - (1) Stream Analytics job

### Step 3 - Run a Script in the Cloud Shell in the Azure Portal:
1. Please wait for the previous step to complete before running this next step.
2. This step entails navigating to the Azure Portal in a web browser, and then clicking on the "Cloud Shell" icon in the upper right-hand corner or the Azure Portal screen.

Below is an image of the "cloud Shell" in the navigation bar:

![Azure Portal Cloud Shell](https://raw.githubusercontent.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/main/images/CloudShellPortal.png "[Azure Portal Cloud Shell")

1. Once you click on the "cloud Shell " icon, 
the screen will split - and you will see a blue screen in the bottom portion of the Azure Portal web page

2. Below is an image of the "cloud Shell" after it has been opened in the Azure Portal:

    ![Azure Portal Cloud Shell](https://raw.githubusercontent.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/main/images/CloudShellPortal2.png "[Azure Portal Cloud Shell")
3. The next step is to load the following text into your favorite text editor: https://raw.githubusercontent.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/main/scripts/IOTHub_CLI_SCRIPT.txt
4. Next, modify the variables below to match YOUR azure environment. (These variables can be found at the top of the script): 
    - $IOTHubName = 'accelIoTHub'
    - $RGP = 'Accel-Smart-Spaces'
    - $KVName = "keyVaultxyz"
    - $FuncHVACName = "Accel-Smart-Spaces-FuncSMARTSPACE-HVAC"
    - $FuncSmartSpaceName = "Accel-Smart-Spaces-FuncSMARTSPACE"

5. Then COPY the updated script and PASTE the contents into the Azure Portal CLOUD SHELL Window (in BLUE).
6. Press the ENTER Key.
7. The script should start running and will perform the following steps:
        (1)  Initialize IOTHub Device Twin Properties for (4) Devices.
        (2)  Retrieve IOTHub Connection Strings
        (3)  Retrieve IOTHub Device Connection Strings
        (4)  SET Key Vault Secrets
        (5)  GET URI's of Key Vault Secrets
        (6)  SET Key Vault URI Variables
        (7)  CREATE / UPDATE FUNCTION APP SETTINGS
        (8)  Get Function App Principal ID + App Id
        (9)  Set Key Vault Access Policy - so secrets can be read from Azure Function App


### Post-Deployment Verification:
To confirm a successful deployment, perform the following Steps:

##### Confirm Azure Functions - HTTP REST Operations:
This step will confirm the IOTHub deployment and coresponding simulation functionality.

1. Download/Open the POSTMan DESKTOP Tool : https://www.postman.com/
2. Navigate to your installation of the Azure Function App named: Accel-Smart-Spaces-FuncSMARTSPACE-HVAC
3. On the left-hand navigation menu, click on the "Functions" icon.
4. Click on the NAME of the deployed function. It should be named "FuncSMARTSPACE-HVAC".
5. Once loaded, Click on the "Get Function Url" icon.
6. Click on the "Copy to clipboard" LINK.  
7. PASTE the Azure Function URL into the POSTMan tool URL address bar.
8. Select "POST" as the HTTP Operation.
9. Enter the following JSON string as the RAW BODY Contents:
          {"DeviceID":"smartspace-HVAC01-iotdevice"}
10. Click "SEND" in the POSTMan tool and wait for a response. 

A successful HTTP reponse message would be "200 OK". 
 
##### Confirm Azure SQL Database Table population:
This step will confirm the "back-end" deployment, the "front-end" IOTHub deployment, and all the corresponding simulation functionality.

1. Download/Open the SQL Server Management Studio (SSMS) Tool: https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16 
2. CONNECT to your newly installed Azure SQL Server Database instance:
        Server Type: Database Engine
        Server Name: <Your SQL Server Name>.database.windows.net
        Login:      <Your SQL User Name>
        Password:   <Your SQL User Password>
3. RIGHT-CLICK on the table: [dbo].[HVACUnitIntermediate] and select "Select top 1000 rows".
4. A new Query window will open and display the query results. 
5. You may wish to add the following SQL to the end of the query to see the most current records: ORDER BY [DateTimeUTC] DESC

A successful deployment will display newly added records to the Azure SQL table -> [dbo].[HVACUnitIntermediate]

### CONGRATULATIONS! 

You have now successfully provisioned and configured the Solution Accelerator!

- Refer to this link to configure Additional [BONUS_CONTENT](https://github.com/MSUSSolutionAccelerators/Smart-Spaces-Sustainability-Solution-Accelerator/blob/main/Documentation/BONUS_CONTENT.md) 

