This a customized version of the Node.js sample app that is available with the [Watson Machine Learning Service on IBM Cloud](http://www.ng.bluemix.net/docs/#services/PredictiveModeling/index.html).
This version of the app is to be used with the Data Science Experience Service to test a predictive model developed in the lab.

See the [original app](https://github.com/pmservice/predictive-modeling-samples) for a walkthrough of the source code.



# Lab - Create and deploy a scoring model to predict heart failure on IBM's Cloud with the Watson Data Platform

© Copyright IBM Corporation 2018

IBM, the IBM logo and ibm.com are trademarks of International Business Machines Corp., registered in many jurisdictions worldwide. Other product and service names might be trademarks of IBM or other companies. A current list of IBM trademarks is available on the Web at &quot;Copyright and trademark information&quot; at www.ibm.com/legal/copytrade.shtml.

This document is current as of the initial date of publication and may be changed by IBM at any time.

The information contained in these materials is provided for informational purposes only, and is provided AS IS without warranty of any kind, express or implied. IBM shall not be responsible for any damages arising out of the use of, or otherwise related to, these materials. Nothing contained in these materials is intended to, nor shall have the effect of, creating any warranties or representations from IBM or its suppliers or licensors, or altering the terms and conditions of the applicable license agreement governing the use of IBM software. References in these materials to IBM products, programs, or services do not imply that they will be available in all countries in which IBM operates. This information is based on current IBM product plans and strategy, which are subject to change by IBM without notice. Product release dates and/or capabilities referenced in these materials may change at any time at IBM&#39;s sole discretion based on market opportunities or other factors, and are not intended to be a commitment to future product or feature availability in any way.

# Overview

This lab is designed to demonstrate how to use IBM Data Science Experience to build a predictive model within a Jupyter Notebook. The predictive model is then deployed to the Watson Machine Learning Service in IBM Cloud where it is consumed by users accessing a Node.js application.

![Flow](images/Picture35.png)

1. The developer creates an IBM Data Science Experience Workspace.
2. IBM Data Science Experience depends on an Apache Spark service.
3. IBM Data Science Experience uses Cloud Object storage to manage your data.
4. This lab is built around a Jupyter Notebook, this is where the developer will import data, train, and evaluate their model.
5. Import data on heart failure.
6. Trained models are deployed into production using IBM's Watson Machine Learning Service.
7. A Node.js web app is deployed on IBM Cloud calling the predictive model hosted in the Watson Machine Learning Service.
8. A user visits the web app, enters their information, and the predictive model returns a response.


## Prerequisites

* IBM Cloud supported [web browser](https://console.ng.bluemix.net/docs/overview/prereqs.html#prereqs)
* An [IBM Cloud Account](https://console.bluemix.net)

## Before you begin
To be able do this lab a IBM Cloud account is necessary. If you don't have one yet -- or you did not complete the initial set up of your IBM Cloud account -- follow the steps below.

Your account must have enough resources available for at least 1 application (128MB) and 6 services.

### Already registered and completed set-up

When you already registered and completed the initial set-up of your IBM Cloud account, you directly jump to [Create a space in Cloud US region](#create-a-space-in-cloud-us-region).

### Not registered

Use Ctrl-click (or the equivalent action for your system) to open the [Sign Up for IBM Cloud](https://console.bluemix.net/registration/) page in a separate tab. Fill in the form and click **Create Account** to complete the registration and free access to IBM's Cloud. You will receive an activation mail in your inbox.

### First time login

Use Ctrl-click (or the equivalent action for your system) to open the [Login to IBM Cloud](https://console.bluemix.net/login) in a separate tab. First time users need to complete a 4-step wizard. This starts by accepting the terms & conditions.

  ![Terms & conditions][1]

Define a name for your organization.

  ![Organization name][2]

Choose a name for your space. Typically `dev` would be a good name for your first space.

  ![Space name][3]

On the last page, click **I'm Ready** to complete the set up process.

### Create a space in IBM Cloud US region

For the remainder of this lab we switch to the **US region** of IBM Cloud. For this, use Ctrl-click (or the equivalent for your system) to open the IBM Cloud dashboard. Click your account and choose **US South** as your active region.

![Select US region][5]

If you are all OK, you get the dashboard. Otherwise, you will be asked to create your first space in this region -- as depicted in the screenshot below. Typically `dev` would be a good name for your space.

![Create space in US][4]

### Download the sample patient data

Use Ctrl-click (or the equivalent action for your system) to open the <a href="https://ibm.box.com/v/patientdataV6" target=download>patientdataV6.csv</a> CSV file in a separate tab. Click **Download** to download this file to your own device.

  ![Download CSV][14]

Congrats, you're now ready to start your data science experience :smiley:!!

# Step 1: Sign up for IBM Data Science Experience

IBM Data Science Experience is an interactive, collaborative, cloud-based environment where data scientists can use multiple tools to activate their insights. In this part of the lab you will sign up for a 30-day trial of IBM Data Science Experience.

  1.  In a web browser navigate to [https://datascience.ibm.com](https://datascience.ibm.com).

  2.  Click on **Sign Up** at the top right.

  ![Sign Up][36]

  3. Click on **Sign in with your IBM id** and enter your IBM Cloud credentials.

  ![Sign In][37]

  4. Follow the instructions to complete the sign up for IBM Data Science Experience. Note that a new IBM Cloud service, _Data Science Experience_ will be created for you.
  

  ![DSX dashboard][6]  
  

# Step 2: Deploy the testing application
In this part of the lab you'll deploy the application that you will use later to test the predictive model that you create.

  1. Use Ctrl-click on the **Deploy to IBM Cloud** button below to open the deployment process in a separate tab.

  [![Deploy to IBM Cloud](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/justinmccoy/watson-dojo-pm-tester.git)

  2. Log in into IBM Cloud with your credentials by clicking on the **Log in** link at the top right.

  3. Make sure to deploy the application to the same region and space as where the *Apache Spark* and *Cloud Object Storage* services were created when you signed up for IBM Data Science Experience. Please take note of this space as later in this lab the Watson Machine Learning service needs to be deployed into the same space.

  4. Click on **Deploy** to deploy the application.

  ![Deploy][7]

  5. A Toolchain and Delivery Pipeline will be created for you to pull the app out of Github and deploy it in to IBM Cloud. Click on the Delivery Pipeline tile to see the status of the deployment.

  ![Toolchain][21]

  6. Wait for the **Deploy Stage** to complete successfully.

  ![Deploy Stage][22]

# Step 3: Create an instance of the Watson Machine Learning Service

In this part of the lab, you'll create an instance of the Watson Machine Learning service and bind it to the application that you created in Step 2.

  1. In your browser go to the IBM Cloud Dashboard and click **Catalog**.

  2. In the navigation menu at the left, select **Data  & Analytics** (under **Platform**) and then select **Machine Learning**.

  ![Watson ML Service][8]

  4. Verify this service is being created in the same space as the app in Step 2.

  5. Click **Create**, followed by **Restage** when you’re prompted to restage your application.

  ![Connect to Service][23]
  
  6. On the Watson ML Dashboard select **Connections** on left menu panel, and _Create Connection_.  Select the application that you deployed earlier in Step 2 of this lab connecting this Watson ML service to the Cloud Foundry application deployed.

  ![Connect to Service][9]
  
  ![Connect to App][41]

  6. Go back to the IBM Cloud dashboard and wait until the app shows that it is running again.

  ![Overview DSX services and test app][10]

## Step 4: Create a project in IBM Data Science Experience and bind it to your Watson Machine Learning service instance

In this part of the lab you will create a new project in IBM Data Science Experience, and bind it to your instance of the Watson Machine Learning service.

  1. In a new browser tab go to [https://datascience.ibm.com](https://datascience.ibm.com).

  2. Click on **Sign In** at the top of the page.

  3. From the dashboard, click on **New Project** from the dashboard.

  ![Create New Project][11]

  4. DSX projects depend on two services: Object Storage, and a Compute Engine.  If you don't already have Object Storage or a Compute Engine, you can create a new instance of each service while defining a new project.  The _New Project_ panel is easy to use, either select an existing service on the right, or create a new one.  In the example below services need to be created. 

  ![Create Services][38]

 **Note:** Services created must be in the same region, and space, as your Data Science Experience service.

  5. Enter _Watson ML Integration_ as the project name and click **Create**. 
  
  6. From within the new project _Overview_ panel, click _Add to project_ on the top right, selecting _Data asset_. 
  
  ![Add to Project][39]
  
  A panel on the right of the screen appears, select _load_ and click on _Browse_ to upload the data file you'll use to create a predictive model.

  ![Add Data Asset][40]

  7. On your laptop, browse to the location where you downloaded the file **patientdataV6.csv** in the section [Download patient data](#download-patient-data) of this lab. Select the file and click on Open (or the equivalent action for your operating system).

  8. Once successfully uploaded, the file should appear in the **Data Assets** section.

  ![Data Assets][13]

  8. Click on **Settings** for the project.

  ![Settings][24]

  9. Click on add associated service and select **Machine Learning**.

  ![Associate Service][25]

  10. Choose your existing Machine Learning instance and click on **Select**.

  ![Add Watson ML Service][26]

  11. The Watson Machine Learning service is now listed as one of your **Associated Services**.

  ![Associated Services][27]

  12. Leave the browser tab open for later.
  
## Step 5: Save the credentials for your Watson Machine Learning Service

In this part of the lab you’ll save the credentials for your Watson Machine Learning instance so you can use it later in your code.

  1. In a different browser tab go to [http://console.bluemix.net](http://console.bluemix.net) and log in to the Dashboard.

  2. Click on your Watson Machine Learning instance under **Services**.

  ![Save Credentials][28]

  3. Click on **Service credentials** and then on **View credentials** to see the credentials.

  ![View Credentials][29]

  4. Save the username and password to a text file on your machine. You’ll need this information later in your Jupyter notebook.

## Step 6: Create a notebook in IBM Data Science Experience

In this part of the lab you’ll create a Jupyter notebook and import the code to create a predictive model.

  1. If you don't have your newly created Project open, first click **Projects -> View All Projects**, and then select your newly created project from Step 4. Next, in the Data Science Experience browser tab click on **Overview** and then click **add notebooks**.

  ![Add Notebook][15]

  2. Click on **From URL** and name the notebook _Apache Spark integration with Watson ML_.

  3. Under **Notebook URL** provide the following url: https://github.com/justinmccoy/watson-dojo-pm-tester/blob/master/demo1.ipynb.

  ![Create Notebook][16]

  4. Click **Create Notebook** to create the new notebook.

  5. Leave your browser tab open for the next part.


## Step 7: Run the notebook in IBM Data Science Experience

In this part of the lab you will run the Jupyter Notebook code creating a predictive model, and save it in the Watson Machine Learning Service.

  1. Place your cursor in the first code block in the notebook.

  ![Insert Spark Data Frame Step 1](images/Picture23.png)

  2. Click on the **Find and Add** data icon -- see step 1 in diagram below -- and then select **Insert to code** under the file **patientdataV6.csv**. This is step 2 in diagram below. Finally select **Insert Spark Data Frame** -- which is step 3 in diagram below.

  ![Insert Spark Data Frame Step 2][30]

  3. You now can load data from Object Storage into a Spark Data Frame. Make sure to rename the variable to `df_data` and add `.option('inferSchema','True')\`.

  ![Insert Spark Data Frame Step 3](images/Picture25.png)

  4. Click on the **Run** icon to run the code in the cell.

  ![Run Notebook][31]

  5. Move your cursor to each code cell and run the code in it. Read the comments for each cell to understand what the code is doing. **Important** when the code in a cell is still running, the label to the left changes to **In [\*]**:.
  Do **not** continue to the next cell until the code is finished running.

  6. When you get to the cell that says **Stop here !!!!** insert the username and password that you saved from your Watson Machine Learning instance into the code before running it.

  ![Run Notebook](images/Picture27.png)

  7. Continue running each cell until you finish the entire notebook.

## Step 8: Deploy the saved predictive model as a scoring service

In this part of the lab you’ll deploy the model you saved by running the Python notebook as a scoring service in Watson Machine Learning.

1. In a different browser tab go to [http://console.bluemix.net](http://console.bluemix.net) and log in to your dashboard.

2. Click on the entry for your Watson Machine Learning service under **Services**.

3. Click **Manage** and then click on the **Launch Dashboard** button in the Watson Machine Learning tile.

![Launch Dashboard][17]

4. Your saved model should appear. Under **Actions** select **Create Deployment**.

![Create Deployment Step 1][18]

5. Name the deployment _Heart Failure Prediction Model Deployment_ and click **Save**. Keep the type of deployment set to the default value 'Online'.

![Create Deployment Step 2][19]

6. Your model should now be deployed and visible as a Deployment with status 'ACTIVE'.

![Create Deployment Step 3][20]

7. Restart the Node.js Web App. For this, return to your IBM Cloud Dashboard and select the restart icon to restart the web application.

![Restart WebApp][32]


## Step 9: Test the deployed Model

In this part of the lab you’ll test the deployed model with the Node.js application that you deployed earlier.

1. Click on the application URL to open the application in a separate tab.

![Test Deployed Model][33]

3. When the application appears click on **Score now** to test the scoring model with the default values.  

4. Verify that the model predicts that there is a risk of heart failure for the patient with these medical characteristics.

![Score][34]

5. **Click Close**. Run the app again with the following parameters.

![Score](images/Picture33.png)

6. Verify that the model predicts that there is not a risk of heart failure for the patient with these medical characteristics.

![Score][35]

Congratulations, you successfully created a predictive model in Apache Spark and deployed and tested it using the Watson Machine Learning Service on IBM Cloud :smiley:!!

## Additional links

* More data science journeys on IBM Code: [https://developer.ibm.com/code/journey/category/data-science/](https://developer.ibm.com/code/journey/category/data-science/)
* IBM Data Science Experience: [https://www.ibm.com/analytics/us/en/watson-data-platform/data-science-experience/](https://www.ibm.com/analytics/us/en/watson-data-platform/data-science-experience/)
* Watson Data Platform: [https://www.ibm.com/analytics/us/en/watson-data-platform/](https://www.ibm.com/analytics/us/en/watson-data-platform/)


[1]: images/terms.png?raw=true
[2]: images/create-org.png?raw=true
[3]: images/create-space.png?raw=true
[4]: images/create-us-space.png?raw=true
[5]: images/select-region.png?raw=true
[6]: images/dsx-dashboard.png?raw=true
[7]: images/pipeline.png?raw=true
[8]: images/create-ml-instance.png?raw=true
[9]: images/connect-to.png?raw=true
[10]: images/overview-services-and-app.png?raw=true
[11]: images/new-project.png?raw=true
[12]: images/browse-file.png?raw=true
[13]: images/data-assets.png?raw=true
[14]: images/download-csv.png?raw=true
[15]: images/add-notebook.png?raw=true
[16]: images/create-notebook.png?raw=true
[17]: images/launch-dashboard.png?raw=true
[18]: images/create-deployment1.png?raw=true
[19]: images/create-deployment2.png?raw=true
[20]: images/create-deployment3.png?raw=true
[21]: images/toolchain.png?raw=true
[22]: images/deploy-stage.png?raw=true
[23]: images/restage-app.png?raw=true
[24]: images/settings.png?raw=true
[25]: images/add-associated-service.png?raw=true
[26]: images/choose-ml-service.png?raw=true
[27]: images/associated-services.png?raw=true
[28]: images/services-overview.png?raw=true
[29]: images/ml-credentials.png?raw=true
[30]: images/insert-credentials2.png?raw=true
[31]: images/run-notebook.png?raw=true
[32]: images/restart-app.png?raw=true
[33]: images/open-app.png?raw=true
[34]: images/failure-yes.png?raw=true
[35]: images/failure-no.png?raw=true
[36]: images/dsx-signup.png?raw=true
[37]: images/use-existing-id.png?raw=true
[38]: images/create-services.png?raw=true
[39]: images/add-to-project.png?raw=true
[40]: images/add-data-asset.png?raw=true
[41]: images/connect-to-app.png?raw=true

