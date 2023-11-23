.. container::
   :name: print-first-page

Cires21 APIv2 documentation
===========================

The API functions used to access the Cires21 applications are
implemented with REST philosophy. This means that the same URI can be
used to retrieve the details of a resource or to change its properties.
It will depend in the method used to call the API. The resources
implemented to be accessed are:

-  `Devices <#control.device>`__
-  `Devices groups <#control.device.group>`__
-  `Channels <#control.channel>`__
-  `Encodings <#control.encoding>`__
-  `Encodings groups <#control.encoding.group>`__
-  `Publishings <#control.publishing>`__
-  `Publishings groups <#control.publishing.group>`__
-  `Publishings Player Info <#control.publishing.playerinfo>`__
-  `Broadcasts <#control.broadcast>`__
-  `Broadcasts Details <#control.broadcastdetail>`__
-  `Broadcast Details Qualities <#control.broadcastdetail.qualities>`__
-  `Broadcasts urls <#control.broadcast.url>`__
-  `TimeFrames Qualities <#control.timeframe.qualities>`__
-  `Geoblockings <#control.geoblocking>`__
-  `Playouts <#control.playout>`__
-  `Remote Folders <#control.remotefolder>`__
-  `Target folders <#control.targetfolder>`__
-  `Media Folder <#control.mediafolder>`__
-  `Recording <#recording.recording>`__
-  `Recording editor <#control.recording.editor>`__
-  `Tags <#recording.tag>`__
-  `Users <#control.user>`__
-  `Users groups <#control.user.group>`__
-  `Users profiles <#control.userprofile>`__
-  `Clients <#control.client>`__
-  System
-  `DrmWidevine <#system.drmwidevine>`__
-  `Images <#system.image>`__
-  `Tempfiles <#system.tempfile>`__
-  `Information <#system.info>`__
-  `Security <#security>`__

The URI will determine the access to the entire collection of the
resources or a particular resource in the collection identified by its
id. The API only allows to get the collection of the resource, meanwhile
it allows to get, add, modify or delete a particular resource.

The methods used in the API are GET, POST, PUT and DELETE. These are
their actions:

-  GET (collection). List the information of all resources in the
   collection.
-  GET (resource). Retrieve the information of the member of the
   collection.
-  POST (collection). Create a new entry in the resource collection.
-  PUT (resource). Replace the addressed member of the colletion, or if
   it does not exist, create it.
-  DELETE (resource). Delete the addressed resource of the collection.

The POST and PUT methods use the content body of the request to create
and modify the addressed member as a JSON object. The GET, POST and PUT
methods retrieves the same information in JSON plus the id of each
element requested. Some resources, like Configuration, represent system
objects and cannot be created or deleted.

All the functions return a defined JSON structure to specify the success
of the operation and, if requested, information of the resource. This
structure include a property success that define the result of the
operation. If it is true, a data property will retrieve an information
object (for resource members) or an array of information objects (for
collections). If the success is false, a code and a message will be
returned in the structure. In this case, also a data can be returned
with detailed information of the problem.

Responses
---------

All the functions return a defined JSON structure to specify the success
of the operation and, if requested, information of the resource. This
structure include a property ``success`` that define the result of the
operation. If it is ``true``, a ``data`` property will retrieve an
information object (for resource members) or an array of information
objects (for collections). If the ``success`` property is ``false``, a
``code`` and a ``message`` will be returned in the structure. In this
case, also a ``data`` can be returned with detailed information of the
problem.

Usually, the correct answer of an API call will be:

::

   {
       "success": true,
       "code": 0,
       "message": null,
       "data": [ ... ]
   }

In the other hand, a fail API call will response something like this:

::

   {
       "success": false,
       "code": "SYSf012",
       "message": "No data found.",
       "data": null
   }

Sometimes, if the request is invalid or has incorrect parameter, it will
return codes specifing the problem and additional data to clarify the
error:

::

   {
       "success": false,
       "code": "APIf001",
       "message": "The parameters are wrong for this API function. Please, check the API documentation.",
       "data": {
             "parameter": "username",
             "definition": "string"
       }
   }

Fields
------

Each field of the API functions has defined how the values must be used
in order to get the function work. This specification includes:

-  *Name*: The name of the field is the name that must be used in the
   requested json and usually auto-explain the meaning of this field.
-  *Required*: Specify if the field is mandatory or not in the funcion
   call.
-  *Type*: Define which value types can be used in this field. The
   possible types defined in the API will be explained in following
   subchapters.
-  *Extra info*: Depending the field type, this will explain more
   information about it.

Field types
~~~~~~~~~~~

string
^^^^^^

The field must include an string. This is the more flexible type and it
can include any information.

::

   "name": "MyBroadcast"

integer
^^^^^^^

The value must be an integer.

::

   "quality": 1500

float
^^^^^

The value must be an integer.

::

   "top": 15.25

boolean
^^^^^^^

The value can only be *true* or *false*. Indicated usually if an
specific detail must be applied or not when executing the API function.

::

   "allbitrates": true

datetime
^^^^^^^^

The datetime is a versatile introduction type for time specification.
The user can specify the time in *Unix Timestamp* in seconds or in
milliseconds. Due to the specification of the standard *Transport Stream
(TS)* format file, it also allows the use of 90Hz time definition. This
type also allows string format types in the way PHP programming language
accepts, but it is recommended to use any of the previous integer types
explained to ensure there is no conversion errors.

These are the examples for the January 1st 2017 at 00:00:00:

::

   "start": 1483228800              // Unix timestamp
   "start": 1483228800000           // Unix timestamp in milliseconds
   "start": 133490592000000         // 90Hz TS time format
   "start": "2017-01-01 00:00:00"   // String format

date
^^^^

Some function requires of a similar time definition, but specifing the
parts of the second in a different way, for example, on frames. With
this type, you can use a dot (.) to specify the frames (2 digits) or the
milliseconds (3 digits) of the second indicated in the first part of the
value. This first part can only be on *Unix Timestamp* or in text
format, but it is also recommended to use the integer format.

::

   "start": 1483228800.05           // Frames
   "start": 1483228800.125          // Milliseconds

enum
^^^^

Requires a string but only accepts a defined enumeration of values,
described on the *Extra info* part of the field.

::

   "type": "clock"

color
^^^^^

Used to specify a color. Must be on hexadecimal RGB prefixed by an #.

::

    "forecolor": "#1200ac"

object
^^^^^^

Includes another object. The definition of it will be explaned in
another fields table linked from the *Extra info* part of this field.

::

   "ftp": { ... }

array
^^^^^

A list of any type of elements, specified in the *Extra info*.

::

   "roles": [ 1, 4 ]

Authentication
--------------

It is necessary to be authenticated in the application to make any call
to the API. For example, the following command run will fail:

::

   curl -X GET 'http://<api.server.com>/c21apiv2/system/info'
   The request requires user authentication

Also, the response HTTP code will be 401 to specify the Authorization
Required. To realize the required authentication, we have to call the
API function ``security/login``. This function receives a user and a
real password and, if the authetication is successfull, it will provide
a cookie that can be used in the other calls. As an example:

::

   curl -c <cookie.file> -X POST -H 'Content-type: application/json' -d '{
       "username": "<username>",
       "password": "<password>"
   }' 'http://<api.server.com>/c21apiv2/security/login'
   {
       "success": true,
       "code": 0,
       "message": null,
       "data": null
   }

If the authentication fails, and error is returned:

::

   curl -c <cookie.file> -X POST -H 'Content-type: application/json' -d '{
       "username": "<username>",
       "password": "<wrong_password>"
   }' 'http://<api.server.com>/c21apiv2/security/login'
   {
       "success": false,
       "code": "SECf002",
       "message": "Login failed.",
       "data": null
   }

Finally, with the generated cookie, we can call any API function with
this authentication, for example, the previous call to ``system/user``
that fails without the cookie:

::

   curl -b <cookie.file> -X GET 'http://mos12dev02/c21apiv2/system/users'
   {
       "success": true,
       "code": 0,
       "message": null,
       "data": ...

\ `TOP ↑ <#top>`__\ 

Devices
-------

Devices management.

GET /c21apiv2/devices
~~~~~~~~~~~~~~~~~~~~~

List all object of type Device.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "alias": "string",
       "description": "string",
       "system": "boolean",
       "last_updated": "datetime",
       "host_id": "string",
       "location": "string",
       "ip_address": "string",
       "licensed": {
           "channels": "string",
           "outputs": "string",
           "publishing_points": "string"
       },
       "type": "string",
       "network_interfaces": "string",
       "device": "string",
       "enabled": "boolean",
       "edit_recordings": "boolean",
       "master": "string",
       "mountstatus": "string",
       "ticket_id": "integer",
       "instance_id": "integer",
       "client": "Client",
       "groups": "DeviceGroup"
   }</pre>

POST /c21apiv2/devices
~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Device.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

alias

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

alias of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ip_address

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the device IP address

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

set the device available for being controlled

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "alias": "<string>",
       "ip_address": "<string>",
       "enabled": <true|false>,
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/devices'

GET /c21apiv2/devices/status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the status of all devices.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices/status'

GET /c21apiv2/devices/livestatus
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the status of livestreams of all devices.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices/livestatus'

GET /c21apiv2/devices/{deviceId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Device.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "alias": "string",
       "description": "string",
       "system": "boolean",
       "last_updated": "datetime",
       "host_id": "string",
       "location": "string",
       "ip_address": "string",
       "licensed": {
           "channels": "string",
           "outputs": "string",
           "publishing_points": "string"
       },
       "type": "string",
       "network_interfaces": "string",
       "device": "string",
       "enabled": "boolean",
       "edit_recordings": "boolean",
       "master": "string",
       "mountstatus": "string",
       "ticket_id": "integer",
       "instance_id": "integer",
       "client": "Client",
       "groups": "DeviceGroup"
   }</pre>

PUT /c21apiv2/devices/{deviceId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Device.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

alias

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

alias of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

location

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

physical location of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ip_address

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the device IP address

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

type

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

type of device

.. raw:: html

   </td>

.. raw:: html

   <td>

standalone, cloud, relay

.. raw:: html

   </td>

.. raw:: html

   <td>

If value of device is Encoder

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set the device available for being controlled

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

edit_recordings

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

use the recordings of this device on the control

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If value of device is Encoder

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "alias": "<string>",
       "description": "<string>",
       "location": "<string>",
       "ip_address": "<string>",
       "type": "<standalone|cloud|relay>",
       "enabled": <true|false>,
       "edit_recordings": <true|false>
   }' 'http://<api.server.com>/c21apiv2/devices/<integer>'

DELETE /c21apiv2/devices/{deviceId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Device.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/devices/<integer>'

\ `TOP ↑ <#top>`__\ 

Devices groups
--------------

Devices groups management.

GET /c21apiv2/devices/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type DevicesGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices/groups'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Device"
   }</pre>

POST /c21apiv2/devices/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type DevicesGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the devices of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

