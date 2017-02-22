# Barnes wear

## Android app
The android app has 2 flavors (this only changes the server url):

| Flavor        | Description                                         |
| ------------- | --------------------------------------------------- |
| Staging       | Used when experimenting new stuff in a stage server |
| Production    | Used to build production apps                       |

and 2 build types: 

| Build type    | Crash report  | Print logcat | Debuggable |
| ------------- | ------------- | ------------ | ---------- |
| Debug         | No            | Yes          | Yes        |
| Release       | Yes           | No           | No         |

To build the debug version run the following command:
```sh
$ ./gradlew assemble[flavor][buildtype]
```
For example, to build the production & release version run:
```sh
$ ./gradlew assembleProductionRelease
```

Also remember to edit the following info:

| File                              | Variable                                                      | Description                                                      |
| --------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------- |
| `app/build.gradle`                | `signingConfigs.debug`                                        | Key used to sign the debug version                               |
| `app/build.gradle`                | `signingConfigs.release`                                      | Key used to sign the release version                             |
| `app/build.gradle`                | `productFlavors.staging.buildConfigField SERVER_BASE_URL`     | Staging salesforce server URL                                    |
| `app/build.gradle`                | `productFlavors.production.buildConfigField SERVER_BASE_URL`  | Production salesforce server URL                                 |
| `AndroidManifest.xml`             | `Application meta-data io.fabric.ApiKey`                      | Fabric.io key used to setup Crashlytics                          |
| `src/utils/BuildConstants.java`   | `CLIENT_ID`                                                   | Salesforce client id                                             |
| `src/utils/BuildConstants.java`   | `CLIENT_SECRET`                                               | Salesforce client secret                                         |
| `src/utils/BuildConstants.java`   | `USER_NAME`                                                   | Salesforce user name                                             |
| `src/utils/BuildConstants.java`   | `USER_SECRET`                                                 | Salesforce user secret/password                                  |
| `src/utils/ApplicationData.java`  | `RANGING_ID`                                                  | A unique id used to start/stop beacons Ranging                   |
| `src/utils/ApplicationData.java`  | `BEACONS_UUID`                                                | The most significant id used to find the beacons (Ranging mode)  |

The login flow is detailed at https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_username_password_oauth_flow.htm


## Salesforce setup

### Instructions

* General Instructions: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_HelloWorld.htm
* Create a custom object: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_customobject.htm
* Create an APEX class: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_class.htm
* Create an APEX trigger: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_trigger.htm
* Create test class: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_test.htm

### Custom objects








#### Device
```
Singular Label: Device	
Plural Label:   Devices	
Object Name:    Device
API Name:       Device__c
```

| Field Label           | API Name              | Field Name         | Data Type                             | Indexed |
| --------------------- | --------------------- | ------------------ | ------------------------------------- | ------- |
| **Standard fields**                                                                                                  |
| Created By            |                       |CreatedBy           | Lookup(User)                          | No      |
| Last Modified By      |                       |LastModifiedBy      | Lookup(User)                          | No      |
| Owner                 |                       |Owner               | Lookup(User,Queue)                    | Yes     |
| Salesforce Id         |                       |Name                | Auto Number                           | Yes     |
| **Custom   fields**                                                                                                  |
| Unique Id             | Unique_Id__c          |                    | Text(255) (Unique Case Sensitive)     | Yes     |
| User                  | User__c               |                    | Lookup(Device User)                   | Yes     |








#### Device User
```
Singular Label: Device User
Plural Label:   Device Users
Object Name:    Device_User
API Name:       Device_User__c
```

| Field Label           | API Name              | Field Name         | Data Type                             | Indexed |
| --------------------- | --------------------- | ------------------ | ------------------------------------- | ------- |
| **Standard fields**                                                                                                  |
| Created By            |                       |CreatedBy           | Lookup(User)                          | No      |
| Last Modified By      |                       |LastModifiedBy      | Lookup(User)                          | No      |
| Owner                 |                       |Owner               | Lookup(User,Queue)                    | Yes     |
| User Name             |                       |Name                | Text(80)                              | Yes     |
| **Custom   fields**                                                                                                  |
| Email                 | Email__c              |                    | Email(Unique)                         | Yes     |





#### Item
```
Singular Label: Item
Plural Label:   Items
Object Name:    Item
API Name:       Item__c
```

