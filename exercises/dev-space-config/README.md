# Dev Space Configurations

1. The starting point for this tutorial is the lobby of SAP Build, the central entry point for all SAP Build products.

  - Launch the cockpit <a href="https://emea.cockpit.btp.cloud.sap/cockpit/?idp=ahig3ghv4.accounts.ondemand.com#/globalaccount/CA50235469TID000000000741455808/subaccount/9dd54f49-c102-49a0-86de-389bc6100f91/subaccountoverview&//detail/19712c1d-d001-4bb5-85bf-823201b5021c/?layout=TwoColumnsMidExpanded" target="_blank">SAP BTP Cockpit</a>
  - After login launch <a href="[https://sap-build-eu10-trial-1-8l00vpnn.eu10.build.cloud.sap/lobby](https://sap-btp-experience-2026-shared-sb-x94grxn8.us10.build.cloud.sap/lobby)" target="_blank">SAP Build Lobby</a>🚀

<br>![](/exercises/dev-space-config/images/1-launch-sap-build.png)


2. In the SAP Build Lobby, go to the __Product switch icon__ (1) (top right corner) and __click__ on __"Dev Space Manager"__ (2) to open the SAP Build Code development space management UI.

<br>![](/exercises/dev-space-config/images/2-dev-space-manager.png)

3. Choose  __"Create Dev Space "__

<br>![](/exercises/dev-space-config/images/3-create-new-dev-space.png)

4. Specify the dev space name and select extensions:
- Name the dev space: __TechEdDA262_U<##>__
> In U<##>, where ## represents the unique number assigned to you at the start of the exercise (for example, 01).
- What kind of application you want to create: 
    - Select __Full Stack Application Using Productivity Tools__
- _Select_ 4 SAP HANA extensions
  - __SAP HANA Calculation View Editor__
  - __SAP HANA Database explorer__ (added by default)
  - __SAP HANA tools__
  - __SAP HANA Performance tools__
- _Apply by clicking __Create Dev Space__

<br>![](/exercises/dev-space-config/images/4-create-dev-space.png)

5. You will see that the newly created dev space is __starting__, once it is ready, that is indicated as __RUNNING__.  
Now you can go back to the SAP Build Code Lobby and create a Full-Stack Application project in the next excercise.

<br>![](/exercises/dev-space-config/images/5-space-starting.png)

6. Go back to your <a href="https://sap-build-eu10-trial-1-8l00vpnn.eu10.build.cloud.sap/lobby" target="_blank">SAP Build Lobby</a> to continue with the next exercise.

## Summary 📝

In this exercise, we configured a Dev Space in SAP Business Application Studio with SAP HANA extensions to enable SAP HANA database development. Verified tools and connectivity for creating and managing HANA artifacts.

Continue to - [Exercise 1 - Create a Full-Stack Project in SAP Build Code](../ex1/README.md)
