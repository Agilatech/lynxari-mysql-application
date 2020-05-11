![Lynxari IoT Platform](https://agilatech.com/images/lynxari/lynxari200x60.png) **IoT Platform**
## Lynxari MySQL Application

### Install
```
$> npm install @agilatech/lynxari-mysql-application
```
Install in the same directory in which lynxari is installed. Create a config.json file to suit.


### Purpose
The purpose of this application is to store device data outputs to MySQL databases. It queries the system for device data properties and streams, and responds whenever data appears. Using the configuration to guide its behavior, it writes the results to a database table.


### Usage
This application runs on the [Agilatech®](https://agilatech.com) Lynxari IoT platform.  As such, it is not applicable for other environments.

To use it with Lynxari, simply insert its object definition as an element in the apps array in the _applist.json_ file. On startup, the Lynxari server reads _applist.json_ and starts all applications found there.

A _config.json_ configuration file must be present in the module's main directory. For this module, that will be within the Lynxari home directory in _node\_modules/@agilatech/lynxari-mysql-application/config.json_


### Configuration
The _config.json_ file defines two arrays, the devices and the connections. 
The devices array defines the devices and their values to be saved. Each device definition is an object with the following elements:

1. **name** : The name of the device -- this is used to query the platform for a connected device of the name. The query will fail if the name is not found. Also, a directory of this name will be created under the path for the data files.
2. **database** : The name of the database to use.
3. **table** : The name of the table in which to store the values.
4. **insertDelay** : The amount of time in milliseconds which will pass waiting for other values to arrive. The purpose of this is to aggregate an insert statement with multiple values into one. It is usually preferable to insert three values in three different columns in a single INSERT rather than perform three separate inserts.
5. **storeType** : One of 'insert' or 'update'. 'insert' creates a new row for every storage, where update replaces the last row.
6. **uniqueId** : Allows inserts and updates to be specified by one or more identifiers. The motivation for this parameter is to allow a single db table to be used for multiple different data feeds. uniqueId __must__ be an object, with the keys being identical to table columns, and the values the unique identifier. For INSERT statements, the unique identifiers are added to the column name and value lists. For UPDATE statements, each unique key and value are added as a WHERE statement. At this time, if more than one key:value pairs are specified, then the WHERE clause ANDs them all together. An OR operator may be added in the future.
6. **values** : An array of values to store. These values are the names of the monitored values or value streams on the device.
7. **columns** : An array of files in which to store the values. This array should correspond to the values array, in that it should contain the same number of elements, and the indicies for values aligns with those of the files (i.e. the 2nd value element will be sotred in the 2nd file element).

There is no limit to the number of device objects which may appear in the **devices** array. 

The connections array defines the databases on which the device tables are stored. There may be any number of connections, but all connections are mirrors of one another -- in other words, there is currently no way to store only certain device values on certain databases for certain connections. Each connection may contain the following parameters:

1. **host** : The hostname or IP address on which the database server is running.
2. **user** : The username to authenticate with.
3. **password** : The password for the above user.
4. **port** : The unix port number to connect to. Defaults to 3306 if not given.
5. **socketPath** : The path to a unix domain socket to connect to.  If used, host and port are ignored, and localhost is assumed.

The config.json file must be valid JSON.

A sample config file:
```
{
  "devices":[
    {
      "name":"MOCK",
      "database":"lynxari",
      "table":"mock",
      "insertDelay":0,
      "uniqueId":{"serial": 24562571},
      "values":["value"],
      "columns":["value"]
    },
    {
      "name":"WIND",
      "database":"wxstation",
      "table":"wind",
      "insertDelay":1000,
      "storeType":"update",
      "values":["speed_mps", "direction_deg"],
      "columns":["speed", "direction"]
    }
  ],
  "connections":[
    {
      "user":"agt",
      "password":"aC7ch6ppig6v4cqi49mx",
      "socketPath":"/opt/local/var/run/mariadb/mysqld.sock"
    },
    {
      "host":"mysql.example.com",
      "user":"billybob",
      "password":"7fjH4Uh6F83JUtE2xGWq"
    }
  ]
}
```

### Copyright
Copyright © 2018-2020 [Agilatech®](https://agilatech.com). All Rights Reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