| Field Label           | API Name              | Field Name         | Data Type                             | Indexed |
| --------------------- | --------------------- | ------------------ | ------------------------------------- | ------- |
| **Standard fields**                                                                                                  |
| Created By            |                       |CreatedBy           | Lookup(User)                          | No      |
| Last Modified By      |                       |LastModifiedBy      | Lookup(User)                          | No      |
| Owner                 |                       |Owner               | Lookup(User,Queue)                    | Yes     |
| Item Name             |                       |Name                | Text(80)                              | Yes     |
| **Custom   fields**                                                                                                  |
| Image URL             | Image_URL__c          |                    | URL(255)                              | No      |
| Item Id               | Item_Id__c            |                    | Text(50) (Unique Case Sensitive)      | Yes     |
| Push Text             | Push_Text__c          |                    | Long Text Area(500)                   | No     |
| Region                | Region__c             |                    | Lookup(Region)                        | Yes     |










#### Region
```
Singular Label: Region
Plural Label:   Regions
Object Name:    Region
API Name:       Region__c
```

| Field Label           | API Name              | Field Name         | Data Type                             | Indexed |
| --------------------- | --------------------- | ------------------ | ------------------------------------- | ------- |
| **Standard fields**                                                                                                  |
| Created By            |                       |CreatedBy           | Lookup(User)                          | No      |
| Last Modified By      |                       |LastModifiedBy      | Lookup(User)                          | No      |
| Owner                 |                       |Owner               | Lookup(User,Queue)                    | Yes     |
| Region Name           |                       |Name                | Text(80)                              | Yes     |
| **Custom   fields**                                                                                                  |
| Beacon UUIDs          | Beacon_UUIDs__c       |                    | Text(255)                             | No      |
| Description           | Description__c        |                    | Text(255)                             | No      |









#### Saved Item
```
Singular Label: Saved Item
Plural Label:   Saved Items
Object Name:    Saved_item
API Name:       Saved_item__c
```

| Field Label           | API Name              | Field Name         | Data Type                             | Indexed |
| --------------------- | --------------------- | ------------------ | ------------------------------------- | ------- |
| **Standard fields**                                                                                                  |
| Created By            |                       |CreatedBy           | Lookup(User)                          | No      |
| Last Modified By      |                       |LastModifiedBy      | Lookup(User)                          | No      |
| Owner                 |                       |Owner               | Lookup(User,Queue)                    | Yes     |
| Saved item Name       |                       |Name                | Text(80)                              | Yes     |
| **Custom   fields**                                                                                                  |
| Device User           | Device_User__c        |                    | Lookup(Device User)                   | Yes     |
| Item                  | Item__c               |                    | Lookup(Item)                          | Yes     |
| Unique Combination    | Unique_Combination__c |                    | Text(255) (Unique Case Sensitive)     | Yes     |

It is also necessary to create a field update/workflow to update the `Unique Combination` field to ensure that we won't have duplicated entries with the user/item combination:

```
Field Update

Name:                                            Saved Item Unique	 	 
Unique Name:                                     Saved_Item_Unique	 	 
Object:                                          Saved Item	 	 
Field to Update:                                 Saved Item: Unique Combination
Field Data Type:                                 Text
Re-evaluate Workflow Rules after Field Change:   Yes
Formula Value:                                   Device_User__c + '--' + Item__c
```



```
Workflow Rule

Rule Name:             Saved Item Unique
Object:                Saved Item
Evaluation Criteria:   Evaluate the rule when a record is created, and every time it's edited
Rule Criteria:         Saved Item: Saved item Name NOT EQUAL TO null
Workflow Actions:      Immediate Workflow Actions
                           Type:          Description
                           Field Update:  Saved Item Unique
```



### Custom classes

#### DeviceRestResource
This class creates a REST endpoint to register the android devices into salesforce. The endpoint will return an id that can be used later to create objects and to indentfy which device has created them.
```
@RestResource(urlMapping='/Device/*')
global with sharing class DeviceRestResource {

@HttpPost   
  global static SimpleDevice createNewCase(String localDeviceId) {
     List<Device__c> devices = [SELECT Name FROM Device__c WHERE Unique_Id__c = :localDeviceId LIMIT 1];
     if(devices == null || devices.size() == 0) {
        Device__c device = new Device__c(Unique_Id__c = localDeviceId);
        insert device;
        devices = [SELECT Name FROM Device__c WHERE Id = :device.Id LIMIT 1];
     }
      return new SimpleDevice(localDeviceId, devices.get(0).Name);
  }
  
    global with sharing class SimpleDevice {
       public String localDeviceId;
       public String remoteDeviceId;
       
       public SimpleDevice(String localDeviceId, String remoteDeviceId){
          this.localDeviceId = localDeviceId;
          this.remoteDeviceId = remoteDeviceId;
       }
    }
}
```

