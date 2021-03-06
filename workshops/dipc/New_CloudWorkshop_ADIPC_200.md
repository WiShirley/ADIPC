![](images/300/AgentImage05-WorkshopHeader.PNG)

Update: May 30, 2018

## Introduction - Remote Agent Install and On-prem to On-prem database Synchronization

This lab covers installation and configuration of DIPC remote agent along with synchronization of two on-prem database schemas. Agents allow synchronization of data from sources outside Oracle Cloud. The target and source schemas will reside in the same database.
These labs are being done in the GSE environment.

This lab supports the following use cases:
-   Configure Remote DIPC Agent
-   Synchronize two On-Premise Databases

## Objectives

-	Ensure Remote Agent is trusted by DIPC instance
-   Agent Download
-   Agent Installation and Configuration
-   Configure Agent SSL
-	Agent Administration - Starting and Stopping.
-   Synchronize two On-Premise Databases
   
## Prerequisites

-   Cloud Dashboard URL and login credentials
-   1 DIPC Instance - DIPCINST
-   2 DBCS Instances - DBCSAMER, DBCSEMEA
-   1 Compute Instance - OnPremiseVM
-   Private keys in OpenSSH format for all instances 
-   OnPremiseVM public IP address
-   VNC Client
-	Putty for ssh connection to instances


### **STEP 1**: [DIPC 18.2.3] Determine the DIPC Console url

-   Log into dashboard url provided with your single signon (update url for delivery)
-   https://myservices-gse00015126.console.oraclecloud.com/mycloud/cloudportal/dashboard

-   Enter single signon user id or id provided
	![](images/300/AgentImage011-DemoLogin.png)

	![](images/300/AgentImage012-DemoLogin.png)

-   Click "Data Integration Platform Cloud" (no login info was required)
-   Login may be required if accessing directly using console url
-   https://myservices-gse00015126.console.oraclecloud.com/mycloud/cloudportal/dashboard

	![](images/300/AgentImage015-DIPC_Console.png)

-   Click "Open Service Console" in top right of page to view DIPC instance "DIPCINST"
	![](images/300/AgentImage016-DIPC_Console.png)

-   Click menu on right and right click "Data Integration Platform Console"
	![](images/300/AgentImage017-DIPC_Console.png)

-   Click "Copy link address" and take note
-   This url will be used in the OnPremiseVM to download the agent
	![](images/300/AgentImage018-DIPC_Console_url.png)

### **STEP 2**: [OnPremiseVM] Log into OnPremiseVM using "VNC Viewer"

-   Start "VNC Viewer" and enter the OnPremiseVM IP provided with port 5901
-   		VNC Server: <OnPremiseVM IP:5901>
-   		Encryption: "Let VNC Server choose"
-   Click "Connect"
	![](images/300/AgentImage020-VNC_Login.png)

-   Click "Continue" for VNC Viewer Encryption
	![](images/300/AgentImage020-VNC_Login_Encryption.png)

-   Enter the VNC Viewer - Authentication password provided
-   Click "Ok"

	![](images/300/AgentImage021-VNC_Login_Authentication.png)
	![](images/300/AgentImage022-VNC_Desktop.png)

### **STEP 3**: [OnPremiseVM] Navigate to the DIPC Console

-   Open the firefox web browser and enter the DIPC Console url noted in step 1
	![](images/300/AgentImage030-VNC_DIPC_Console.png)

-   Navigate to the Agent page (Click Agent in left toolbar).
	![](images/300/AgentImage031-VNC_DIPC_Agent_page.png)

-   Select drop down menu and select zip file for your Operating System

	![](images/300/AgentImage040-DownloadAgent.png)

-   Confirm your agent download selection
-   Click "OK" to confirm selection

	![](images/300/AgentImage045-DownloadAgent.png)

### **STEP 4**: [DIPC 18.2.3] Save the agent zip file to the download directory

-   Save the agent file to the download directory

	![](images/300/AgentImage050-DownloadAgentSave.png)

	![](images/300/AgentImage051-DownloadAgentSave.png)


### **STEP 5**: [OnPremiseVM] View and unzip agent file

-   Launch Putty for connection to "OnPremiseVM"
-   Enter OnPremiseVM IP address
	![](images/300/AgentImage055-1-LaunchPutty.png)

-   Expand "Connection"and click "Data" in left pane to add username "opc" 
	![](images/300/AgentImage055-2-PuttyAutoLoginUsername.png)

-   Expand "Connection" -> "SSH" and click "Auth" in left pane to add OnPremiseVM private key
-   Click "Open" to connect to OnPremiseVM
	![](images/300/AgentImage055-3-PuttyAddPrivKey.png)

-   Navigate to the target directory "/home/opc/dipcagent"
-   $ cd /home/opc/dipcagent
-   $ ls
-   View agent zip file "agent-linux.64.bit.zip"

	![](images/300/AgentImage055-4-ViewAgentFileOnPrem.png)

-   unzip agent file
    -   $unzip agent-linux.64.bit.zip

	![](images/300/AgentImage060-UnzipAgent.png)


### **STEP 6**: [DIPC 18.2.3] Show current agent page in DIPC console

-   Navigate to DIPC console on VM "DIPC 18.2.3"
-   Click "Agent" in the left toolbar
-   There is only one local agent install 


	![](images/300/AgentImage065-Current_DIPC_Agent_Page.png)


### **STEP 7**: [OnPremiseVM] Install the Agent from On-Prem Console *MODIFY FOR GSE*

-   Open a terminal in On-Prem VM
-   Navigate to the agent directory
-   Execute command to install agent using password - #!hyper1on!#
    ./dicloudConfigureAgent.sh -dipchost=10.0.0.3 -dipcport=7003 -user=weblogic -authType=BASIC

