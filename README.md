# mft-sample-web-ui
This repository contains a sample web page which uses JavaScript to interact with the 
IBM MQ Managed File Transfer administrative REST API added in MQ 9.0.5. Support has 
been added to drive all the major capabilities of the REST API available at the 
MQ 9.0.5 level.


## Instructions:

In order to use this sample you will need to perform the following steps:

1. Install MQ 9.0.5 on your favourite platform, remembering to include the Web component.

2. Configure the mqweb server so that users can log in to the REST API. Documentation on
how to do this is here: 
https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.con.doc/q128300_.htm

3. Install the provided agentstatus.html, transferstatus.html and mftrest.js files in a 
suitable web serving environment. The approach used for this will vary depending on 
environment. For example, if using WebSphere Liberty Profile (WLP) then you can simply 
run the following command:

jar -cvf mftwebui.war agentstatus.html transferstatus.html mftrest.js

and copy the resulting mftwebui.war to the dropins directory of your server. Note
that the WLP install provided with MQ is not licensed for running user code.

4. Add the origin hosting the mftwebui.war file to the Cross Origin Resource Sharing (CORS)
configuration in the mqweb server. More information on how to do that is here:
https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.sec.doc/q128790_.htm

5. Configure an IBM MQ queue manager as coordination queue manager. The queue manager must be
running on the same machine as MQ Web Server because MQ Web server connects to coordination
queue manager in bindings mode.

6. Ensure mqwebuser.xml file contains the following roles of MFT REST API. For example:
   <enterpriseApplication id="com.ibm.mq.rest">
       <application-bnd>
         <security-role name="MFTWebAdmin">
                <group name="MQWebAdminGroup" realm="defaultRealm"/>
            </security-role>
        </application-bnd>
    </enterpriseApplication>

7. Add the following variables to mqwebuser.xml file
	<!-- Enable MFT REST Service /-->
	<variable name="mqRestMftEnabled" value="true"/> 
	<!-- Replace with your MFT Coordination queue manager /-->
	<variable name="mqRestMftCoordinationQmgr" value="COORDINATION QMGR"/> 
	<!-- Default reconnect time out is 5 minutes.  /-->
	<variable name="mqRestMftReconnectTimeoutInMinutes" value="5"/> 

8. Restart MQ web server using strmqweb command.

9. Access the sample webui using your browser. For example for running the agent status API in WLP, the URL would 
be something like: https://localhost:9444/mft/agentstatus, where sample application is installed under 'mft' folder. 

## Some trouble shooting hints:

* If you can bring up the sample webui but get a message about a CORS error then review
the CORS configuration in step 4 above.

* You don't have to host the sample application in a web server, you can load it
directly from the file system. However your requests will get rejected by your web browser
due to you CORS configuration. To rectify this either use a web-server or disable security
in the mqweb server. Disabling security can be achieved by using the no_security sample
described here: 

https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.sec.doc/q127970_.htm


## Problems:

If you have any problems with this sample please create an Issue and let us know.

## License:
Please read the accompanying license.txt file prior to use.