### SavedItemRestResource
The mobile app has a 'Save for later' action which will call this endpoint to save the item and the user. This is used to indicate that the user expressed an interest in a particular item. 
```
@RestResource(urlMapping='/SavedItem/*')
global with sharing class SavedItemRestResource {

    @HttpPost   
    global static SimpleSavedItem createNewCase(String itemId, String deviceId) {
        RestResponse res = RestContext.response;        
        List<Device__c> deviceList = [select User__c from Device__c where Name = :deviceId LIMIT 1];
        List<Item__c> itemList = [select Id from Item__c where Id = :itemId LIMIT 1];
        SimpleSavedItem simpleSavedItem;
        if(deviceList != null && deviceList.size() > 0 && itemList != null && itemList.size() > 0 ) {
            Saved_item__c savedItem = new Saved_item__c(Device_User__c = deviceList.get(0).User__c, Item__c = itemId);
            simpleSavedItem = new SimpleSavedItem(savedItem);
            try {
                insert savedItem;
            } catch (DMLException e) {
            }
        } else {
            res.statusCode = 403;
        }
        return simpleSavedItem;
    }
  
    global with sharing class SimpleSavedItem {
       public Id itemId;
       public Id userId;
       
       public SimpleSavedItem(Saved_item__c savedItem){
          itemId = savedItem.Item__c;
          userId = savedItem.Device_User__c;
       }
    }
}
```

### ItemRestResource
Endpoint to retrieve a list of items associated with a particular beacon/region.
```
@RestResource(urlMapping='/Item/*')
global with sharing class ItemRestResource {
    @HttpGet
    global static List<SimpleItem> getItems() {
        RestRequest req = RestContext.request;
        RestResponse res = RestContext.response;
        String uuid = req.params.get('uuid');
        String uuidQuery = uuid == null ? '%%' : '%('+uuid+')%';
        
        List<SimpleItem> result = new List<SimpleItem>();
        List<Item__c> originalItems =
            [SELECT Name, Item_Id__c, Image_URL__c, Push_Text__c, Region__c
            FROM Item__c
            WHERE Item_ID__c != null AND Region__c in (SELECT Id FROM Region__c WHERE Beacon_UUIDs__c LIKE :uuidQuery)];
        for(Item__c current : originalItems ){
            result.add(new SimpleItem(current));
        }
        return result;
    }
    
    global with sharing class SimpleItem {
       public Id regionId;
       public Id id;
       public String name;
       public String imageUrl;
       public String description;
       
       public SimpleItem(Item__c originalItem){
          id = originalItem.Id;
          name= originalItem.Name;
          imageUrl= originalItem.Image_URL__c;
          description= originalItem.Push_Text__c;
          regionId = originalItem.Region__c;
          updateImageUrl();
       }
       
       private void updateImageUrl() {
          if (imageUrl == null) {
            List<ContentDocumentLink> links = [SELECT ContentDocumentId, ContentDocument.LatestPublishedVersionId FROM ContentDocumentLink WHERE LinkedEntityId = :id];
            if (links != null && links.size() > 0){
                String contentDocumentId = links.get(0).ContentDocumentId;
                String latestPublishedVersionId = links.get(0).ContentDocument.LatestPublishedVersionId;
                
                List<ContentDistribution> contentList = [
                    SELECT ContentDocumentId, ContentVersionId, DistributionPublicUrl
                    FROM ContentDistribution
                    WHERE ContentDocumentId = :contentDocumentId
                ];
                
                if (contentList != null && contentList.size() > 0){
                    ContentDistribution firstContent = contentList.get(0);
                    String host = URL.getSalesforceBaseURL().getHost().split('\\.')[0];
                    String realUrl = 'https://c.' + host + '.content.force.com/sfc/dist/version/download?';
                    realUrl += 'ids=' + latestPublishedVersionId;
                    realUrl += '&oid=' + UserInfo.getOrganizationId();
                    realUrl += '&d=' + firstContent.DistributionPublicUrl.substring(firstContent.DistributionPublicUrl.indexOf('/a/'));
                    imageUrl = realUrl;
                }
            }
          }
       }
    }
}
```
