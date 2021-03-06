<properties date="2016-06-24"
SortOrder="12"
/>

The PageBuilder uses DataHandlers for data-binding. DataHandler is the layer between the GUI and the Web Service. The handlers expose the web service results (the carriers) to the GUI, and take care of sending the modified results back to the Web services when needed.

For example the below section shows the SoContactPage.config file. This file shows the datahandlers  and the panels which are used. Here we can consider the datahandler called ContactEntityDataHandler.

```
<page id="ContactPage">
  <data>
    <datahandlers>
      <!-- Some other code-->
      <datahandler id="ContactEntityDataHandler"
      type="ContactEntityDataHandler"></datahandler>
      <!-- Some other code-->
    </datahandlers>
  </data>
  <panels>
    <panel reference="Menu" />
    <panel reference="ButtonBar" />
    <panel reference="Navigator" />
    <panel reference="Contact" />
  </panels>
</page>
```

 

The ContactEntityDataHandler is called in the contact panel. The below code segment shows the SoContactPanel.config file.

```
<?xml version="1.0" encoding="utf-8"?>
<panel id="Contact" type="SplitterPanel" soprotocol="Contact" paneltype="Main" placeholderid="MainPlaceHolder">
  <caption>[SR_COMMON_CONTACT]: [current:contact_name]</caption>
  <cards>
    <card id="ContactMainCard" type="SoTabbedCard" placeholderid="leftpanel" cardtype="MainCard">
      <views>
        <!-- Some other code-->       
        <view id="more" type="SoView" soprotocol="udef" current="contact">
          <caption>[SR_MORE_CONTACT]</caption>
          <tooltip>More...</tooltip>
          <controlgroups>
              <!-- Data Handler-->       
            <controlgroup id="moreHeadergroup" type="SoControlGroup" position="absolute" top="5px" left="5px" right="20px">
              <controls>
                <control id="ContactMoreHeaderControl" type="ContactHeader">
                  <datasource>ContactEntityDataHandler.ContactEntity
                  </datasource>
                  <config>
                  </config>
                </control>
              </controls>
            </controlgroup>
            <!--End of Data Handler-->
            <!-- Some other code-->
          </controlgroups>
          <!-- Some other code-->
        </view>
        <!-- Some other code-->
      </views>
      <!-- Some other code-->
    </card>
    <!-- Some other card-->  
  </cards>
  <!-- Some other code-->
</panel>
```

 

ContactEntityDataHandler is used to call the ContactEntity from the web service. With the statement below, we create the object to contact based on the control used. This is how we can use the datasource tag in cofig file for databinding.

```
<datasource>ContactEntityDataHandler.ContactEntity</datasource>
```

 

There is another way to bind the data to the controls by using the datasourcename. In the above code sample within the “ContactArchives” card id, SoArchiveControl is used.

```
<control id="ContactPersonArchive" type="SoArchiveControl" width="100%" top="0px" left="0px" height="100%" position="absolute" >
```

 

The SoArchiveControl control id is ContactPersonArchive. Within this UserControl there is a special tag called &lt;config&gt;.The &lt;config&gt; tag below indicates to us from where the Archive data has been retrieved, i.e. the name of the provider used. An “archivecolumninfo-datasourcename” tag tells us about the datahandler that has been used to retrieve the data. Tags such as showheader, showtoolbar and default sort are related to how the retrieved out put should be displayed.

```
<config>
     <toolbar>
     <button caption="[SR_MB_ADD]" icon="images/addbutton.gif" iconhover="images/addbuttonhover.gif" onclick="javascript:Dialog.open('Person','person[dialog=stop].main[mode=edit;new=true]?person_id=0');" fieldright="C"/>
     <button caption="[SR_MB_DELETE]" icon="images/deletebutton.gif" iconhover="images/deletebutton.gif" linkhint="nav=deletePerson"  fieldright="D"/>
     </toolbar>
     <restrictions>
       <restriction name="contactId" operator="=" binding="current">contact</restriction>
     </restrictions>
     <providername>person</providername>
     <archivecolumninfo-datasourcename>ArchiveColumnConfigDataHandler.ContactPersonArchive</archivecolumninfo-datasourcename>
     <showheader>true</showheader>
     <showtoolbar>true</showtoolbar>
     <defaultsort>rank</defaultsort>
     <pagesize>50</pagesize>
     <current>person</current>
</config>
```

 

In the above code segment a tag called “archivecolumninfo-datasourcename” is included.

```
<archivecolumninfo-datasourcename> ArchiveColumnConfigDataHandler.ContactPersonArchive
</archivecolumninfo-datasourcename>
```

This tag tells us how the datahandler has been bound to the control to retrieve the data through ArchiveColumnConfigDataHandler.

The above code is related to the following area of the CRM.web.

<img src="../User%20controls%20Data%20Binding_files/image001.jpg" width="606" height="469" />

See Also:

[Data Handlers](IDataHandler.md)

Code segment from the SoObjectMapping.config file. The file registers the objects that is used by the application. Ie. in order for any object to be used, it should first be entered into the SoObjectMapping.config file.

```
<object type="UserControl" mappingname="ContactMainView" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Contact/ContactMainView.ascx"></object>
  <object type="UserControl" mappingname="Contact2PersonMainView" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Contact/Contact2PersonMainView.ascx"></object>
 
  <object type="UserControl" mappingname="ContactWWWView" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Contact/ContactMainView.ascx"></object>
  <object type="UserControl" mappingname="ContactCommonHeaderView" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Contact/ContactCommonHeaderView.ascx"></object>
  <object type="UserControl" mappingname="ContactHeader" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Contact/ContactHeader.ascx"></object>
 
  <object type="UserControl" mappingname="PersonDialogHeader" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Dialogs/Person/PersonDialogHeader.ascx"></object>
  <object type="UserControl" mappingname="PersonDialogMain" assemblyname="SuperOffice.CRMWeb" objectname="~/WebParts/Dialogs/Person/PersonDialogMain.ascx"></object>
 
```

XML configuration files are used to call the usercontrols in to the page by mapping the “objectname”. “objectname” and the “assemblyname” gives the path from where the user controls should be loaded. “mappingname” is the key to map the objects with the SoObjectMapping.config file.
