# Exercise 6 - Bind your application to an HDI Container and view the data in SAP HANA Database Explorer

In this exercise, you will create a <a href="https://help.sap.com/docs/SAP_HANA_PLATFORM/3823b0f33420468ba5f1cf7f59bd6bd9/3ef0ee9da11440e4b01708455b8497a9.html" target="_blank">SAP HANA Deployment Infrastructure (HDI) container</a>
and bind that container to our project and deploy the project to SAP HANA Cloud.


## Login to your Cloud Foundry space and create a HDI container 👤🔓

1. _Login_ to __Cloud Foundry "environment"__ under your BTP subaccount and your __Cloud Foundry "space"__.  
- Click on __"Cloud Foundry login"-icon__ in the left-side activity bar
![alt text](/exercises/ex6/images/1_cflogin.png)
  - or alternative search for the CF login command in the top command search  ![alt text](/exercises/ex6/images/2_cfcommandsearch.png)   

2. Verify the correct __"Cloud Foundry Endpoint URL"__ from the image below.
3. then select SSO Passcode and _click_ __"Open a new browser ..."__ to generate and copy a SSO passcode.

<br>![](/exercises/ex6/images/3_gensso.png)

4.  This will open a new browser tab, enter __academy platform__ and click on __Sign in with alternative provider__
   ahig3ghv4-platform
<br>![](/exercises/ex6/images/4_enterap.png)
- __Copy__ the passcode by clicking on the icon.
<br>![](/exercises/ex6/images/5_copyicon.png)

5.  __Paste the Passcode__ and then click __"Sign in"__
![alt text](/exercises/ex6/images/6_pastepasscode.png) 

- Choose the __"Cloud Foundry Target environment Org ID"__ and __"CF Space"__  as shown below:
![alt text](/exercises/ex6/images/7_cf_target.png)    
  

## Prepare your multi-target application & bind to an HDI container 📎

1. The next step is to __prepare the Multi-Target Application definition__ (mta.yaml-file)  
Service- and target runtime-dependencies are declared within the mta.yaml-file, using some terminal commands, the file can be easily created and filled with the information about the SAP HANA and HDI container runtime.  

Go to Project Explorer, an __open a New Terminal__ window
- From top-menu in the left-side activity bar, select Terminal
- Click on __"New Terminal"__. 
<br>![](/exercises/ex6/images/8_new_terminal.png)
  
- Enter and execute the below commands to add mta and hana. 

```shell
cds add mta
```
```shell
cds add hana
```
2. Once added successfully, you will see the __mta.yaml__ file in your project structure & your SAP HANA Project module appears in the __SAP HANA Projects__ view.

> __ℹ️ NOTE__: You can move the SAP HANA Projects view to above by simply dragging and dropping.

<br>![](/exercises/ex6/images/9_mtaadd.png)

3. Now __create__ and __bind__ your __HDI container__
- Under SAP HANA Projects, click on the __bind icon__, select Bind to an HDI Container.

![alt text](/exercises/ex6/images/10_bindicon.png)

![alt text](/exercises/ex6/images/30_bind.png)


- Click on __"+Create a new service instance"__  
![alt text](/exercises/ex6/images/11_createsrv.png)

- Enter the __name of the HDI container__ with the one generated and __press "enter"__ to confirm.  
![alt text](/exercises/ex6/images/12_srvname.png)

- This will take a couple of seconds. 
Click on __"Enable and do not ask again"__.  
<br>![](/exercises/ex6/images/13_enable.png)

- Now your HDI container service is created and is bound to your CAP project.
![alt text](/exercises/ex6/images/14_bound.png)

4. Open the Database Explorer by clicking on the cubical icon.

![alt text](/exercises/ex6/images/15_opendb.png)

<br>![](/exercises/ex6/images/16_dia.png)

- Enter __academy platform__ and sign in & click on __authorize__ 

<br>![](/exercises/ex6/images/17_signindb.png)
<br>![](/exercises/ex6/images/18_auth.png)


5. In the SAP HANA Database Explorer, for the connected HDI container, you will see that the none of the (here table) artifacts are yet generated as they still require to be deployed to the container. Refer to the image below.
<br>![](/exercises/ex6/images/19_hdi.png)

6. Return to your project, navigate to the SAP HANA Projects tree, and deploy your HDI artifacts.
- _Click_ on the __rocket icon__, and this will deploy your project to your SAP HANA Cloud Instance
<br>![](/exercises/ex6/images/20_deploy.png)
<br><br>

## Exercise 6.3 - Go to SAP HANA Database explorer and check the tables 🗄️📁

7. Click on database exploerer icon under SAP HANA Projects again, which will open the SAP HANA Database explorer view. 
<br>![](/exercises/ex6/images/15_opendb.png)

8. Generated (table) artifacts are now shown as below.  
However, the sample data that was generated before will not have been populated into the below tables. 
<br>![](/exercises/ex6/images/21_dbx.png)

9. Populating tables with initial data.  
The test data that was generated in Exercise 2 (under /test in the project tree), will not be populated into the target HANA runtime as it is targeted for CF design-time testing purpose only.  
In order to add the data to the target HDI container,

- Create a new folder named __data__ inside the db folder, as shown below.
![alt text](/exercises/ex6/images/22_newfolder.png)

- copy the csv files from the test-folder that is shown below.
![alt text](/exercises/ex6/images/23_copydata.png)

- paste the csv files in the newly created data folder ( db>data)
![alt text](/exercises/ex6/images/24_pastedata.png)

- As a result, the folder structure should look as follows
![alt text](/exercises/ex6/images/25_folderstr.png)


10. Now, the deploy to the HDI container again
![alt text](/exercises/ex6/images/20_deploy.png)

11. Once deployed successfully
- Navigate to SAP HANA Database explorer and review the data.

- Review the tables and the data that is generated by the cds definitions and annotations.
![alt text](/exercises/ex6/images/26_finaldb.png)

12. Go back to your project,

- Open the Project Overview from the left-side activity bar to explore your project.

- Navigate to the Risks entity within the Data Models.

- Click Set Data to view and edit the initial data in the Data Editor.

> __ℹ️ NOTE__: Sample data is used for design-time testing purposes only, whereas initial data is also populated into the target runtime used for production.
![alt text](/exercises/ex6/images/27_opendataeditor.png)
- We are now examining the sample data generated by SAP Joule and the initial data copied into HANA.

![alt text](/exercises/ex6/images/28_mitidata.png)

![alt text](/exercises/ex6/images/29_riskdata.png)

- Close all open tabs before proceeding to the next exercise.

## Summary  📝

You've now created a new HDI container and bound to your CAP Project. You can deploy the project to your SAP HANA Cloud instance and view the data in SAP HANA database explorer.

Continue to - [Exercise 6.1 - Run your application for testing](../ex6/ex6.1/README.md)