-   Set the "dipchost" parameter to 10.0.0.3
-   This configuration does not have IDCS so use "BASIC" for parameter AuthType


	![](images/300/AgentImage075-Execute_Agent_Install.png)
   
-   Output shows agent created

-   ![](images/300/AgentImage080-Execute_Agent_Install.png)


### **STEP 8**: [OnPremiseVM] Modify Agent Parameter 

-   Modify agent port "agentPort" in parameter file "agent.properties" to 7010
-   path to parameter file: 
-   /home/DIPC/Documents/dicloud/agent/dipcagent001/conf 


	![](images/300/AgentImage085-ModifyAgentParameter.png)

	![](images/300/AgentImage086-ModifyAgentParameter.png)

### **STEP 9**: [OnPremiseVM] Start Agent

-   Navigate to the agent bin directory
    -   $ cd /home/DIPC/dipcagent/dicloud/agent/dipcagent001/bin
-   Start agent using script startAgentInstance.sh
    -   $ ./startAgentInstance.sh


	![](images/300/AgentImage090-StartAgent.png)

	![](images/300/AgentImage091-StartAgent.png)

### **STEP 10**: [DIPC 18.2.3] View Remote Agent in DIPC Console

-   Navigate to DIPC Console
-   Click "Agents" in left toolbar


	![](images/300/AgentImage095-Confirm_Agent_DIPC_Console.png)

### **STEP 11**: [DIPC 18.2.3] Ensure local and Remote Agents are started

-   Start agents as needed from Agent bin directory
    -   $ ./startAgentInstance.sh


	![](images/300/AgentImage096-Confirm_SRC_TRG_Agent_Started.png)

### **STEP 12**: [OnPremiseVM] Review On-Prem Schema

-   Connect to remote schema and view tables and row count


	![](images/300/AgentImage100-ReviewOnPremSchema.png)

### **STEP 13**: [DIPC 18.2.3] Create Source Connection to On-Prem Schema

-   Navigate to DIPC console
-   Click "Home" in left toolbar and click "Create Connections"


	![](images/300/AgentImage101-CreateSourceConnection.png)

-   Enter source connection information for On-prem schema

    - Name:        ONPREM_SRC
    - Description: Connection to on-prem database schema with source tables
	- Agent:       localhost:7010
	- Type:        Oracle
	- Connection Settings
	  - Hostname:   10.0.0.4
	  - Port:       1521
	  - Username:   DIPC_SRC
	  - Password:   welcome1
	  - Service:    Service Name: dics12c


	![](images/300/AgentImage105-EnterSrcConnectionInfo.png)

-   Click "Test Connection" and "Save"

	![](images/300/AgentImage105-TestSaveSrcConnection.png)

-   Search for source connection ONPREM_SRC in catalog

	![](images/300/AgentImage110-SearchConnectionCatalog.png)

    ![](images/300/AgentImage111-ViewSrcConnectionCatalog.png)

-   Click connection name to view summary

    ![](images/300/AgentImage112-ViewSrcConnectionSummary.png)

-   Click metadata tab

    ![](images/300/AgentImage113-ViewSrcConnectionMetadata.png)


### **STEP 14**: [OnPremiseVM] Create blank target Schema ONPREM_TRG

-   Create schema and ensure necessary privileges
-   SQL> create user onprem_trg identified by welcome1;
-   SQL> grant connect, resource, unlimited tablespace to onprem_trg;
-   SQL> grant create database link to onprem_trg;
-   
-   View schema tables and row count - should be empty
-   SQL> connect onprem_trg
-   SQL> select table_name from user_tables;

	![](images/300/AgentImage115-OnpremTrgSchema.png)


### **STEP 15**: [DIPC 18.2.3] Create Target Connection to Schema ONPREM_TRG

-   Enter target connection information to schema ONPREM_TRG
-   Use same remote agent on port 7010 for target connection

    - Name:        ONPREM_TRG
    - Description: Connection to target schema onprem_trg
	- Agent:       localhost:7010
	- Type:        Oracle
	- Connection Settings
	  - Hostname:   localhost
	  - Port:       1521
	  - Username:   ONPREM_TRG
	  - Password:   welcome1
	  - Service:    Service Name: dics12c

-   Click "Test Connection"

	![](images/300/AgentImage120-CreateOnpremTargetConnection.png)

-   Click "Save" to save target connection

	![](images/300/AgentImage121-TestSaveTargetConnection.png)

-   View Target Connection

	![](images/300/AgentImage122-ViewTargetConnection.png)

### **STEP 16**: [DIPC 18.2.3] Create Sync Job between Source and Target

-   In DIPC Console click "Home" in left toolbar
-   Click "Synchronize Data"

	![](images/300/AgentImage125-CreateSyncJob.png)

-   Enter Sync Job Information
    - General Information
	  - Name:        SYNC ONPREM SCHEMA
	  - Description: Job to sync on-prem schema with cloud target schema
	- Source Configuration
	  - Connection: ONPREM_SRC
	  - Schema:     DIPC_SRC
	- Target Configuration
	  - Connection: ONPREM_TRG
	  - Schema: ONPREM_TRG
	- Advanced Options
	  - Include Initial Load: check for initial load
	  - Include Replication: check for replication

-   Click "Save & Run"

	![](images/300/AgentImage125-EnterSynJobInfo.png)

-   Click Job to review Sync Job Details

	![](images/300/AgentImage130-SyncJobMonitor.png)

-   Monitor Target Schema for table creation and row count
AgentImage135-SyncJobMonitorSchema.png

	![](images/300/AgentImage135-SyncJobMonitorSchema.png)

	![](images/300/AgentImage136-SyncJobMonitorSchema2.png)

	![](images/300/AgentImage137-SyncJobMonitorSchema3.png)

