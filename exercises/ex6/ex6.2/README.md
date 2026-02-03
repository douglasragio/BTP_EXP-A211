# Exercise 6.2 - Add SAP HANA Cloud Native Artifacts to a CAP Application

In this exercise, you will extend a SAP CAP application by adding SAP HANA Cloud native artifacts, such as calculation views. You will create a join between risks and mitigations to analyze and visualize risk data more effectively. By leveraging HANA Cloud features, you will perform advanced data modeling and aggregation directly within the database. This hands-on activity demonstrates how to combine CAP application logic with powerful HANA Cloud capabilities for real-time insights.

## Exercise 6.2.1 -  Create Calculation View and Expose via CAP (SAP HANA Cloud) 

> __⚠️ NOTE__: In case of slowness or performance degradation, please refresh your browser.

>💡 __Insight corner__: Calculation Views and other HANA native artifacts allow you to leverage HANA specific features and optimizations that might not otherwise be available at the abstraction layers within the SAP Cloud Application Programming Model. Calculation Views are especially good at aggregation and filtering of large datasets.

1. Create a new Calculation View via View > Command Pallette and then type __SAP HANA: Create SAP HANA Database Artifact__ command pallete entry

<br>![](/exercises/ex6/ex6.2/images/1_cmdpalette.png) 

<br>![](/exercises/ex6/ex6.2/images/2_viewcreate.png) 

2. Create a calculation view called __V_RISKS__ of Data Category __DIMENSION__ and Dimension Type of __STANDARD__. Press Create.

<br>![](/exercises/ex6/ex6.2/images/3_cvdetails.png) 

- Create a new folder named calculationview under the src directory, then move the newly created calculation view (V_RISKS.hdbcalculationview) into this folder by dragging and dropping it.


3. The new artifact is created in the /db/src/calculationview folder. This way you can have a single HANA database model that contains both HANA native content and CAP generated content.

<br>![](/exercises/ex6/ex6.2/images/28_cvfolder.png) 

4. Create file /db/src/.hdiconfig and paste this code
``` 
{
    "file_suffixes": {
        "hdbcalculationview": {
            "plugin_name": "com.sap.hana.di.calculationview"
        }
    }
}
```   
   

6. Click on the __V_RISKS.hdbcalculationview__ to load the graphical calculation view editor.

<br>![](/exercises/ex6/ex6.2/images/5_viewcv.png) 

6. Now lets model the join relationship. Drop a join node into the modeling space.Click on the join and drop it at the end of the projection.

<br>![](/exercises/ex6/ex6.2/images/6_dropjoin.png) 

7. Use the __'+'__ sign sign to add tables to the node. On clicking, it will open a new dialog box to add the data source.

<br>![](/exercises/ex6/ex6.2/images/7_addds.png) 

8. Type in RISK and then select the table you created earlier via CDS called __RISK_MANAGEMENT_U00_RISKS__ 

<br>![](/exercises/ex6/ex6.2/images/28_addrisk.png) 

- Repeat the process to add the __RISK_MANAGEMENT_U00_MITIGATIONS__ and press Finish. 

<br>![](/exercises/ex6/ex6.2/images/29_addmiti.png) 

>__ℹ️ NOTE:__ Choose in the same order. 

<br>![](/exercises/ex6/ex6.2/images/8_addtables.png) 

9. You should see both artifacts in the join node.

<br>![](/exercises/ex6/ex6.2/images/9_artifacts.png) 

10. Double-click on the join node. A panel will open on the right.

>__ℹ️ NOTE:__ The two tables might not be visible initially. Please check your zoom level and try moving your cursor to locate them.

<br>![](/exercises/ex6/ex6.2/images/10_joinpanel.png) 