device_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "client": "<string>",
       "elements": [
           {
               "device_id": <integer>
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/devices/groups'

GET /c21apiv2/devices/groups/{deviceGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type DevicesGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/devices/groups/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Device"
   }</pre>

PUT /c21apiv2/devices/groups/{deviceGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type DevicesGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the devices of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

device_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

action on list of devices

.. raw:: html

   </td>

.. raw:: html

   <td>

add, remove

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "elements": [
           {
               "device_id": <integer>,
               "action": "<add|remove>"
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/devices/groups/<integer>'

DELETE /c21apiv2/devices/groups/{deviceGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type DevicesGroup.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/devices/groups/<integer>'

\ `TOP ↑ <#top>`__\ 

Channels
--------

Channels management

GET /c21apiv2/channels
~~~~~~~~~~~~~~~~~~~~~~

List all object of type Channel.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/channels'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "alias": "string",
       "description": "string",
       "system": "boolean",
       "public": "boolean",
       "last_updated": "datetime",
       "type": "string",
       "hd": "boolean",
       "settings": {
           "audiotrack": "integer",
           "audiogain": "float",
           "program": "integer",
           "channel": "string",
           "url": "string",
           "filename": "string",
           "frequency": "integer",
           "polarity": "string",
           "symbol_rate": "integer",
           "valid_ips": "string",
           "multiaudio": {
               "pid": "string",
               "left": "string",
               "right": "string",
               "description": "string",
               "language": "string",
               "audiogain": "float"
           },
           "srt": {
               "port": "integer",
               "latency": "integer",
               "encryption": "string",
               "passphrase": "string"
           }
       },
       "device": {
           "id": "Device",
           "input": "integer"
       },
       "ticket_id": "integer",
       "client": "Client"
   }</pre>

POST /c21apiv2/channels
~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Channel.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

alias

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

alias of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

type

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

type of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

File, IPTV, Playout, SDI, Stream, ASI, AES/EBU, Analog Video, Analog
Audio, DVB-S, DVB-T, RTMP-Push, UDP-R Cloud, SRT

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

public

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set this channel as public for all the users

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

hd

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the settings of the channel depending of the type

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

device

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the device linked to this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI, ASI, AES/EBU, Analog Video, Analog Audio

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiotrack

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the track used for audio

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T or DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiogain

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

From 0 to 5 included decimals

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T, DVB-S or SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

program

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the program pid of the video

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is ASI, Stream, DVB-S or DVB-T

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channel

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the url used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is IPTV

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

url

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the url used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the filename used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is File or Playout

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

frequency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the frequency tuned

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S or DVB-T

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

polarity

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the polarity of the tuned channel

.. raw:: html

   </td>

.. raw:: html

   <td>

Vertical, Horizontal

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

symbol_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the symbol rate of the tuned channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

valid_ips

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of IPs separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is RTMP-Push or UDP-R Cloud

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

left

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

right

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

multiaudio

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

configuration of multiaudio track

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T, DVB-S or SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

srt

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SRT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Multiaudio members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

pid

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T or DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

left

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

right

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

language

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiogain

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

From 0 to 5 included decimals

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Srt object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

port

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

latency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encryption

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

AES-128, AES-192, AES-256

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

passphrase

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Device object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the device id linked to this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

input

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the specific input used on this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "alias": "<string>",
       "type": "<File|IPTV|Playout|SDI|Stream|ASI|AES\/EBU|Analog Video|Analog Audio|DVB-S|DVB-T|RTMP-Push|UDP-R Cloud|SRT>",
       "description": "<string>",
       "public": <true|false>,
       "hd": <true|false>,
       "client": "<string>",
       "settings": {
           "audiotrack": <integer>,
           "audiogain": <float>,
           "program": <integer>,
           "channel": "<string>",
           "url": "<string>",
           "filename": "<string>",
           "frequency": <integer>,
           "polarity": "<Vertical|Horizontal>",
           "symbol_rate": <integer>,
           "valid_ips": "<string>",
           "left": "<string>",
           "right": "<string>",
           "multiaudio": [
               {
                   "pid": "<string>",
                   "left": "<string>",
                   "right": "<string>",
                   "description": "<string>",
                   "language": "<string>",
                   "audiogain": <float>
               }
           ],
           "srt": {
               "port": <integer>,
               "latency": <integer>,
               "encryption": "<AES-128|AES-192|AES-256>",
               "passphrase": "<string>"
           }
       },
       "device": {
           "id": <integer>,
           "input": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/channels'

GET /c21apiv2/channels/{channelId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Channel.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/channels/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "alias": "string",
       "description": "string",
       "system": "boolean",
       "public": "boolean",
       "last_updated": "datetime",
       "type": "string",
       "hd": "boolean",
       "settings": {
           "audiotrack": "integer",
           "audiogain": "float",
           "program": "integer",
           "channel": "string",
           "url": "string",
           "filename": "string",
           "frequency": "integer",
           "polarity": "string",
           "symbol_rate": "integer",
           "valid_ips": "string",
           "multiaudio": {
               "pid": "string",
               "left": "string",
               "right": "string",
               "description": "string",
               "language": "string",
               "audiogain": "float"
           },
           "srt": {
               "port": "integer",
               "latency": "integer",
               "encryption": "string",
               "passphrase": "string"
           }
       },
       "device": {
           "id": "Device",
           "input": "integer"
       },
       "ticket_id": "integer",
       "client": "Client"
   }</pre>

PUT /c21apiv2/channels/{channelId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Channel.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

alias

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

alias of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

public

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set this channel as public for all the users

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

hd

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the settings of the channel depending of the type

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

device

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the device linked to this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI, ASI, AES/EBU, Analog Video, Analog Audio

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiotrack

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the track used for audio

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T or DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiogain

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

From 0 to 5 included decimals

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T, DVB-S or SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

program

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the program pid of the video

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is ASI, Stream, DVB-S or DVB-T

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channel

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the url used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is IPTV

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

url

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the url used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the filename used as input

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is File or Playout

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

frequency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the frequency tuned

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S or DVB-T

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

polarity

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the polarity of the tuned channel

.. raw:: html

   </td>

.. raw:: html

   <td>

Vertical, Horizontal

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

symbol_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the symbol rate of the tuned channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

valid_ips

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of IPs separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is RTMP-Push or UDP-R Cloud

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

left

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

right

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

multiaudio

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

configuration of multiaudio track

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T, DVB-S or SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

srt

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SRT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Multiaudio members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

pid

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is Stream, IPTV, File, Playout, DVB-T or DVB-S

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

left

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

right

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Channel 1 … 16

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDI

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

language

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audiogain

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

From 0 to 5 included decimals

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Srt object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

port

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

latency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encryption

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

AES-128, AES-192, AES-256

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

passphrase

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Device object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the device id linked to this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

input

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the specific input used on this channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "alias": "<string>",
       "description": "<string>",
       "public": <true|false>,
       "hd": <true|false>,
       "settings": {
           "audiotrack": <integer>,
           "audiogain": <float>,
           "program": <integer>,
           "channel": "<string>",
           "url": "<string>",
           "filename": "<string>",
           "frequency": <integer>,
           "polarity": "<Vertical|Horizontal>",
           "symbol_rate": <integer>,
           "valid_ips": "<string>",
           "left": "<string>",
           "right": "<string>",
           "multiaudio": [
               {
                   "pid": "<string>",
                   "left": "<string>",
                   "right": "<string>",
                   "description": "<string>",
                   "language": "<string>",
                   "audiogain": <float>
               }
           ],
           "srt": {
               "port": <integer>,
               "latency": <integer>,
               "encryption": "<AES-128|AES-192|AES-256>",
               "passphrase": "<string>"
           }
       },
       "device": {
           "id": <integer>,
           "input": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/channels/<integer>'

DELETE /c21apiv2/channels/{channelId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Channel.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/channels/<integer>'

\ `TOP ↑ <#top>`__\ 

Encodings
---------

Encodings management

GET /c21apiv2/encodings
~~~~~~~~~~~~~~~~~~~~~~~

List all object of type Encoding.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/encodings'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system_template": "boolean",
       "type": "string",
       "settings_video": {
           "active": "boolean",
           "bitrate": "integer",
           "fps": "integer",
           "size_width": "integer",
           "aspect_ratio": "string",
           "codec": "string",
           "deinterlace": "string",
           "h264": {
               "encoder": "string",
               "profile": "string",
               "level": "string",
               "keyframe_interval": "integer",
               "advanced": "string"
           }
       },
       "settings_audio": {
           "active": "boolean",
           "bitrate": "integer",
           "channels": "integer",
           "sample_rate": "integer",
           "codec": "string"
       },
       "settings_extra": {
           "cbr": "boolean",
           "cbr_rate": "integer"
       },
       "ticket_id": "integer",
       "client": "Client",
       "groups": "EncodingGroup"
   }</pre>

POST /c21apiv2/encodings
~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Encoding.For each encoding profile,
system generates automatically a “System Encoding Group” object.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

type

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

type of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

DASH, HLS, IPTV, IPTV / STREAM, Record, RTMP, SDIOUT, Smooth Streaming

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_video

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the video settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_audio

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the audio settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_extra

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the extra settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_video object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

fps

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

frames per second

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Value 0 is not fixed

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

size_width

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video width size

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aspect_ratio

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video aspect ratio

.. raw:: html

   </td>

.. raw:: html

   <td>

11:9, 16:9, 1:1, 4:3

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codec

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video codec

.. raw:: html

   </td>

.. raw:: html

   <td>

MPEG-4, Copy, H.264, HEVC, VP9, LOGO, MPEG-2

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS, DASH, RTMP or Smooth Streaming the values are Copy,
H.264, LOGO

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

deinterlace

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

deinterlace method

.. raw:: html

   </td>

.. raw:: html

   <td>

disabled, blend, x, yadif, yadif2x

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

h264

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**H264 object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoder

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

encoder type

.. raw:: html

   </td>

.. raw:: html

   <td>

Software, QuickSync

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

profile

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 profile

.. raw:: html

   </td>

.. raw:: html

   <td>

baseline, main, high

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

level

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 level

.. raw:: html

   </td>

.. raw:: html

   <td>

1.0, 1.1, 1.2, 1.3, 2.0, 2.1, 2.2, 3.0, 3.1, 3.2, 4.0, 4.1, 4.2, 5.0,
5.1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

keyframe_interval

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 keyframe interval

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

advanced

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 advanced parameters

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

For to activate advanced H264 settings, set H264=1 in the file lms.conf

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Advanced object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

preset

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

ultrafast, superfast, veryfast, faster, fast, medium, slow, slower,
veryslow, placebo

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

tune

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

film, animation, grain, stillimage, psnr, ssim, fastdecode, zerolatency

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

min-keyint

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

intra-refresh

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

scenecut

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bframes

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

b-pyramid

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, strict, normal

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

open-gop

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cabac

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ref

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

rc-lookahead

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-maxrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-bufsize

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-init

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ratetol

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

nal-hrd

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, vbr, cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aud

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mbtree

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sync-lookahead

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

interlaced

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, tff, bff

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sliced-threads

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

fullrange

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

colorprim

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, bt709, undef, bt470m, bt470bg, smpte170m, smpte240m, film

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

transfer

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, bt709, undef, bt470m, bt470bg, smpte170m, smpte240m, linear,
log100, log316

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

colormatrix

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, GBR, bt709, undef, fcc, bt470bg, smpte170m, smpte240m, YCgCo

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_audio object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channels

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio channels

.. raw:: html

   </td>

.. raw:: html

   <td>

1, 2

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sample_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio sample rate

.. raw:: html

   </td>

.. raw:: html

   <td>

22050, 44100, 48000

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is RTMP and codec of audio is MP3 the values are 22050, 44100

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codec

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio codec

.. raw:: html

   </td>

.. raw:: html

   <td>

AAC, AAC+, OPUS, Copy, MP3, MPEG-Audio

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH the values are AAC, AAC+, Copy If type is HLS the values
are AAC, MP3, Copy If type is RTMP the values are AAC, AAC+, MP3, Copy
If type is Smooth Streaming the values are AAC, Copy

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_extra object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

enabled cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cbr_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

cbr bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "type": "<DASH|HLS|IPTV|IPTV \/ STREAM|Record|RTMP|SDIOUT|Smooth Streaming>",
       "client": "<string>",
       "settings_video": {
           "bitrate": <integer>,
           "fps": <integer>,
           "size_width": <integer>,
           "aspect_ratio": "<11:9|16:9|1:1|4:3>",
           "codec": "<MPEG-4|Copy|H.264|HEVC|VP9|LOGO|MPEG-2>",
           "deinterlace": "<disabled|blend|x|yadif|yadif2x>",
           "h264": {
               "encoder": "<Software|QuickSync>",
               "profile": "<baseline|main|high>",
               "level": "<1.0|1.1|1.2|1.3|2.0|2.1|2.2|3.0|3.1|3.2|4.0|4.1|4.2|5.0|5.1>",
               "keyframe_interval": <integer>,
               "advanced": {
                   "name": "<string>",
                   "preset": "<ultrafast|superfast|veryfast|faster|fast|medium|slow|slower|veryslow|placebo>",
                   "tune": "<film|animation|grain|stillimage|psnr|ssim|fastdecode|zerolatency>",
                   "min-keyint": <integer>,
                   "intra-refresh": <true|false>,
                   "scenecut": <integer>,
                   "bframes": <integer>,
                   "b-pyramid": "<none|strict|normal>",
                   "open-gop": <true|false>,
                   "cabac": <true|false>,
                   "ref": <integer>,
                   "rc-lookahead": <integer>,
                   "vbv-maxrate": <integer>,
                   "vbv-bufsize": <integer>,
                   "vbv-init": <float>,
                   "ratetol": <float>,
                   "nal-hrd": "<none|vbr|cbr>",
                   "aud": <true|false>,
                   "mbtree": <true|false>,
                   "sync-lookahead": <integer>,
                   "interlaced": "<none|tff|bff>",
                   "sliced-threads": <true|false>,
                   "fullrange": "<none|on|off>",
                   "colorprim": "<none|bt709|undef|bt470m|bt470bg|smpte170m|smpte240m|film>",
                   "transfer": "<none|bt709|undef|bt470m|bt470bg|smpte170m|smpte240m|linear|log100|log316>",
                   "colormatrix": "<none|GBR|bt709|undef|fcc|bt470bg|smpte170m|smpte240m|YCgCo>"
               }
           }
       },
       "settings_audio": {
           "bitrate": <integer>,
           "channels": "<1|2>",
           "sample_rate": "<22050|44100|48000>",
           "codec": "<AAC|AAC+|OPUS|Copy|MP3|MPEG-Audio>"
       },
       "settings_extra": {
           "cbr": <true|false>,
           "cbr_rate": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/encodings'

GET /c21apiv2/encodings/{encodingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Encoding.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/encodings/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system_template": "boolean",
       "type": "string",
       "settings_video": {
           "active": "boolean",
           "bitrate": "integer",
           "fps": "integer",
           "size_width": "integer",
           "aspect_ratio": "string",
           "codec": "string",
           "deinterlace": "string",
           "h264": {
               "encoder": "string",
               "profile": "string",
               "level": "string",
               "keyframe_interval": "integer",
               "advanced": "string"
           }
       },
       "settings_audio": {
           "active": "boolean",
           "bitrate": "integer",
           "channels": "integer",
           "sample_rate": "integer",
           "codec": "string"
       },
       "settings_extra": {
           "cbr": "boolean",
           "cbr_rate": "integer"
       },
       "ticket_id": "integer",
       "client": "Client",
       "groups": "EncodingGroup"
   }</pre>

PUT /c21apiv2/encodings/{encodingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Encoding.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_video

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the video settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_audio

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the audio settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_extra

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the extra settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_video object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video settings active

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

fps

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

frames per second

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Value 0 is not fixed

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

size_width

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video width size

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aspect_ratio

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video aspect ratio

.. raw:: html

   </td>

.. raw:: html

   <td>

11:9, 16:9, 1:1, 4:3

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codec

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

video codec

.. raw:: html

   </td>

.. raw:: html

   <td>

MPEG-4, Copy, H.264, HEVC, VP9, LOGO, MPEG-2

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS, DASH, RTMP or Smooth Streaming the values are Copy,
H.264, LOGO

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

deinterlace

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

deinterlace method

.. raw:: html

   </td>

.. raw:: html

   <td>

disabled, blend, x, yadif, yadif2x

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

h264

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**H264 object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoder

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

encoder type

.. raw:: html

   </td>

.. raw:: html

   <td>

Software, QuickSync

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

profile

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 profile

.. raw:: html

   </td>

.. raw:: html

   <td>

baseline, main, high

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

level

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 level

.. raw:: html

   </td>

.. raw:: html

   <td>

1.0, 1.1, 1.2, 1.3, 2.0, 2.1, 2.2, 3.0, 3.1, 3.2, 4.0, 4.1, 4.2, 5.0,
5.1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

keyframe_interval

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 keyframe interval

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

advanced

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

h264 advanced parameters

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

For to activate advanced H264 settings, set H264=1 in the file lms.conf

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Advanced object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

preset

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

ultrafast, superfast, veryfast, faster, fast, medium, slow, slower,
veryslow, placebo

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

tune

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

film, animation, grain, stillimage, psnr, ssim, fastdecode, zerolatency

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

min-keyint

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

intra-refresh

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

scenecut

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bframes

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

b-pyramid

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, strict, normal

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

open-gop

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cabac

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ref

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

rc-lookahead

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-maxrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-bufsize

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

vbv-init

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ratetol

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

nal-hrd

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, vbr, cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aud

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mbtree

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sync-lookahead

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

interlaced

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, tff, bff

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sliced-threads

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

fullrange

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

colorprim

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, bt709, undef, bt470m, bt470bg, smpte170m, smpte240m, film

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

transfer

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, bt709, undef, bt470m, bt470bg, smpte170m, smpte240m, linear,
log100, log316

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

colormatrix

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

none, GBR, bt709, undef, fcc, bt470bg, smpte170m, smpte240m, YCgCo

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_audio object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio settings active

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channels

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio channels

.. raw:: html

   </td>

.. raw:: html

   <td>

1, 2

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

sample_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio sample rate

.. raw:: html

   </td>

.. raw:: html

   <td>

22050, 44100, 48000

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is RTMP and codec of audio is MP3 the values are 22050, 44100

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codec

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

audio codec

.. raw:: html

   </td>

.. raw:: html

   <td>

AAC, AAC+, OPUS, Copy, MP3, MPEG-Audio

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH the values are AAC, AAC+, Copy If type is HLS the values
are AAC, MP3, Copy If type is RTMP the values are AAC, AAC+, MP3, Copy
If type is Smooth Streaming the values are AAC, Copy

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_extra object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

enabled cbr

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cbr_rate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

cbr bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "settings_video": {
           "active": <true|false>,
           "bitrate": <integer>,
           "fps": <integer>,
           "size_width": <integer>,
           "aspect_ratio": "<11:9|16:9|1:1|4:3>",
           "codec": "<MPEG-4|Copy|H.264|HEVC|VP9|LOGO|MPEG-2>",
           "deinterlace": "<disabled|blend|x|yadif|yadif2x>",
           "h264": {
               "encoder": "<Software|QuickSync>",
               "profile": "<baseline|main|high>",
               "level": "<1.0|1.1|1.2|1.3|2.0|2.1|2.2|3.0|3.1|3.2|4.0|4.1|4.2|5.0|5.1>",
               "keyframe_interval": <integer>,
               "advanced": {
                   "name": "<string>",
                   "preset": "<ultrafast|superfast|veryfast|faster|fast|medium|slow|slower|veryslow|placebo>",
                   "tune": "<film|animation|grain|stillimage|psnr|ssim|fastdecode|zerolatency>",
                   "min-keyint": <integer>,
                   "intra-refresh": <true|false>,
                   "scenecut": <integer>,
                   "bframes": <integer>,
                   "b-pyramid": "<none|strict|normal>",
                   "open-gop": <true|false>,
                   "cabac": <true|false>,
                   "ref": <integer>,
                   "rc-lookahead": <integer>,
                   "vbv-maxrate": <integer>,
                   "vbv-bufsize": <integer>,
                   "vbv-init": <float>,
                   "ratetol": <float>,
                   "nal-hrd": "<none|vbr|cbr>",
                   "aud": <true|false>,
                   "mbtree": <true|false>,
                   "sync-lookahead": <integer>,
                   "interlaced": "<none|tff|bff>",
                   "sliced-threads": <true|false>,
                   "fullrange": "<none|on|off>",
                   "colorprim": "<none|bt709|undef|bt470m|bt470bg|smpte170m|smpte240m|film>",
                   "transfer": "<none|bt709|undef|bt470m|bt470bg|smpte170m|smpte240m|linear|log100|log316>",
                   "colormatrix": "<none|GBR|bt709|undef|fcc|bt470bg|smpte170m|smpte240m|YCgCo>"
               }
           }
       },
       "settings_audio": {
           "active": <true|false>,
           "bitrate": <integer>,
           "channels": "<1|2>",
           "sample_rate": "<22050|44100|48000>",
           "codec": "<AAC|AAC+|OPUS|Copy|MP3|MPEG-Audio>"
       },
       "settings_extra": {
           "cbr": <true|false>,
           "cbr_rate": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/encodings/<integer>'

DELETE /c21apiv2/encodings/{encodingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Encoding.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/encodings/<integer>'

\ `TOP ↑ <#top>`__\ 

Encodings groups
----------------

Encodings groups management.

GET /c21apiv2/encodings/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type EncodingsGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/encodings/groups'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "type": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Encoding"
   }</pre>

POST /c21apiv2/encodings/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type EncodingsGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the encodings of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "client": "<string>",
       "elements": [
           {
               "encoding_id": <integer>
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/encodings/groups'

GET /c21apiv2/encodings/groups/{encodingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type EncodingsGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/encodings/groups/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "type": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Encoding"
   }</pre>

PUT /c21apiv2/encodings/groups/{encodingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type EncodingsGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the encodings of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

action on list of encodings

.. raw:: html

   </td>

.. raw:: html

   <td>

add, remove

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "elements": [
           {
               "encoding_id": <integer>,
               "action": "<add|remove>"
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/encodings/groups/<integer>'

DELETE /c21apiv2/encodings/groups/{encodingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type EncodingsGroup.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/encodings/groups/<integer>'

\ `TOP ↑ <#top>`__\ 

Publishings
-----------

Publishings management

GET /c21apiv2/publishings
~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type Publishing.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishings'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "type": "string",
       "asset_id": "string",
       "settings_common": {
           "stream": "string",
           "geoblocking": "GeoBlocking",
           "username": "string",
           "password": "string",
           "urls": {
               "primary_server": "string",
               "primary": "string",
               "backup_server": "string",
               "backup": "string",
               "player": "string"
           },
           "provider": "string",
           "rss": {
               "client_id": "string",
               "client_secret": "string",
               "client_token": "string"
           },
           "swf_verification": "boolean"
       },
       "settings_recording": {
           "remotefolder": "RemoteFolder",
           "delete_after": "float"
       },
       "settings_extra": {
           "min_buffer_time": "integer",
           "time_shift_buffer_depth": "integer",
           "minimum_update_period": "integer",
           "hls": {
               "segments": "integer",
               "duration": "integer",
               "ad_duration": "integer",
               "max_ad_duration": "integer"
           },
           "token": {
               "enabled": "boolean",
               "duration": "integer",
               "gen": "integer",
               "key": "string",
               "absolute_path": "string"
           },
           "aes": {
               "active": "boolean",
               "key": "string",
               "iv": "string",
               "key_url": "string"
           },
           "srt": {
               "latency": "integer",
               "encryption": "string",
               "passphrase": "string"
           }
       },
       "settings_sdiout": {
           "format": "string",
           "encoder": "Device",
           "slot": "integer",
           "channels": "integer"
       },
       "ticket_id": "integer",
       "client": "Client",
       "groups": "PublishingGroup"
   }</pre>

POST /c21apiv2/publishings
~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Publishing.For each publishing
destination, system generates automatically a “System Publishing Group”
object.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

asset_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

type

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

type of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

DASH, HLS / TS, CMAF, IPTV, IPTV / STREAM, Record, RTMP, SDIOUT, Smooth
Streaming, SRT

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_common

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

common settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS, IPTV / STREAM, Record, RTMP or Smooth
Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_recording

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

recording settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS, IPTV / STREAM, Record, RTMP or Smooth
Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_extra

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

extra settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS or Smooth Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_sdiout

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

sdiout settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDIOUT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_common object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

stream

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the stream name

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

geoblocking

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the geoblocking id

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

username

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

username for publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

urls

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

url settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the cdn provider

.. raw:: html

   </td>

.. raw:: html

   <td>

C21 Live Cloud, Akamai, CenturyLink, Level3, Azure Media Services,
Youtube, LimeLight, Fastly, TransparentCDN, Generic CDN, Youtube API,
Facebook Live API

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mbr

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Deprecated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

swf_verification

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set the swf verification of the rtmp stream

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

rss

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

youtube or facebook account

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If provider is Youtube API or Facebook Live API

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Urls object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

primary url

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

backup url

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

player

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

player url (just informative)

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Rss object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_secret

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_token

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_recording object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

remotefolder

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the remote folder to save the recordings

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Set 0 for Local folder selected

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

delete_after

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

delete the recordings after this days

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_extra object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

min_buffer_time

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the buffer time used

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

time_shift_buffer_depth

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the timeshift buffer used

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

minimum_update_period

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the minimum time to update the manifest

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

hls

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

hls settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS or CMAF

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

token

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

token settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aes

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

srt

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SRT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Hls object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

segments

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

number of generated segments

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

duration of each segment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

default duration of advertisment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

max_ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

maximun duration of advertisment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Token object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

gen

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

absolute_path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Aes object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iv

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key_url

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Srt object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

latency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encryption

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

AES-128, AES-192, AES-256

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

passphrase

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_sdiout object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

format

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

sdi format for the output

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoder

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the encoder device to use

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

slot

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the adapter of the device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "asset_id": "<string>",
       "type": "<DASH|HLS \/ TS|CMAF|IPTV|IPTV \/ STREAM|Record|RTMP|SDIOUT|Smooth Streaming|SRT>",
       "client": "<string>",
       "settings_common": {
           "stream": "<string>",
           "geoblocking": <integer>,
           "username": "<string>",
           "password": "<string>",
           "urls": {
               "primary": "<string>",
               "backup": "<string>",
               "player": "<string>"
           },
           "provider": "<C21 Live Cloud|Akamai|CenturyLink|Level3|Azure Media Services|Youtube|LimeLight|Fastly|TransparentCDN|Generic CDN|Youtube API|Facebook Live API>",
           "mbr": <true|false>,
           "swf_verification": <true|false>,
           "rss": {
               "client_id": "<string>",
               "client_secret": "<string>",
               "client_token": "<string>"
           }
       },
       "settings_recording": {
           "remotefolder": <integer>,
           "delete_after": <float>
       },
       "settings_extra": {
           "min_buffer_time": <integer>,
           "time_shift_buffer_depth": <integer>,
           "minimum_update_period": <integer>,
           "hls": {
               "segments": <integer>,
               "duration": <integer>,
               "ad_duration": <integer>,
               "max_ad_duration": <integer>
           },
           "token": {
               "duration": <integer>,
               "gen": <integer>,
               "key": "<string>",
               "absolute_path": "<string>"
           },
           "aes": {
               "active": <true|false>,
               "key": "<string>",
               "iv": "<string>",
               "key_url": "<string>"
           },
           "srt": {
               "latency": <integer>,
               "encryption": "<AES-128|AES-192|AES-256>",
               "passphrase": "<string>"
           }
       },
       "settings_sdiout": {
           "format": "<string>",
           "encoder": <integer>,
           "slot": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/publishings'

GET /c21apiv2/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Publishing.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishings/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "type": "string",
       "asset_id": "string",
       "settings_common": {
           "stream": "string",
           "geoblocking": "GeoBlocking",
           "username": "string",
           "password": "string",
           "urls": {
               "primary_server": "string",
               "primary": "string",
               "backup_server": "string",
               "backup": "string",
               "player": "string"
           },
           "provider": "string",
           "rss": {
               "client_id": "string",
               "client_secret": "string",
               "client_token": "string"
           },
           "swf_verification": "boolean"
       },
       "settings_recording": {
           "remotefolder": "RemoteFolder",
           "delete_after": "float"
       },
       "settings_extra": {
           "min_buffer_time": "integer",
           "time_shift_buffer_depth": "integer",
           "minimum_update_period": "integer",
           "hls": {
               "segments": "integer",
               "duration": "integer",
               "ad_duration": "integer",
               "max_ad_duration": "integer"
           },
           "token": {
               "enabled": "boolean",
               "duration": "integer",
               "gen": "integer",
               "key": "string",
               "absolute_path": "string"
           },
           "aes": {
               "active": "boolean",
               "key": "string",
               "iv": "string",
               "key_url": "string"
           },
           "srt": {
               "latency": "integer",
               "encryption": "string",
               "passphrase": "string"
           }
       },
       "settings_sdiout": {
           "format": "string",
           "encoder": "Device",
           "slot": "integer",
           "channels": "integer"
       },
       "ticket_id": "integer",
       "client": "Client",
       "groups": "PublishingGroup"
   }</pre>

PUT /c21apiv2/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Publishing.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

asset_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_common

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

common settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS, IPTV / STREAM, Record, RTMP or Smooth
Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_recording

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

recording settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS, IPTV / STREAM, Record, RTMP or Smooth
Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_extra

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

extra settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH, HLS / TS or Smooth Streaming

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings_sdiout

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

sdiout settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SDIOUT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_common object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

stream

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the stream name

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

geoblocking

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the geoblocking id

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

username

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

username for publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

urls

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

url settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the cdn provider

.. raw:: html

   </td>

.. raw:: html

   <td>

C21 Live Cloud, Akamai, CenturyLink, Level3, Azure Media Services,
Youtube, LimeLight, Fastly, TransparentCDN, Generic CDN, Youtube API

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mbr

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Deprecated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

swf_verification

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set the swf verification of the rtmp stream

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

rss

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

youtube or facebook account

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If provider is Youtube API or Facebook Live API

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Urls object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

primary url

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

backup url

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

player

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

player url (just informative)

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Rss object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_secret

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client_token

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_recording object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

remotefolder

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the remote folder to save the recordings

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Set 0 for Local folder selected

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

delete_after

.. raw:: html

   </td>

.. raw:: html

   <td>

float

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

delete the recordings after this days

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_extra object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

min_buffer_time

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the buffer time used

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

time_shift_buffer_depth

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the timeshift buffer used

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

minimum_update_period

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the minimum time to update the manifest

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is DASH

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

hls

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

hls settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS or CMAF

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

token

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

token settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aes

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is HLS / TS

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

srt

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

If type is SRT

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Hls object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

segments

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

number of generated segments

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

duration of each segment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

default duration of advertisment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

max_ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

maximun duration of advertisment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Token object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

gen

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

absolute_path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Aes object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iv

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key_url

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Srt object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

latency

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encryption

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

AES-128, AES-192, AES-256

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

passphrase

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Settings_sdiout object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

format

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

sdi format for the output

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoder

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the encoder device to use

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

slot

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the adapter of the device

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "asset_id": "<string>",
       "settings_common": {
           "stream": "<string>",
           "geoblocking": <integer>,
           "username": "<string>",
           "password": "<string>",
           "urls": {
               "primary": "<string>",
               "backup": "<string>",
               "player": "<string>"
           },
           "provider": "<C21 Live Cloud|Akamai|CenturyLink|Level3|Azure Media Services|Youtube|LimeLight|Fastly|TransparentCDN|Generic CDN|Youtube API>",
           "mbr": <true|false>,
           "swf_verification": <true|false>,
           "rss": {
               "client_id": "<string>",
               "client_secret": "<string>",
               "client_token": "<string>"
           }
       },
       "settings_recording": {
           "remotefolder": <integer>,
           "delete_after": <float>
       },
       "settings_extra": {
           "min_buffer_time": <integer>,
           "time_shift_buffer_depth": <integer>,
           "minimum_update_period": <integer>,
           "hls": {
               "segments": <integer>,
               "duration": <integer>,
               "ad_duration": <integer>,
               "max_ad_duration": <integer>
           },
           "token": {
               "enabled": <true|false>,
               "duration": <integer>,
               "gen": <integer>,
               "key": "<string>",
               "absolute_path": "<string>"
           },
           "aes": {
               "active": <true|false>,
               "key": "<string>",
               "iv": "<string>",
               "key_url": "<string>"
           },
           "srt": {
               "latency": <integer>,
               "encryption": "<AES-128|AES-192|AES-256>",
               "passphrase": "<string>"
           }
       },
       "settings_sdiout": {
           "format": "<string>",
           "encoder": <integer>,
           "slot": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/publishings/<integer>'

DELETE /c21apiv2/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Publishing.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/publishings/<integer>'

GET /c21apiv2/publishing/providers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Return list of providers of an publishing.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishing/providers'

\ `TOP ↑ <#top>`__\ 

Publishings groups
------------------

Publishings groups management.

GET /c21apiv2/publishings/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type PublishingsGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishings/groups'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Publishing"
   }</pre>

POST /c21apiv2/publishings/groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type PublishingsGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the publishings of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "client": "<string>",
       "elements": [
           {
               "publishing_id": <integer>
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/publishings/groups'

GET /c21apiv2/publishings/groups/{publishingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type PublishingsGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishings/groups/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "system": "boolean",
       "client": "Client",
       "elements": "Publishing"
   }</pre>

PUT /c21apiv2/publishings/groups/{publishingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type PublishingsGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the publishings of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

action on list of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

add, remove

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "elements": [
           {
               "publishing_id": <integer>,
               "action": "<add|remove>"
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/publishings/groups/<integer>'

DELETE /c21apiv2/publishings/groups/{publishingGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type PublishingsGroup.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/publishings/groups/<integer>'

\ `TOP ↑ <#top>`__\ 

Publishings Player Info
-----------------------

Publishings player info

GET /c21apiv2/publishings/{publishingName}/playerinfo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the info of player

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/publishings/{publishingName}/playerinfo'

\ `TOP ↑ <#top>`__\ 

Broadcasts
----------

Livestreams

GET /c21apiv2/livestreams
~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type Livestream.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreams'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "public": "boolean",
       "automatic": "boolean",
       "status": "boolean",
       "record": "integer",
       "enabled": "boolean",
       "aspect_ratio": "string",
       "meta_data_cache": "string",
       "meta_data_type": "string",
       "webservice": "boolean",
       "asrunlog_format": "string",
       "drm": {
           "provider": "integer",
           "content_id": "string"
       },
       "mosaic_ticket_id": "integer",
       "monitor_ticket_id": "integer",
       "client": "Client",
       "geoblocking": "GeoBlocking",
       "channel": "Channel",
       "groups": {
           "user": "UserGroup",
           "encoder": "DeviceGroup",
           "mosaic": "DeviceGroup",
           "monitor": "DeviceGroup"
       },
       "broadcastDetails": "BroadcastDetails"
   }</pre>

POST /c21apiv2/livestreams
~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channel

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

public

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set this livestream as public or private

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

automatic

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if this livestream will start and stop automatically with the
timeframes defined

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the livestream have to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

userGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the user group that owns the livestream

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

webservice

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the webservice should be called on specific livestream changes

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

asrunlog_format

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the filename format when using asrunlog functionality

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

drm

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Protected with DRM: only available for HLS and MPEG-DASH publishing
formats

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

broadcastDetails

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the livestreams details

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Drm object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

content_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

The content ID must be hexadecimal value.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**BroadcastDetails members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The name of detail of broadcast.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a encoding profile, you have to introduce the “System
Encoding Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a publishing destination, you have to introduce the “System
Publishing Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "channel": <integer>,
       "description": "<string>",
       "public": <true|false>,
       "automatic": <true|false>,
       "enabled": <true|false>,
       "userGroup": <integer>,
       "webservice": <true|false>,
       "asrunlog_format": "<string>",
       "client": "<string>",
       "drm": {
           "provider": <integer>,
           "content_id": "<string>"
       },
       "broadcastDetails": [
           {
               "name": "<string>",
               "encoding": <integer>,
               "publishing": <integer>
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/livestreams'

GET /c21apiv2/livestreams/{livestreamId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Livestream.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreams/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "public": "boolean",
       "automatic": "boolean",
       "status": "boolean",
       "record": "integer",
       "enabled": "boolean",
       "aspect_ratio": "string",
       "meta_data_cache": "string",
       "meta_data_type": "string",
       "webservice": "boolean",
       "asrunlog_format": "string",
       "drm": {
           "provider": "integer",
           "content_id": "string"
       },
       "mosaic_ticket_id": "integer",
       "monitor_ticket_id": "integer",
       "client": "Client",
       "geoblocking": "GeoBlocking",
       "channel": "Channel",
       "groups": {
           "user": "UserGroup",
           "encoder": "DeviceGroup",
           "mosaic": "DeviceGroup",
           "monitor": "DeviceGroup"
       },
       "broadcastDetails": "BroadcastDetails"
   }</pre>

PUT /c21apiv2/livestreams/{livestreamId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channel

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of channel

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

public

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set this livestream as public or private

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

automatic

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if this livestream will start and stop automatically with the
timeframes defined

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the livestream have to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

userGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the user group that owns the livestream

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

webservice

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the webservice should be called on specific livestream changes

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

asrunlog_format

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the filename format when using asrunlog functionality

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

drm

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Protected with DRM: only available for HLS and MPEG-DASH publishing
formats

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

broadcastDetails

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the livestreams details

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Drm object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

content_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

The content ID must be hexadecimal value.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**BroadcastDetails members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

detail_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The identifier of detail of broadcast.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If action is remove

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The name of detail of broadcast.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If action is add

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a encoding profile, you have to introduce the “System
Encoding Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If action is add

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a publishing destination, you have to introduce the “System
Publishing Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If action is add

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The action on the list of details.

.. raw:: html

   </td>

.. raw:: html

   <td>

add, remove

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "channel": "<string>",
       "description": "<string>",
       "public": <true|false>,
       "automatic": <true|false>,
       "enabled": <true|false>,
       "userGroup": <integer>,
       "webservice": <true|false>,
       "asrunlog_format": "<string>",
       "drm": {
           "provider": <integer>,
           "content_id": "<string>"
       },
       "broadcastDetails": [
           {
               "detail_id": <integer>,
               "name": "<string>",
               "encoding": <integer>,
               "publishing": <integer>,
               "action": "<add|remove>"
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/livestreams/<integer>'

DELETE /c21apiv2/livestreams/{livestreamId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Livestream.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/livestreams/<integer>'

POST /c21apiv2/livestreams/{livestreamId}/start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start of livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

geoBlocking

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

aspectRatio

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

4:3, 16:9, 11:9, 1:1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoderGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

If encoder is Cloud then must be value 0, If encoder is C21LiveCloud
then must be value -1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mosaicGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

If mosaic is Cloud then must be value 0, If mosaic is C21LiveCloud then
must be value -1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

monitorGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

If monitor is Cloud then must be value 0, If monitor is C21LiveCloud
then must be value -1

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

record

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

entrypoints

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

overlay

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

scte

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

logo

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

crop

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Entrypoints members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the identifier of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set of ids from qualities activated separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Overlay object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

text

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Special date formats can be used to insert the text. The most common
ones are: %d: day -%m: month -%Y: year -%H: hours -%M: minutes -%S:
seconds -%C: chronometer.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

position

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Center, Left, Right, Top, Bottom, Top-Left, Top-Right, Bottom-Left,
Bottom-Right

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

size

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

font

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

color

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

date_offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

x

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

y

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Scte object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cuepoints

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

blackout

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_tags

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_variable

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_valueIn

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_valueOut

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_countdown

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Logo object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

position

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Center, Left, Right, Top, Bottom, Top-Left, Top-Right, Bottom-Left,
Bottom-Right

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Crop object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

top

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The crop must be in the range of 0 to 50 percent.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

right

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The crop must be in the range of 0 to 50 percent.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bottom

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The crop must be in the range of 0 to 50 percent.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

left

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The crop must be in the range of 0 to 50 percent.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "geoBlocking": <integer>,
       "aspectRatio": "<4:3|16:9|11:9|1:1>",
       "encoderGroup": <integer>,
       "mosaicGroup": <integer>,
       "monitorGroup": <integer>,
       "record": <true|false>,
       "entrypoints": [
           {
               "publishing": <integer>,
               "qualities": [
                   <integer>
               ],
               "primary": "<on|off>",
               "backup": "<on|off>"
           }
       ],
       "overlay": {
           "text": "<string>",
           "position": "<Center|Left|Right|Top|Bottom|Top-Left|Top-Right|Bottom-Left|Bottom-Right>",
           "size": <integer>,
           "font": "<string>",
           "color": "<string>",
           "recording_offset": <integer>,
           "date_offset": <integer>,
           "x": <integer>,
           "y": <integer>
       },
       "scte": {
           "cuepoints": <true|false>,
           "blackout": "<string>",
           "id3_tags": <true|false>,
           "id3_variable": "<string>",
           "id3_valueIn": "<string>",
           "id3_valueOut": "<string>",
           "id3_countdown": <integer>
       },
       "logo": {
           "filename": "<string>",
           "position": "<Center|Left|Right|Top|Bottom|Top-Left|Top-Right|Bottom-Left|Bottom-Right>"
       },
       "crop": {
           "top": <integer>,
           "right": <integer>,
           "bottom": <integer>,
           "left": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/livestreams/<integer>/start'

POST /c21apiv2/livestreams/{listofIds}/faststart
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Fast start of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreams/<string>/faststart'

POST /c21apiv2/livestreams/{livestreamId}/stop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Stop of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreams/<integer>/stop'

GET /c21apiv2/livestreams/status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get status of all livestreams.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreams/status'

GET /c21apiv2/livestreams/{livestreamId}/status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get status of livestream.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreams/{livestreamId}/status'

GET /c21apiv2/livestreams/{livestreamId}/playouts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get the playouts of livestream.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreams/{livestreamId}/playouts'

POST /c21apiv2/livestreams/{livestreamId}/playout/stop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Stop the playout of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreams/{livestreamId}/playout/stop'

POST /c21apiv2/livestreams/{livestreamId}/playout/status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get the status of playout of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreams/{livestreamId}/playout/status'

POST /c21apiv2/livestreams/{livestreamId}/text
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set the text or hide text of livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

action of text on the broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

show, hide

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

text

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

text of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

position

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the position of text from broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

color

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the color of text from broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

size

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the size of text from broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "action": "<show|hide>",
       "text": "<string>",
       "position": <integer>,
       "color": "<string>",
       "size": <integer>
   }' 'http://<api.server.com>/c21apiv2/livestreams/{livestreamId}/text'

POST /c21apiv2/livestreams/{livestreamId}/record
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start or stop the recording of a configured livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

record

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "record": <true|false>
   }' 'http://<api.server.com>/c21apiv2/livestreams/<integer>/record'

\ `TOP ↑ <#top>`__\ 

Broadcasts Details
------------------

Management of details of Livestreams.

GET /c21apiv2/livestreamdetails
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type LivestreamDetail.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreamdetails'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "status": "boolean",
       "enabled": "boolean",
       "slot": "integer",
       "doRecording": "boolean",
       "logo": {
           "filename": "string",
           "position": "integer"
       },
       "crop": {
           "left": "float",
           "right": "float",
           "top": "float",
           "bottom": "float"
       },
       "overlay": {
           "text": "string",
           "position": "integer",
           "size": "integer",
           "font": "string",
           "color": "string",
           "date_offset": "integer"
       },
       "scte": {
           "cuepoints": "boolean",
           "blackout": "string",
           "id3_tags": "boolean",
           "id3_variable": "string",
           "id3_valueIn": "string",
           "id3_valueOut": "string",
           "id3_countdown": "integer"
       },
       "rss": {
           "broadcast": "string",
           "stream": "string"
       },
       "recording_offset": "integer",
       "ticket_id": "integer",
       "fake": "integer",
       "real_channel_id": "integer",
       "broadcast": "Broadcast",
       "encoder": "Device",
       "encodingGroup": "EncodingGroup",
       "publishingGroup": "PublishingGroup",
       "qualities": "BroadcastDetailQuality"
   }</pre>

POST /c21apiv2/livestreamdetails
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type LivestreamDetail.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The name of broadcast detail.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a encoding profile, you have to introduce the “System
Encoding Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a publishing destination, you have to introduce the “System
Publishing Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

The identifier of broadcast belong to detail.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "encoding": <integer>,
       "publishing": <integer>,
       "broadcast": <integer>
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails'

GET /c21apiv2/livestreamdetails/{livestreamdetailId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type LivestreamDetail.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/livestreamdetails/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "status": "boolean",
       "enabled": "boolean",
       "slot": "integer",
       "doRecording": "boolean",
       "logo": {
           "filename": "string",
           "position": "integer"
       },
       "crop": {
           "left": "float",
           "right": "float",
           "top": "float",
           "bottom": "float"
       },
       "overlay": {
           "text": "string",
           "position": "integer",
           "size": "integer",
           "font": "string",
           "color": "string",
           "date_offset": "integer"
       },
       "scte": {
           "cuepoints": "boolean",
           "blackout": "string",
           "id3_tags": "boolean",
           "id3_variable": "string",
           "id3_valueIn": "string",
           "id3_valueOut": "string",
           "id3_countdown": "integer"
       },
       "rss": {
           "broadcast": "string",
           "stream": "string"
       },
       "recording_offset": "integer",
       "ticket_id": "integer",
       "fake": "integer",
       "real_channel_id": "integer",
       "broadcast": "Broadcast",
       "encoder": "Device",
       "encodingGroup": "EncodingGroup",
       "publishingGroup": "PublishingGroup",
       "qualities": "BroadcastDetailQuality"
   }</pre>

PUT /c21apiv2/livestreamdetails/{livestreamdetailId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type LivestreamDetail.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

The name of broadcast detail

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a encoding profile, you have to introduce the “System
Encoding Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

To associate a publishing destination, you have to introduce the “System
Publishing Group”.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "encoding": <integer>,
       "publishing": <integer>
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/<integer>'

DELETE /c21apiv2/livestreamdetails/{livestreamdetailId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type LivestreamDetail.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/livestreamdetails/<integer>'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start the detail of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/start'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/stop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Stop the detail of livestream.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/stop'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/setBlackout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set blackout mode to detail of livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mode

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

Mode of Blackout

.. raw:: html

   </td>

.. raw:: html

   <td>

Live, Black Frame, File

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

file

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

File of Blackout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

If mode is File then select a file

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "mode": "<Live|Black Frame|File>",
       "file": "<string>"
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/setBlackout'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/SetAdValues
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set values for advertisment to detail of livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

default_ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

default duration of advertisment in seconds

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

max_ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

maximun duration of advertisment in seconds

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

next_ad_duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

next duration of advertisment in seconds

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "default_ad_duration": <integer>,
       "max_ad_duration": <integer>,
       "next_ad_duration": <integer>
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/SetAdValues'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/sendMetaData
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Send meta data to detail from broadcast

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

metadata

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

type

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Metadata, CuePoint, DateRange, ID3Tag, CueIn, CueOut, ScteIn, ScteOut

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Metadata object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

value

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "metadata": {
           "name": "<string>",
           "value": "<string>"
       },
       "type": "<Metadata|CuePoint|DateRange|ID3Tag|CueIn|CueOut|ScteIn|ScteOut>"
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/sendMetaData'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/setDetailsOptions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set options from detail for broadcast

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

logo

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

overlay

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

scte

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Logo object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

position

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Possible values: Center, Left, Right, Top, Bottom, Top-Left, Top-Right,
Bottom-Left, Bottom-Right.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Overlay object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

text

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Special date formats can be used to insert the text. The most common
ones are: %d: day -%m: month -%Y: year -%H: hours -%M: minutes -%S:
seconds -%C: chronometer.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

position

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Possible Values: Center, Left, Right, Top, Bottom, Top-Left, Top-Right,
Bottom-Left, Bottom-Right.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

size

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

font

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

color

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

date_offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

x

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

y

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Scte object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

cuepoints

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

blackout

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_tags

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_variable

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_valueIn

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_valueOut

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id3_countdown

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "logo": {
           "filename": "<string>",
           "position": "<string>"
       },
       "overlay": {
           "text": "<string>",
           "position": "<string>",
           "size": <integer>,
           "font": "<string>",
           "color": "<string>",
           "recording_offset": <integer>,
           "date_offset": <integer>,
           "x": <integer>,
           "y": <integer>
       },
       "scte": {
           "cuepoints": <true|false>,
           "blackout": "<string>",
           "id3_tags": <true|false>,
           "id3_variable": "<string>",
           "id3_valueIn": "<string>",
           "id3_valueOut": "<string>",
           "id3_countdown": <integer>
       }
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/{livestreamdetailId}/setDetailsOptions'

POST /c21apiv2/livestreamdetails/{livestreamdetailId}/record
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start or stop the recording of detail livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

record

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "record": <true|false>
   }' 'http://<api.server.com>/c21apiv2/livestreamdetails/<integer>/record'

\ `TOP ↑ <#top>`__\ 

Broadcast Details Qualities
---------------------------

Broadcast details qualities management

GET /c21apiv2/broadcastdetails/{broadcastDetailId}/qualities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type BroadcastDetailQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/broadcastdetails/{broadcastDetailId}/qualities'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "status": "Link\/Go\/Error\/Warning",
       "textStatus": "string",
       "qualities": "integer",
       "audios": "integer",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

POST /c21apiv2/broadcastdetails/{broadcastDetailId}/qualities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type BroadcastDetailQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the identifier of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encodingGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the identifier of group of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set of ids from qualities activated separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "publishing": <integer>,
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "encodingGroup": <integer>,
       "qualities": [
           <integer>
       ],
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/broadcastdetails/{broadcastDetailId}/qualities'

GET /c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type BroadcastDetailQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}?qualityId=<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "qualities": "integer",
       "audios": "integer",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

PUT /c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type BroadcastDetailQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set of ids from qualities activated separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "qualities": [
           <integer>
       ],
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}?qualityId=<integer>'

DELETE /c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type BroadcastDetailQualitie.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/broadcastdetails/{broadcastDetailId}/qualities/{publishingId}?qualityId=<integer>'

GET /c21apiv2/broadcastdetails/qualities/{qualityId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type BroadcastDetailQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/broadcastdetails/qualities/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "qualities": "integer",
       "audios": "integer",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

PUT /c21apiv2/broadcastdetails/qualities/{qualityId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type BroadcastDetailQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set of ids from qualities activated separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "qualities": [
           <integer>
       ],
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/broadcastdetails/qualities/<integer>'

DELETE /c21apiv2/broadcastdetails/qualities/{qualityId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type BroadcastDetailQualitie.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/broadcastdetails/qualities/<integer>'

GET /c21apiv2/broadcastdetails/{broadcastDetailId}/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type BroadcastDetailPublishingQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/broadcastdetails/<integer>/publishings/{publishingId}'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "status": "Link\/Go\/Error\/Warning",
       "textStatus": "string",
       "qualities": "integer",
       "audios": "integer",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

PUT /c21apiv2/broadcastdetails/{broadcastDetailId}/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type BroadcastDetailPublishingQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encodingGroup

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the identifier of group of encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set of ids from qualities activated separated by coma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "encodingGroup": <integer>,
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "qualities": [
           <integer>
       ],
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/broadcastdetails/<integer>/publishings/{publishingId}'

DELETE /c21apiv2/broadcastdetails/{broadcastDetailId}/publishings/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type BroadcastDetailPublishingQualitie.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/broadcastdetails/<integer>/publishings/{publishingId}'

\ `TOP ↑ <#top>`__\ 

Broadcasts urls
---------------

Management of urls of Livestreams.

GET /c21apiv2/url/{broadcastName}/{broadcastDetailName}/quality/{qualityName}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the url of broadcast.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/url/<string>/<string>/quality/<string>'

\ `TOP ↑ <#top>`__\ 

TimeFrames Qualities
--------------------

Management of timeframes qualities.

GET /c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type TimeframeQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "qualities": "integer",
       "audios": "integer",
       "timeframe": "Timeframe",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

POST /c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type TimeframeQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the identifier of publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

bit set of qualities activated for this publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "publishing": <integer>,
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "qualities": <integer>,
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities'

GET /c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type TimeframeQualitie.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/timeframes/<integer>/{broadcastDetailId}/qualities/{publishingId}'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "state": {
           "primary": "on\/off",
           "backup": "on\/off"
       },
       "qualities": "integer",
       "audios": "integer",
       "timeframe": "Timeframe",
       "broadcastDetail": "BroadcastDetail",
       "publishing": "Publishing"
   }</pre>

PUT /c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type TimeframeQualitie.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

state

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

qualities

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

bit set of qualities activated for this publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audios

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

binary that represent the set of pids from audiotracks separated

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**State object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

primary

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if primary url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

backup

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if backup url has to be started

.. raw:: html

   </td>

.. raw:: html

   <td>

on, off

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "state": {
           "primary": "<on|off>",
           "backup": "<on|off>"
       },
       "qualities": <integer>,
       "audios": <integer>
   }' 'http://<api.server.com>/c21apiv2/timeframes/<integer>/{broadcastDetailId}/qualities/{publishingId}'

DELETE /c21apiv2/timeframes/{timeframeId}/{broadcastDetailId}/qualities/{publishingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type TimeframeQualitie.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/timeframes/<integer>/{broadcastDetailId}/qualities/{publishingId}'

\ `TOP ↑ <#top>`__\ 

Geoblockings
------------

Management of geoblockings.

GET /c21apiv2/geoblockings
~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type GeoBlocking.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/geoblockings'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "iphone_index": "integer",
       "system_value": "string",
       "client": "Client"
   }</pre>

POST /c21apiv2/geoblockings
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type GeoBlocking.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of geoblocking

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iphone_index

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

system_value

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "iphone_index": <integer>,
       "system_value": "<string>",
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/geoblockings'

GET /c21apiv2/geoblockings/{geoBlockingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type GeoBlocking.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/geoblockings/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "iphone_index": "integer",
       "system_value": "string",
       "client": "Client"
   }</pre>

PUT /c21apiv2/geoblockings/{geoBlockingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type GeoBlocking.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of geoblocking

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iphone_index

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

system_value

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "iphone_index": <integer>,
       "system_value": "<string>"
   }' 'http://<api.server.com>/c21apiv2/geoblockings/<integer>'

DELETE /c21apiv2/geoblockings/{geoBlockingId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type GeoBlocking.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/geoblockings/<integer>'

\ `TOP ↑ <#top>`__\ 

Playouts
--------

Management of playouts.

GET /c21apiv2/playouts
~~~~~~~~~~~~~~~~~~~~~~

List all object of type Playout.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/playouts'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "start": "integer",
       "real_start": "integer",
       "real_end": "integer",
       "offset": "integer",
       "duration": "integer",
       "path": "string",
       "filename": "string",
       "result": "string",
       "broadcast": "Broadcast",
       "broadcastName": "string",
       "mediafolder": "MediaFolder",
       "mediafolderName": "string"
   }</pre>

POST /c21apiv2/playouts
~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Playout.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of broadcast

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime for starting the play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

real_start

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

after started, the real unix timestamp when the play starts

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

real_end

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

after stopped, the real unix timestamp when the play stops

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the offset of the file to start the play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

duration of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mediafolder

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the media folder id where the file is stored

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the path inside de media folder where the file is stored

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of file

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

result

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the result code of this file play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "broadcast": <integer>,
       "name": "<string>",
       "description": "<string>",
       "start": <integer>,
       "real_start": <integer>,
       "real_end": <integer>,
       "offset": <integer>,
       "duration": <integer>,
       "mediafolder": <integer>,
       "path": "<string>",
       "filename": "<string>",
       "result": "<string>"
   }' 'http://<api.server.com>/c21apiv2/playouts'

GET /c21apiv2/playouts/{playoutId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Playout.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/playouts/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "description": "string",
       "start": "integer",
       "real_start": "integer",
       "real_end": "integer",
       "offset": "integer",
       "duration": "integer",
       "path": "string",
       "filename": "string",
       "result": "string",
       "broadcast": "Broadcast",
       "broadcastName": "string",
       "mediafolder": "MediaFolder",
       "mediafolderName": "string"
   }</pre>

PUT /c21apiv2/playouts/{playoutId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Playout.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

description of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime for starting the play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

real_start

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

after started, the real unix timestamp when the play starts

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

real_end

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

after stopped, the real unix timestamp when the play stops

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

offset

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the offset of the file to start the play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

duration

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

duration of playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the path inside de media folder where the file is stored

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of file

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

result

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the result code of this file play

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "description": "<string>",
       "start": <integer>,
       "real_start": <integer>,
       "real_end": <integer>,
       "offset": <integer>,
       "duration": <integer>,
       "path": "<string>",
       "filename": "<string>",
       "result": "<string>"
   }' 'http://<api.server.com>/c21apiv2/playouts/<integer>'

DELETE /c21apiv2/playouts/{playoutId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Playout.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/playouts/<integer>'

\ `TOP ↑ <#top>`__\ 

Remote Folders
--------------

Management of remote folders.

GET /c21apiv2/remotefolders
~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type RemoteFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/remotefolders'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

POST /c21apiv2/remotefolders
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type RemoteFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of remote folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>",
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/remotefolders'

GET /c21apiv2/remoteFolders/{remoteFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type RemoteFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/remoteFolders/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

PUT /c21apiv2/remoteFolders/{remoteFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type RemoteFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of remote folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>"
   }' 'http://<api.server.com>/c21apiv2/remoteFolders/<integer>'

DELETE /c21apiv2/remoteFolders/{remoteFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type RemoteFolder.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/remoteFolders/<integer>'

\ `TOP ↑ <#top>`__\ 

Target folders
--------------

Management of target folders.

GET /c21apiv2/targetfolders
~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type TargetFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/targetfolders'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

POST /c21apiv2/targetfolders
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type TargetFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of remote folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV, SFTP, FTP, FTPXML, S3, AZURE, BRIGHTCOVE, YOUTUBE,
FACEBOOK

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV|SFTP|FTP|FTPXML|S3|AZURE|BRIGHTCOVE|YOUTUBE|FACEBOOK>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>",
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/targetfolders'

GET /c21apiv2/targetfolders/{targetFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type TargetFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/targetfolders/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

PUT /c21apiv2/targetfolders/{targetFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type TargetFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of remote folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV, SFTP, FTP, FTPXML, S3, AZURE, BRIGHTCOVE, YOUTUBE,
FACEBOOK

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV|SFTP|FTP|FTPXML|S3|AZURE|BRIGHTCOVE|YOUTUBE|FACEBOOK>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>"
   }' 'http://<api.server.com>/c21apiv2/targetfolders/<integer>'

DELETE /c21apiv2/targetfolders/{targetFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type TargetFolder.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/targetfolders/<integer>'

\ `TOP ↑ <#top>`__\ 

Media Folder
------------

Management of media folder.

GET /c21apiv2/mediafolders
~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type MediaFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/mediafolders'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

POST /c21apiv2/mediafolders
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type MediaFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of media folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>",
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/mediafolders'

GET /c21apiv2/mediafolders/{mediaFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type MediaFolder.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/mediafolders/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "host": "string",
       "folder": "string",
       "protocol": "string",
       "user": "string",
       "password": "string",
       "status": "string",
       "client": "Client"
   }</pre>

PUT /c21apiv2/mediafolders/{mediaFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type MediaFolder.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of media folder

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the host server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

folder

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the folder on the server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

protocol

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the procol used to connect

.. raw:: html

   </td>

.. raw:: html

   <td>

NFS, CIFS, WEBDAV

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

status

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the status of the folder mount

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "host": "<string>",
       "folder": "<string>",
       "protocol": "<NFS|CIFS|WEBDAV>",
       "user": "<string>",
       "password": "<string>",
       "status": "<string>"
   }' 'http://<api.server.com>/c21apiv2/mediafolders/<integer>'

DELETE /c21apiv2/mediafolders/{mediaFolderId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type MediaFolder.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/mediafolders/<integer>'

\ `TOP ↑ <#top>`__\ 

Recording
---------

Manages the recordings results generated by the system.

GET /c21apiv2/recordings
~~~~~~~~~~~~~~~~~~~~~~~~

Show all recordings.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/recordings'

POST /c21apiv2/recordings/download
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Performs the download of an interval from the selected recording. This
action does not perform any cut edition, and will return completed
minutes that include the desired times.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

channel

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the channel name of the recording

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

quality

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the quality selection

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mode

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the mode to download

.. raw:: html

   </td>

.. raw:: html

   <td>

ts, mp4, flv, audio, dash, hls, dash&hls

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

interval start to download

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

end

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

interval end to download

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name used for downloaded file

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audio

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

options for download audio tracks

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Audio object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

pids

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of pids separated by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

languages

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of languages by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codecs

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of codecs by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "channel": "<string>",
       "quality": <integer>,
       "mode": "<ts|mp4|flv|audio|dash|hls|dash&hls>",
       "start": "<datetime>",
       "end": "<datetime>",
       "name": "<string>",
       "audio": {
           "pids": [
               <integer>
           ],
           "languages": [
               "<string>"
           ],
           "codecs": [
               "<string>"
           ]
       }
   }' 'http://<api.server.com>/c21apiv2/recordings/download'

GET /c21apiv2/recordings/{recordingName}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the info of a record.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/recordings/<string>'

DELETE /c21apiv2/recordings/{recordingName}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Delete an interval of recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of recording

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

quality

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the quality selection

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

interval start

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

end

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

interval end

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X DELETE -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "quality": <integer>,
       "start": "<datetime>",
       "end": "<datetime>"
   }' 'http://<api.server.com>/c21apiv2/recordings/<string>'

GET /c21apiv2/recordings/{recordingName}/epg
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the EPG of a recorded livestream.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

quality

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X GET -H 'Content-Type: application/json' -d '{
       "quality": <integer>,
       "start": "<datetime>"
   }' 'http://<api.server.com>/c21apiv2/recordings/<string>/epg'

GET /c21apiv2/recordings/{recordingName}/tracks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the tracks of a recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

quality

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X GET -H 'Content-Type: application/json' -d '{
       "quality": <integer>,
       "start": "<datetime>"
   }' 'http://<api.server.com>/c21apiv2/recordings/<string>/tracks'

\ `TOP ↑ <#top>`__\ 

Recording editor
----------------

Executes an editor operation over a recording.

POST /c21apiv2/editor/{broadcastName}/{broadcastDetailName}/execute
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Execute a cutting edition of a recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

quality

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the quality of the video

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

allbitrates

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

operate over all bitrates recorded, if encoding selected is DASH o HLS,
then value must be True

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

intervals

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the intervals to cut

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

kfstart

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the intervals cut used a current keyframe, avoiding reencoding
the first part of the created fragment

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

concat

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

concatenate the result intervals of the same bitrate, if encoding
selected is DASH o HLS, then value must be True

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

encoding

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the result encoding format

.. raw:: html

   </td>

.. raw:: html

   <td>

ts, mp4, flv, audio, dash, hls, dash&hls

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of the result file

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

command

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the operation to do after the edition

.. raw:: html

   </td>

.. raw:: html

   <td>

download, ftp, ftp + xml, save

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

audio

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

options for download audio tracks or send audio tracks

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

drm

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

Protected with DRM: only available for HLS and MPEG-DASH publishing
formats

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

ftp

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

settings for ftp or ftp + xml

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

save

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

save to folder settings

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Intervals members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

start

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

interval start

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

end

.. raw:: html

   </td>

.. raw:: html

   <td>

datetime

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

interval end

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of the clips

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Audio object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

pids

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of pids separated by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

languages

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of languages by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

codecs

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

list of codecs by comma

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Drm object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

content_id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

The content ID must be hexadecimal value.

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Ftp object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

host

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

ftp server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

username

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

password for authentication

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

path inside the ftp server

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of the file to send

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

limit

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

speed limit in bps

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

title

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

settings only for ftp + xml

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

description

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

settings only for ftp + xml

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

publishing

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

settings only for ftp + xml

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

category

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

settings only for ftp + xml

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Save object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

targetid

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the target folder id

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

targetname

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the target folder name

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

path

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

folder inside the target

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

filename

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of file

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "quality": <integer>,
       "allbitrates": <true|false>,
       "intervals": [
           {
               "start": "<datetime>",
               "end": "<datetime>",
               "name": "<string>"
           }
       ],
       "kfstart": <true|false>,
       "concat": <true|false>,
       "encoding": "<ts|mp4|flv|audio|dash|hls|dash&hls>",
       "filename": "<string>",
       "command": "<download|ftp|ftp + xml|save>",
       "audio": {
           "pids": [
               <integer>
           ],
           "languages": [
               "<string>"
           ],
           "codecs": [
               "<string>"
           ]
       },
       "drm": {
           "provider": <integer>,
           "content_id": "<string>"
       },
       "ftp": {
           "host": "<string>",
           "username": "<string>",
           "password": "<string>",
           "path": "<string>",
           "filename": "<string>",
           "limit": <integer>,
           "title": "<string>",
           "description": "<string>",
           "publishing": "<string>",
           "category": "<string>"
       },
       "save": {
           "targetid": <integer>,
           "targetname": "<string>",
           "path": "<string>",
           "filename": "<string>"
       }
   }' 'http://<api.server.com>/c21apiv2/editor/{broadcastName}/{broadcastDetailName}/execute'

\ `TOP ↑ <#top>`__\ 

Tags
----

Manages tags of recordings.

GET /c21apiv2/tags
~~~~~~~~~~~~~~~~~~

List all object of type Tag.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X GET -H 'Content-Type: application/json' -d '{
       "recording_name": "<string>",
       "user": <integer>
   }' 'http://<api.server.com>/c21apiv2/tags'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "recording_name": "string",
       "tag": "string",
       "timestamp": "string",
       "user": "integer"
   }</pre>

POST /c21apiv2/tags
~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Tag.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of the recording to set the tag

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

tag

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

title of tag

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

timestamp

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the timestamp where the tag is set

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "recording_name": "<string>",
       "tag": "<string>",
       "timestamp": <integer>
   }' 'http://<api.server.com>/c21apiv2/tags'

GET /c21apiv2/tags/{tagId}
~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets one tag of recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X GET -H 'Content-Type: application/json' -d '{
       "recording_name": "<string>",
       "user": <integer>
   }' 'http://<api.server.com>/c21apiv2/tags/{tagId}'

PUT /c21apiv2/tags/{tagId}
~~~~~~~~~~~~~~~~~~~~~~~~~~

Update one tag of recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the name of the recording to set the tag

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

owner of tag

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

tag

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

title of tag

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "recording_name": "<string>",
       "user": <integer>,
       "tag": "<string>"
   }' 'http://<api.server.com>/c21apiv2/tags/{tagId}'

DELETE /c21apiv2/tags/{tagId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Delete one tag of recording.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

recording_name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X DELETE -H 'Content-Type: application/json' -d '{
       "recording_name": "<string>"
   }' 'http://<api.server.com>/c21apiv2/tags/{tagId}'

\ `TOP ↑ <#top>`__\ 

Users
-----

Managment of users.

GET /c21apiv2/users
~~~~~~~~~~~~~~~~~~~

List all object of type User.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/users'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": {
           "user": "string",
           "first": "string",
           "last": "string"
       },
       "email": "string",
       "password": "string",
       "staff": "boolean",
       "active": "boolean",
       "superuser": "boolean",
       "last": {
           "login": "datetime",
           "joined": "datetime"
       },
       "groups": "UserGroup"
   }</pre>

POST /c21apiv2/users
~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type User.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

email

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

email of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

password of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

staff

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is active or not

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

superuser

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is an administrator

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Name object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

username

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

first

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the first name of the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

last

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the last name of the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": {
           "user": "<string>",
           "first": "<string>",
           "last": "<string>"
       },
       "email": "<string>",
       "password": "<string>",
       "staff": <true|false>,
       "active": <true|false>,
       "superuser": <true|false>
   }' 'http://<api.server.com>/c21apiv2/users'

GET /c21apiv2/users/{userId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type User.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/users/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": {
           "user": "string",
           "first": "string",
           "last": "string"
       },
       "email": "string",
       "password": "string",
       "staff": "boolean",
       "active": "boolean",
       "superuser": "boolean",
       "last": {
           "login": "datetime",
           "joined": "datetime"
       },
       "groups": "UserGroup"
   }</pre>

PUT /c21apiv2/users/{userId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type User.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

email

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

email of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

password of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

staff

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

active

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is active or not

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

superuser

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is an administrator

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Name object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

first

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the first name of the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

last

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

the last name of the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": {
           "first": "<string>",
           "last": "<string>"
       },
       "email": "<string>",
       "password": "<string>",
       "staff": <true|false>,
       "active": <true|false>,
       "superuser": <true|false>
   }' 'http://<api.server.com>/c21apiv2/users/<integer>'

DELETE /c21apiv2/users/{userId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type User.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/users/<integer>'

\ `TOP ↑ <#top>`__\ 

Users groups
------------

Managment of group of users.

GET /c21apiv2/usergroups
~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type UserGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/usergroups'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "elements": "User"
   }</pre>

POST /c21apiv2/usergroups
~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type UserGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

users of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "elements": [
           {
               "user_id": <integer>
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/usergroups'

GET /c21apiv2/usergroups/{userGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type UserGroup.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/usergroups/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "elements": "User"
   }</pre>

PUT /c21apiv2/usergroups/{userGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type UserGroup.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of group

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

elements

.. raw:: html

   </td>

.. raw:: html

   <td>

array

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

users of group

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for members definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Elements members**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user_id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

action

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

action on list of users

.. raw:: html

   </td>

.. raw:: html

   <td>

add, remove

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "elements": [
           {
               "user_id": <integer>,
               "action": "<add|remove>"
           }
       ]
   }' 'http://<api.server.com>/c21apiv2/usergroups/<integer>'

DELETE /c21apiv2/usergroups/{userGroupId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type UserGroup.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/usergroups/<integer>'

\ `TOP ↑ <#top>`__\ 

Users profiles
--------------

Managment of profile of users.

GET /c21apiv2/userprofiles
~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type UserProfile.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/userprofiles'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "language": "integer",
       "api": "boolean",
       "admin": "boolean",
       "settings": "string",
       "role": "integer",
       "date_active": "datetime",
       "user": "User"
   }</pre>

POST /c21apiv2/userprofiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type UserProfile.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

language

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

language of user

.. raw:: html

   </td>

.. raw:: html

   <td>

English, Español

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

api

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the user is used for api or not

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

admin

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is an administrator user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

json with specific settings for the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

role

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

role of user

.. raw:: html

   </td>

.. raw:: html

   <td>

System Administrator, Operator, Advanced Operator, Editor, Playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

identifier of user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "language": "<English|Espa\u00f1ol>",
       "api": <true|false>,
       "admin": <true|false>,
       "settings": "<string>",
       "role": "<System Administrator|Operator|Advanced Operator|Editor|Playout>",
       "user": <integer>
   }' 'http://<api.server.com>/c21apiv2/userprofiles'

GET /c21apiv2/userprofiles/{userProfileId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type UserProfile.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/userprofiles/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "language": "integer",
       "api": "boolean",
       "admin": "boolean",
       "settings": "string",
       "role": "integer",
       "date_active": "datetime",
       "user": "User"
   }</pre>

PUT /c21apiv2/userprofiles/{userProfileId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type UserProfile.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

language

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

language of user

.. raw:: html

   </td>

.. raw:: html

   <td>

English, Español

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

api

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

if the user is used for api or not

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

admin

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

set if the user is an administrator user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

settings

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

json with specific settings for the user

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

role

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

role of user

.. raw:: html

   </td>

.. raw:: html

   <td>

System Administrator, Operator, Advanced Operator, Editor, Playout

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "language": "<English|Espa\u00f1ol>",
       "api": <true|false>,
       "admin": <true|false>,
       "settings": "<string>",
       "role": "<System Administrator|Operator|Advanced Operator|Editor|Playout>"
   }' 'http://<api.server.com>/c21apiv2/userprofiles/<integer>'

DELETE /c21apiv2/userprofiles/{userProfileId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type UserProfile.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/userprofiles/<integer>'

GET /c21apiv2/userprofiles/getUserProfile/{userprofileId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the profile of user.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/userprofiles/getUserProfile/{userprofileId}'

\ `TOP ↑ <#top>`__\ 

Clients
-------

Clients multi profile management

GET /c21apiv2/clients
~~~~~~~~~~~~~~~~~~~~~

List all object of type Client.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/clients'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "origin_ip": "string",
       "outputs": "integer",
       "mpp": "integer",
       "udpr": "integer",
       "bitrate": "integer",
       "enabled": "boolean",
       "owner": "boolean",
       "C21LiveCloud": {
           "id": "integer",
           "password": "string"
       },
       "C21LiveEntryPoint": {
           "user": "string",
           "password": "string"
       }
   }</pre>

POST /c21apiv2/clients
~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type Client.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

origin_ip

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

outputs

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mpp

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

udpr

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

owner

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

C21LiveCloud

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

C21LiveEntryPoint

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**C21LiveCloud object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**C21LiveEntryPoint object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "origin_ip": "<string>",
       "outputs": <integer>,
       "mpp": <integer>,
       "udpr": <integer>,
       "bitrate": <integer>,
       "enabled": <true|false>,
       "owner": <true|false>,
       "C21LiveCloud": {
           "id": <integer>,
           "password": "<string>"
       },
       "C21LiveEntryPoint": {
           "user": "<string>",
           "password": "<string>"
       }
   }' 'http://<api.server.com>/c21apiv2/clients'

GET /c21apiv2/clients/{clientId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type Client.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/clients/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "origin_ip": "string",
       "outputs": "integer",
       "mpp": "integer",
       "udpr": "integer",
       "bitrate": "integer",
       "enabled": "boolean",
       "owner": "boolean",
       "C21LiveCloud": {
           "id": "integer",
           "password": "string"
       },
       "C21LiveEntryPoint": {
           "user": "string",
           "password": "string"
       }
   }</pre>

PUT /c21apiv2/clients/{clientId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type Client.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

origin_ip

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

outputs

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

mpp

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

udpr

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

bitrate

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

enabled

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

owner

.. raw:: html

   </td>

.. raw:: html

   <td>

boolean

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

C21LiveCloud

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

C21LiveEntryPoint

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**C21LiveCloud object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

integer

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**C21LiveEntryPoint object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

user

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "origin_ip": "<string>",
       "outputs": <integer>,
       "mpp": <integer>,
       "udpr": <integer>,
       "bitrate": <integer>,
       "enabled": <true|false>,
       "owner": <true|false>,
       "C21LiveCloud": {
           "id": <integer>,
           "password": "<string>"
       },
       "C21LiveEntryPoint": {
           "user": "<string>",
           "password": "<string>"
       }
   }' 'http://<api.server.com>/c21apiv2/clients/<integer>'

DELETE /c21apiv2/clients/{clientId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type Client.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/clients/<integer>'

\ `TOP ↑ <#top>`__\ 

System - DrmWidevine
--------------------

Drm Widevine accounts management

GET /c21apiv2/system/drmwidevine
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List all object of type DrmWidevine.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/drmwidevine'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "provider_id": "string",
       "provider_key": "string",
       "provider_iv": "string",
       "status": "string",
       "environment": "string",
       "client": "Client"
   }</pre>

POST /c21apiv2/system/drmwidevine
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new object entry of type DrmWidevine.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

name of account

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

provider of account

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

environment

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Production, Staging, UAT

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

client

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Provider object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iv

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "provider": {
           "id": "<string>",
           "key": "<string>",
           "iv": "<string>"
       },
       "environment": "<Production|Staging|UAT>",
       "client": "<string>"
   }' 'http://<api.server.com>/c21apiv2/system/drmwidevine'

GET /c21apiv2/system/drmwidevine/{drmwidevineId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Show an object of type DrmWidevine.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/drmwidevine/<integer>'

Response:

.. raw:: html

   <pre>{
       "id": "integer",
       "name": "string",
       "provider_id": "string",
       "provider_key": "string",
       "provider_iv": "string",
       "status": "string",
       "environment": "string",
       "client": "Client"
   }</pre>

PUT /c21apiv2/system/drmwidevine/{drmwidevineId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update an object of type DrmWidevine.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

name

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

name of account

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

provider

.. raw:: html

   </td>

.. raw:: html

   <td>

object

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

provider of account

.. raw:: html

   </td>

.. raw:: html

   <td>

See below for object definition

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

environment

.. raw:: html

   </td>

.. raw:: html

   <td>

enum

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

Production, Staging, UAT

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

**Provider object**

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

id

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

key

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

iv

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X PUT -H 'Content-Type: application/json' -d '{
       "name": "<string>",
       "provider": {
           "id": "<string>",
           "key": "<string>",
           "iv": "<string>"
       },
       "environment": "<Production|Staging|UAT>"
   }' 'http://<api.server.com>/c21apiv2/system/drmwidevine/<integer>'

DELETE /c21apiv2/system/drmwidevine/{drmwidevineId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove an object of type DrmWidevine.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/system/drmwidevine/<integer>'

\ `TOP ↑ <#top>`__\ 

System - Images
---------------

Manages the images uploaded to the Mosaic to be used in the windows of
the application.

GET /c21apiv2/system/images
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets all the images uploaded to the server.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/images'

POST /c21apiv2/system/images
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Uploads a new image to the server.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

files

.. raw:: html

   </td>

.. raw:: html

   <td>

file

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

A file in a multipart form message

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -F 'files=@<path-to-file>' 'http://<api.server.com>/c21apiv2/system/images'

DELETE /c21apiv2/system/images/{imageId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Removes an image uploaded to the server.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/system/images/<integer>'

\ `TOP ↑ <#top>`__\ 

System - Tempfiles
------------------

Manages the tempfiles uploaded to be used in the windows of the
application.

GET /c21apiv2/system/tempfiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets all the tempfiles uploaded to the server.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/tempfiles'

POST /c21apiv2/system/tempfiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Uploads a new tempfile to the server.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

files

.. raw:: html

   </td>

.. raw:: html

   <td>

file

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

A file in a multipart form message

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -b <cookie.file> -X POST -F 'files=@<path-to-file>' 'http://<api.server.com>/c21apiv2/system/tempfiles'

DELETE /c21apiv2/system/tempfiles/{tempfileId}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Removes an tempfile uploaded to the server.

::

   curl -b <cookie.file> -X DELETE 'http://<api.server.com>/c21apiv2/system/tempfiles/<integer>'

\ `TOP ↑ <#top>`__\ 

System - Information
--------------------

Manages the status information of the server.

GET /c21apiv2/system/info
~~~~~~~~~~~~~~~~~~~~~~~~~

Get the actual status of the server system.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/info'

GET /c21apiv2/system/info/alarms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the current active alarms in the system.

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/info/alarms'

POST /c21apiv2/system/info/download
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download encrypted low level information about the status of the server
system to be sent to Cires21 if required.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/system/info/download'

POST /c21apiv2/system/info/mibs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download the MIB files.

::

   curl -b <cookie.file> -X POST 'http://<api.server.com>/c21apiv2/system/info/mibs'

GET /c21apiv2/system/info/getAvailableLogoFilenames
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get a list of filenames of the logos which are available to include into
a Broadcast

::

   curl -b <cookie.file> -X GET 'http://<api.server.com>/c21apiv2/system/info/getAvailableLogoFilenames'

\ `TOP ↑ <#top>`__\ 

Security
--------

Performs the login security process.

POST /c21apiv2/security/login
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Login into the application with an username and a password.

.. raw:: html

   <table>

.. raw:: html

   <tbody>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Name

.. raw:: html

   </th>

.. raw:: html

   <th>

Type

.. raw:: html

   </th>

.. raw:: html

   <th>

Required

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Extra info

.. raw:: html

   </th>

.. raw:: html

   <th>

Dependence

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

username

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

password

.. raw:: html

   </td>

.. raw:: html

   <td>

string

.. raw:: html

   </td>

.. raw:: html

   <td style="text-align: center;">

✔

.. raw:: html

   </td>

.. raw:: html

   <td>

the real password

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </tbody>

.. raw:: html

   </table>

::

   curl -c <cookie.file> -X POST -H 'Content-Type: application/json' -d '{
       "username": "<string>",
       "password": "<string>"
   }' 'http://<api.server.com>/c21apiv2/security/login'
