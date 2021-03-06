# RaptorBox

Raptorbox is an open-source solution for Rapid Prototyping of application for the Internet of Things or IoT

## Overview
Raptor is a platform offering a set of tools to easily and quickly connect your devices, making data they generate accessible from the cloud and offering mechanisms to interact with them remotely in order to build your connected solutions and applications.

![Features](https://github.com/raptorbox/raptor-docs/blob/master/RaptorFeatures.png?raw=true) 

#### Raptor offers you specific tools to:
1. **configure your device** allowing it to communicate and interact with Raptor cloud services in a secure way
2. **manage data flows** produced by your devices and handle data storage, allow applications to subscribe for notifications about data changes and offer capabilities to perform actuation on your devices
3. **build & deploy rapidly** your applications enjoying rich tools, open restful APIs and libraries aiming at simplifying the way in which you build your next IoT application


## Architecture
The following picture depicts an high level view of Raptor architecture.
![Architecture](https://github.com/raptorbox/raptor-docs/blob/master/Raptor-Arch.png?raw=true  "Architecture")

As a rapid prototyping platform, Raptor offers you a graphical web development environment and user interface simplifying the process of configuring and programming your devices in order to allow them to communicate with the cloud and (when needed) make them accessible remotely.
##### Virtual Devices
Logically each device you want to include in your applications is represented inside Raptor as a “virtual device”, i.e. the virtual representation of your device available for identification and interaction inside your applications.
##### Data storage
When your device produce new data (such as, for example, samples of the temperature in your room) it will use Raptor restful APIs in order to push this data into the cloud.
Data acquired by Raptor cloud will be automatically associated to the virtual representation of your device, stored into a scalable data store and made available for future usage and retrieval, business logic associated to your device in the cloud (i.e. subscriptions of applications interested in this data, workflows associated to your data flows) is automatically triggered and executed.

1. Logically each device you want to include in your applications is represented inside Raptor as a “virtual device”, i.e. the virtual representation of your device available for identification and interaction inside your applications.
2. When your device produce new data (such as, for example, samples of the temperature in your room) it will use Raptor restful APIs in order to push this data into the cloud. Data acquired by Raptor cloud will be automatically associated to the virtual representation of your device, stored into a scalable data store and so made available for future usage and retrieval, business logic associated to your device in the cloud (i.e. subscriptions of applications interested in this data, workflows associated to your data flows) is automatically triggered and executed.
*Note: Definition and creation of your devices (of their virtual representation) can be done using Raptor graphical editor or programmatically via Raptor APIs (typically, the user interface is used for rapid prototyping and testing your solutions, while APIs are used to handle device provisioning according to your specific deployment needs in a programmatic way).*

#### The real-time Dashboard
Raptor offers indeed a monitoring environment (the Raptor dashboard) where you can check the status of your connected devices and verify acquired data through historical data visualizations and queries.
Once your devices are connected with the cloud and their data flows managed by Raptor cloud, it’s time to focus on creation of your application starting from the definition of its business logic. As for device creation, Raptor offer two possible ways that you can use in order to implement your application logic:
1. use Raptor restful APIs to interact with devices and their data and build your application logic externally (using your preferred programming language and execution environment, such as, for example a java or PHP application, etc.)
2. use Raptor Workflow Visual Editor to quickly define, test and verify your application logic combining data flows with external services, data sources, events

#### Workflow Visual Editor
Raptor Workflow Visual Editor is mainly aimed to support fast prototyping due to the nature of its capabilities: starting form data flows produced by your devices, the editor allow the creation of a workflow in a graphical way, according to a drag & drop paradigm, reusing existing processing capabilities configured inside existing nodes (selected from a palette of predefined nodes) offering specific functionalities to transform your data flows, combine them this other external and internal data sources, executing actions (such as, for example, actuations on your devices or on external services, such as, for example, twitting a message).

As a rapid prototyping tool, the visual editor allows users to easily customize application logic predefined inside nodes made available by the editor (using javascript language) in order to quickly adapt the logic to your specific needs and objectives, test it and verify on the fly its behaviour).
Raptor Workflow Visual Editor is based on a well known open source solution Node Red that has been customized and adapted to this specific context.


## RaptorBox in Detail

The following picture depicts a detailed view of RaptorBox (Raptor / Raptor Cloud) architecture.

![Raptorbox](https://github.com/raptorbox/raptor-docs/blob/master/Raptorbox.png?raw=true  "RaptorBox Architecture")

RaptorBox as a rapid prototyping platform is a Microservice Architecture. All the components are designed to work independently from other. As security is a big concern in IoT world so the Authentication module is specially designed to keep data secure and provide all the components the authentication to communicate securely. 

RaptorBox is designed to be a resource friendly so it can be used on simple and normal machine with less processing power and RAM. Therefore all of its components are dockerized which can be used either on same machine or can be distributed on different machine and connected together over tcp. 

### API Gateway

API Gateway is an entry point to Raptor Cloud / RaptorBox from any application whether it is a Raptor's provided UI or any 3rd party application. API gateway uses [NGNIX](https://www.nginx.com/) NGNIX underneath. NGINX is open source software for web serving, reverse proxying, caching and more. 

API gateway gives access to the devices and its data and apps etc. All the components (app, devices etc) use this gateway (web server) to exchange information between each other. Every request that comes from client (website or mobile or 3rd party) first get authenticated from Auth component of Raptor and then responded with the required data or response. All of this authorization uses the same gateway before responding with the required action. 

### MQTT Broker

Raptor provides an other interface other than the APIs to get updates about different events in platform for example device data and stream. This counterpart to APIs is a MQTT (Message Queuing Telemetry Transport) broker, which is the heart of any publish/subscribe protocol. Depending on its core implementation, a broker can handle up to thousands of concurrently connected MQTT clients. For more details see [mqtt.org](http://mqtt.org/) mqtt.org

In MQTT, the connected devices need a protocol using which they could communicate only when it is required. As security is one of the core concern in IoT infrastructure, the authentication (explained below) made compulsory to communicate with the broker. 

A valid user of platform can subscribe the topic in order to receive the updates about his devices (already added and connected devices) using the following:

````bash
url: 		v5.raptorbox.eu
port:		1883
username:	<valid username> // authentication required to subscribe the topic
password:	<valid password>
topic:	 	<device id>/streams/<stream name>/updates
````

Detailed information about connection to MQTT, payload, and response etc can be found [here](https://docs.raptorbox.eu/pages/documentation/api-docs/v5/mqtt.html) here.

### Authentication (Auth)
Raptor supports user login and token based authentication in order to handle authentication and authorization to access and use the Raptor APIs.

Users will be able to get credentials for the various entities needing to access specific platform capabilities via Raptor API: credentials are in the form of access tokens, called API Keys, that can be generated and obtained though Raptor user interface.

The following picture explains relationships among users, devices and API keys handled by Raptor.
![](https://github.com/raptorbox/raptor-docs/blob/master/API_Keys.png?raw=true) 

##### Token
A User can have many Token which can be generated both from the frontend and backend and used in the code.
Tokens can be generated, disabled and deleted, affecting immediately the device or code using that key.

##### Role
Roles contains one or more permissions, allowing or denying an user with that role to perform an operation
A User can have many roles.

##### Permissions
Permissions allow to have a fine-grained control over what an user can do within the platform
For example a *web application* may have a PULL-only permission to access the data of a device. The device itself instead will have **PUSH** permission to the data stream API in order to store the data from sensors.
Permission are plain labels that follow a structure:
*read_own_device* is composed of three informations:
First read is the permission. Core permission recognized by the platform are
1. *read*
2. *create*
3. *update*
4. *delete*
5. *push* & *pull* Reference respectively to send and read data for a device
6. *execute* Is the permission to execute a command on a device
7. *admin* is a special flag that means all the permission
Second is an optional flag to define ownership (own). This will apply the permission only to the subjects an user created and is considered owner.
An example is admin_own_device which means users can manage devices created by them but no access those created by others.
Instead admin_device will allow users to manage all the devices in the platform.
Third part is the subject type, in this case device. Core types in the platform are
1. *device* A device instance modeled in the platform
2. *user* An user
3. *stream* A collection of data
4. *token* A token used to query the API
5. *client* An oAuth2 client to delegate access
This permission model allow for a certain degree of flexibility in describing permissions on premise. When a more specialized access control is required, the permission API allow to set a specific set of permission of an user on a type instance.
Refer to the Permission API on how to change permissions and delegate access to users

#### Tokens
Tokens are used in place of username/password to identify the acting user during the interaction with the platform.

HTTP requests: During an HTTP request the token may be prefixed with the Bearer keyword and inserted in the Authorization header.

````bash
GET / HTTP/1.1
Host: raptor.local
Accept: application/json
Authorization: Bearer <token>
````

### App

Imagine you have different devices that are being used for entirely different purposes. For example you have 10 devices that gives you data about your fields and 5 other devices to keep track of your health. Keeping all the devices without application will make it little difficult when you tries to see the data of your smartband for your health. With having the application context, you will be able to easily differentiate the devices.

With the newer version of Raptor, Raptor introduce the application concept. Application works like a group where you can add different valid users and assign them different roles to perform different actions. You can add devices to the application and valid users of application will be able to perform specific operations on these devices specified by their roles. 

#### App - Data Model

Imagine the above example. We need to create and manage the app related to fields that will have some valid users who can perform some task and a bunch of devices that will send the data about the field's condition. A user will create an application in Raptor platform's will have following properties:

````bash
{
    "name": "Health",
    "description": "Health tracking Devices",
    "roles": [
	    {
	      "name": "admin",
	      "permissions": [
		"admin"
	      ]
	    },
	    {
	      "name": "viewer",
	      "permissions": [
		"read_device"
	      ]
	    }
    ],
    "users": [
	    {
	      "roles": [
		"admin"
	      ],
	      "id": "user_id"
	    }
    ]
}
````

1. ````name```` is required and is used to identify the device.
2. ````description```` is a textual description of the device to help users to identify the device.
3. ````roles```` is an array of objects which are key / values properties where name is the name of the role like *admin* and *viewer* and permission is an other array of objects that consists of permission like *admin* (who can do anything in app like an owner) and *read_device* (who can read only devices but cannot update, delete or create new devices). More details about permissions can be seen [here](https://docs.raptorbox.eu/pages/overview/authentication.html) here.
4. ````users```` is an array of objects made of users and their specific roles within the application

#### How it works:

A valid user can create an application.  As a user created an application he/she is the owner of this application. He can add other already registered users and assign them specific roles within that application. These users will then be able to perform opetions specified by their roles. A user then be able to register devices in the application or can add already registered devices to the application. This will group everything in one unit that will help the user to keep track of devices and users easily. The user will be able to distinguish devices and data of his/her or his/her family's health and fields as mentioned in the above exmaple. 

Detailed overview of operation on app can be viewed here[here](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/raptorbox/raptorbox.github.io/v5/swagger/api/raptor-application/swagger.json).

### Inventory

Inventory provides the detailed information and functionality about the devices connected to Raptor. A well-defined data model for the devices and its stream or data is implemented to support different type of devices. How specific devices will be store data or what are the properties of it and what possible intraction you can have, everything is defined and provided under inventory. 

#### Device - Data Model

Imagine we want to control a robot with Raptor. We need to identify some properties of this robot and store it in Raptor by describing these properties of robot. We will register it in  Raptor platform's understandable way that is:

````bash
{
    "name": "Robot",
    "description": "iRobot - Raptor",
    "properties": {
        "model": "robot-007",
        "batteryLevel":"90%"
    }
}
````

1. ````name```` is required and is used to identify the device.
2. ````description```` is a textual description of the device to help users to identify the device.
3. ````properties```` is an object of key / values properties which may be useful to define further details of the device.

#### How it works:

When a valid user tries to register a device in Raptor, role and permissions of the user is being checked to give security to the device. If the user has authority to perform this operation, a device of valid model processed and added to Raptor database. This newly valid device is assigned to the user automatically with full authority over the device. If some specific permissions are given to user defined in Application[Application](https://docs.raptorbox.eu/pages/overview/data-model.html) context, the user then will be able to perform those operations.

Detailed overview of operation on valid devices can be viewed [here](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/raptorbox/raptorbox.github.io/v5/swagger/api/raptor-inventory/swagger.json) here.

### Stream

After registering devices into the system, the next important thing is the data that is sent by devices. Data that is sent by devices saved in the Raptor but can have different type of raw data. This data can be retrieved by or viewed in Raptor provided GUI by valid user. In Raptor, Stream word is used to group the data that is send by devices at one time. It depends on the user which kind of data he/she wants from the device. The data can either be a dynamic data or predefined and can either be form different sensors or just one sensor in the device. In all cases the specific data is received by Raptor and sotred accordingly that can be viewed by the valid authrozied users. 

#### Stream - Data Model

Imagine our robot within Raptor environment is sending us data. We need to view the data through Raptor provided APIs. Upon requesting the data from Raptor, we will receive following:

````bash
{
  "offset": 0,
  "limit": 0,
  "sortBy": {
    "direction": "string",
    "fields": [
      "string"
    ]
  },
  "channels": {
    "movearm": {
      "arm": "right",
      "degree": 70
    },
    "takestep": {
      "direction": "east"
    },
    "water": {
      "on": false
    }
  },
  "location": {
    ...
  }
}
````

#### How it works:

When a valid user register a device in Raptor, the device data (stream) is started to store in Raptor. If the user has authority to view the stream data, he/she can view it by logging in Raptor's GUI or can access it by consuming the APIs.

Detailed overview of APIs of stream can be viewed [here](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/raptorbox/raptorbox.github.io/v5/swagger/api/raptor-stream/swagger.json).

### Tree

Imagine we have installed devices in our fields (example mentioned above). Some of them are for watering and some of them are providing us with field's soil conditions and 2-3 are for weather condition etc. Let's say we have to water the fields. In the normal way, we will look for all the devices and turn on water functionality for the specific devices but we have to scan all the devices. If we can make a cluster / group of those similiar valid devices then our job will become easy. This group is our **tree**.

Tree helps us in making a virtual tree in Raptor to cluster all the similiar valid devices in one place. In this cluster kind of tree, we will have one root (water device) and its children (all devices who shares the same functionality). This will solve our problem mentioned above and by using the Action API of Raptor we will be able to turn on water on all the devices within out fields.

#### Tree - Data Model

````bash
{
    "name": "Watering Devices",
    "type": "water",
    "parentId": "device Id",
    "children": [
      null
    ]
  }
````
1. ````name```` is required and is used to identify the tree.
2. ````type```` is a string to help users to distinguish the trees.
3. ````parentId```` is a device id of root / parent device which is used to get its children. It is usually a virtual device without any connected physical existence.
4. ````children```` is the list of all its children.

#### How it works:

A valid user can create a root device (a virtual device without any real connected device) only if he is authorised to create tree. The user can then add ids of child devices if and only if he/she owns or have authority over the devices. 

Detailed overview of APIs of tree can be viewed [here](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/raptorbox/raptorbox.github.io/v5/swagger/api/raptor-tree/swagger.json#/).

### Action

Imagine we want our robot to turn on water pump for our fields. How will we do that being in Raptor cloud. We can register a new device, we can receive the data from device but this is still a half way communication. Action provides us the functionality to send data to our valid devices. 

Take the above example, we need to send water on signal to our sensor so we can water our fields remote. We first have to connect the device and then to send signal or command to perform a specific task. Within Raptor cloud, we need to provide a specific model to communicate with device. As every other IoT device hace specific way to process commands or signal, Raptor allows the users to integrate their own functionality and device specific model that will be understanable by the device.

#### Action - Data Model

Our robot, as mentioned above, needs to turn on the water pump for the fields. As this functionality is specific for every IoT device, we have implemented one as an example to communicate with our robot. As mentioned in the model object of Stream, our robot sends us data about move, step and water. We here write the data on the water channel of Stream object that take a true/false parameter that will make our robot to perform the task. We will send the data to our robot through Raptor following the specific model that will let our robot to perform task:

````bash
URL:	/action/{deviceId}/{actionId}
{
  "on": true
}
````

*Note:* This payload model can be different according to sensor or device.

#### How it works:

A valid user can send the command to the device in Raptor that has already specified communication channel. If the device needs some specific way of communication, a user must have that integrated with Raptor cloud. If this specific way of communication is availale, a valid user with his/her valid permissions can send the commands and actions to the device. 

Detailed overview of APIs of stream can be viewed [here](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/raptorbox/raptorbox.github.io/v5/swagger/api/raptor-action/swagger.json).

### Database and Cache

Database is the most important part in software architecture to keep all the data. Raptor is fully supported to **MongoDB** but fexlible to be used with other databases. 

Cache in large scale platforms helps to make access to data fast. Raptor is equiped with state of the art caching system based on **Redis** that makes the data access for user fast.

As the RaptorBox is dockerized for all the components, a developer can provide his/her own docker container for MongoDB and redis other than the default one and can secure it in his/her own ways. 

### UI

A platform is incomplete without its GUI and user (or developer) cannot find the true meaning and functionality of it unless he/she tries it. RaptorBox is provided with GUI that gives user the last and most important part to complete a package. 

This GUI is design in state of the art manner that provides most of the functionalities of the platform to make a complete package that a user can use to control his/her devices and perform different type of actions over them. It is designed to give a better view of data visualisation with real time (live) updates from the devices. User can view his/her applications with the users and their roles and can find his devices easily. 

The following APIs are fully working with the current version of UI:

1. App
2. Stream
3. Action
4. Inventory
5. Auth
6. Profile (User profile)

UI is fully open source based on Vue.JS with the CoreUI theme. A developer can download it and deploy it without thinking about the data visualization softwares. And if user/developer dont like it he/she can create a new UI and use it with the RaptorBox easily.

The online version of RaptorBox can be checked [here](https://v5.raptorbox.eu/#/). 