11. Drag and drop the ID field of the RISK to the RISK_ID field of the MITIGATIONS ( RISK_MANAGEMENT_U<##>_MITIGATIONS) field and Set the cardinality to 1..n.

<br>![](/exercises/ex6/ex6.2/images/11_setcard.png) 

12. In the Mapping tab, add the output columns as shown below by dragging them from the left panel to the right. You can select and drag multiple columns at once to speed up the process.

- Add the following columns: 

<strong>RISK_MANAGEMENT_U##_RISKS</strong>
- TITLE
- DESCRIPTION 
- IMPACT
- CRITICALITY 
- STATUS 

<strong>RISK_MANAGEMENT_U##_MITIGATIONS</strong>
- TITLE  
- DESCRIPTION
- COUNTER

<br>![](/exercises/ex6/ex6.2/images/30_addcols.png) 

- You get a dialog box to make the column name unique. Choose free text and type _MITIGATION_

<br>![](/exercises/ex6/ex6.2/images/27_uniquename.png) 

- your final join shoulld have the following output columns.

<br>![](/exercises/ex6/ex6.2/images/12_mapping.png) 

13. Connect the join node with the Projection node using ↗️

<br>![](/exercises/ex6/ex6.2/images/14_arrow.png) 
<br>![](/exercises/ex6/ex6.2/images/15_connect.png) 

14. Click on the Projection node and double-click on the _Join_1_ parent to add all the columns to the output.

<br>![](/exercises/ex6/ex6.2/images/16_joinmap.png) 


15. From the SAP HANA Projects view, press the Deploy button & Check the deployment log to make sure everything was successfully created in the database.

<br>![](/exercises/ex6/ex6.2/images/17_deploy.png) 
<br>![](/exercises/ex6/ex6.2/images/18_deploylogs.png) 

16. Open the HDI Container in the Database Explorer. Choose your assigned user.

<br>![](/exercises/ex6/ex6.2/images/19_openhdi.png) 

17. Under Column Views you will find your Calculation View. Choose __Open Data__.

<br>![](/exercises/ex6/ex6.2/images/20_opendata.png) 

## Exercise 6.2.2 - Create calculation view proxy entity

We now want to expose our Calculation View to the Cloud Application Programming model by creating a “proxy” entity for the view in the CDS data model.

18. Return to the project and open _'_schema.cds_'_.

<br>![](/exercises/ex6/ex6.2/images/21_schemadef.png) 

19. We need to add a matching entity definition for the Calculation View. This means redefining all the column names and data types / lengths. Doing so manually would be error prone, but the '__hana-cli__' has a utility that will help. Open a new terminal. 

20. We need our proxy entity to be created without the namespace in our current _schema.cds_. Therefore comment out the namespace line and add all the existing content except the __using ...__ line in a new context for __Risk_Management_U00__

<br>![](/exercises/ex6/ex6.2/images/31_modifyCV.png) 

>💡 __Insight Corner__: HANA CLI is a command-line tool for interacting with SAP HANA databases, allowing you to manage data, execute SQL, and deploy artifacts efficiently from the terminal.

- Install the '__hana-cli__' with the following command:
```
npm install -g hana-cli
```
- Run the command cd db to navigate to the database directory
```
cd db
```
- Now issue the below command. With this command you are looking up the definition of the view but asking for the output (-o) in the CDS format
```
hana-cli inspectView -v V_RISKS -o cds
```
<br>![](/exercises/ex6/ex6.2/images/22_hanacli.png) 

>💡 __Insight corner__: You can build a developer-centric SAP HANA command line tool called __hana-cli__, particularly designed to be used when performing SAP HANA development.

- Run the below command to return to orginal app directory.
```
cd ..
```

21. Copy this block from the terminal and paste it into the _schema.cds_ file at the end outside the context block.

<br>![](/exercises/ex6/ex6.2/images/23_copy.png) 

>⚠️ __Disclaimer:__
Any code snippets provided in this exercise are based on the namespace risk_management_u00.
Please remember to replace this namespace with your own, following the format risk_management_u<##>, where ## is the unique number assigned to you at the beginning of the exercise.
Failing to update the namespace may lead to build or deployment issues.

```cds
// namespace Risk_Management_U00;

using {
  cuid,
  managed
} from '@sap/cds/common';

using {API_BUSINESS_PARTNER} from '../srv/external/API_BUSINESS_PARTNER';

context Risk_Management_U00 {
  entity Risks : cuid, managed {
    title           : String(100)
    @mandatory;
    description     : String(500);
    impact          : Integer;
    criticality     : Integer;
    status          : String(20);
    mitigations     : Association to many Mitigations
                        on mitigations.risk = $self;
    BusinessPartner : Association to one API_BUSINESS_PARTNER.A_BusinessPartner;
  }

  annotate Risks with @assert.unique: {title: [title], };

  entity Mitigations : cuid, managed {
    title       : String(100)
    @mandatory;
    description : String(500);
    counter     : Integer;
    risk        : Association to one Risks;
  }

  annotate Mitigations with @assert.unique: {title: [title], };
}

@cds.persistence.exists 
@cds.persistence.calcview 
Entity V_RISKS {
key     TITLE: String(100)  @title: 'TITLE: TITLE' ; 
        DESCRIPTION: String(500)  @title: 'DESCRIPTION: DESCRIPTION' ; 
        IMPACT: Integer  @title: 'IMPACT: IMPACT' ; 
        CRITICALITY: Integer  @title: 'CRITICALITY: CRITICALITY' ; 
        STATUS: String(20)  @title: 'STATUS: STATUS' ; 
        TITLE_MITIGATION: String(100)  @title: 'TITLE_MITIGATION: TITLE_MITIGATION' ; 
        DESCRIPTION_MITIGATION: String(500)  @title: 'DESCRIPTION_MITIGATION: DESCRIPTION_MITIGATION' ; 
        COUNTER_MITIGATION: Integer  @title: 'COUNTER_MITIGATION: COUNTER_MITIGATION' ; 
}

```

>💡 __Insight corner__: CDS does have an annotation called @cds.persistence.exists. This annotation allows you to re-define an existing DB object and CDS won’t attempt to create or alter it. It will just assume it already exists in the matching state.There is also the annotation @cds.persistence.calcview. This will further tell the Cloud Application Programming Model that this target entity is also a Calculation View.

22. Now open the service.cds file from the /srv folder. Add this new Calculation View based entity to the CAP service as read-only.

```cds
    using V_RISKS from '../db/schema.cds';
```

```
    @readonly
    entity V_Risks  as projection on V_RISKS;
```

<br>![](/exercises/ex6/ex6.2/images/24_servicemod.png) 

23. From the terminal, issue the below comamnd.
```
cds build --production
```
<br>![](/exercises/ex6/ex6.2/images/25_cdsbuild.png) 

24. Although we didn’t add any new database artifacts to the project, the addition of an entity to the service layer causes new HANA SQL views to be generated within SAP HANA. Therefore we need to deploy to the database using the SAP HANA Projects view before we can test.

<br>![](/exercises/ex6/ex6.2/images/17_deploy.png) 

25. You can now run the application by clicking on the icon as shown below:

<br>![](/exercises/ex6/ex6.2/images/32_run.png) 

- This will open a new tab, where you can now view the service layer
<br>![](/exercises/ex6/ex6.2/images/33_view.png) 

<br>![](/exercises/ex6/ex6.2/images/26_viewodata.png) 

- Close all open tabs before proceeding to the next exercise.


Continue to - [Exercise 6.3 - Building and Integrating a HANA Stored Procedure into a CAP Service](../ex6.3/README.md)
