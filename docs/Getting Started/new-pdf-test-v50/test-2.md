---
title: test 2
deprecated: false
hidden: false
metadata:
  robots: index
---
# Introduction

This bulk load tool is created to increase users' productivity in Equipment Modeling. Equipment models can be created in any spreadsheet application, such as Excel, LibreOffice Calc, etc. Spreadsheet styles don't affect parsing processes, thus leaves users visually appealing works in their favorite application. Currently, _xlsx_ file format is supported.

There are two ways of parsing the spreadsheet:

- Command-line tool:  _ExcelPopulateEquipment.exe_.  Using the command-line tool is convenient in automation, but requires that the file we want to parse is not locked.&#x20;
- A parse button in Excel application.

When modeling is ready, then parsing and committing the models to RTDB is easier.


<Image src="https://files.readme.io/69fcc16-parse_button.png" alt="Parse button in Excel application" align="center" />


# Modeling

The modeling format is simple as everything is tables. There are two kinds of tables:

- **Model table:** The model table describes the model's properties and its relationship with other models.
- **Instance table:** Instance table is a list of instances of a model.


<Image src="https://files.readme.io/a784eca-ExcelPopulateFlowCmd.png" alt="Flow Chart" align="center" />


To distinguish between model table and instance table, three keywords are reserved: **Class**, **AbstractClass** and **Instance**. If a header has **Class** or **AbstractClass** that means the table is model, and **Instance** means instance table. **AbstractClass** is a **Class** with Abstract attribute set to true.

# Model table & Instance table with simple example

Below we have two tables of a simple model and its instance. You may find the simple model code[ here.](https://github.com/cpmPlus/Examples/blob/master/EquipmentModelBulkLoadTool/SimpleExample.xlsx)

For this model we have defined:

- Properties (tank and level)
- Data Type
- Descriptions
- And if the values are Historized or not

## Model table


<Image src="https://files.readme.io/dc0a926-simple_tank_model.png" alt="Simple Tank Model Table from Excel" align="center" />


As the tables show, only level property will be historized, collected. The table could have many more properties, e.g.  EquipmentPropertyInfo , for example _Category_, _ReferenceTarget_, _Unit_, _ValueMax_, _ValueMin_, etc. But for example purposes, we want to keep this simple.

| Class : Tank | Type   | Description                 | Historized |
| :----------- | :----- | :-------------------------- | :--------- |
| Level        | Double | Water level inside the tank | True       |
| Volume       | Double | Volume of the water tank    |            |

## Instance table


<Image src="https://files.readme.io/394aa8f-simple_tank_instance_table.PNG" alt="Simple Tank Instance Table from Excel" align="center" />


## Parsing Model and Instance Table

When the model is ready, it can be parsed:

```powershell Parse Model
"C:\Program Files\ABB Oy\RTDB\Bin\ExcelPopulateEquipment\ExcelPopulateEquipment.exe" <model and instance table excel file>
```

Parsing the above model generates following JSON file:

```json Simple Tank
{
  "ClassData": [
    {
      "Class": "Equipment",
      "Instances": [
        {
          "Id": "e712132b-af53-4929-9d0e-e9ed44bef27a",
          "Name": "Tank",
          "ClassName": "Path_Tank"
        }
      ]
    },
    {
      "Class": "EquipmentPropertyInfo",
      "Instances": [
        {
          "Id": "9d0f0e71-c2e5-4a4c-aaea-4b116598b627",
          "DisplayName": "Level",
          "Equipment": "e712132b-af53-4929-9d0e-e9ed44bef27a",
          "Type": 14,
          "Description": "Water level inside the tank",
          "Historized": true
        },
        {
          "Id": "9867d720-e0f1-4c59-ad60-3873ac169f63",
          "DisplayName": "Volume",
          "Equipment": "e712132b-af53-4929-9d0e-e9ed44bef27a",
          "Type": 14,
          "Description": "Volume of the water tank"
        }
      ]
    }
  ]
}
```

According to the JSON file, models are listed as instances under the class of _Equipment_, and their properties are also listed as instances under _EquipmentPropertyInfo_. The JSON above can be committed to a database remotely:

```powershell Commit Changes to Remote DB
"C:\Program Files\ABB Oy\RTDB\Bin\VtrinCmd.exe" -c wss://<my server>/history -i <my model.json> -u nmea\<username> -ak
# -c connection string.
# -i import JSON data from files.
# -u user name for the connection.
# -ak Accept new server keys.
```

or locally:

```powershell Commit Changes to Local DB
"C:\Program Files\ABB Oy\RTDB\Bin\VtrinCmd.exe" -c <db location> -i <my model.json> -v
# -c connection string.
# -i import JSON data from files.
# -v verbose.
```

The result can be visualized in Engineering UI:


<Image src="https://files.readme.io/baf05c2-simple_tank.png" alt="Tank and its properties in Engineering UI" align="center" />


A model-table can inherit properties from another model-table by appending _: \<base table name>_. For example: _Class : Tank : Mechanical device_, means _Tank_ inherits all properties from _Mechanical device_.

# Water Tank Bulk Load Tool Example

On our second example, we will more into inheritance.

## Instance-table

First we need to define our model to create instances for it.

Instance-table starts with keyword _Instance : \<name of model>_. The above example instantiates two tanks, source and target. The header contains the model's properties, location (Parent, Name) of equipment model, and URI parts. Model's properties are for defining properties values. Locations are for defining instance's location Equipment Model. URI parts are to define where data is collected, and also if it is an input or output of data.  If some URI parts are empty, then the parsing program tries to use the last value from the same column. However, this can be overwritten, [see Water Tank System example](#section-water-pump-system) for more.

| Instance: Tank | Parent                                       | Name        | Level       | Volume | Protocol | Server location | Port  | UaNamespace                                      | Level.output |
| :------------- | :------------------------------------------- | :---------- | :---------- | :----- | :------- | :-------------- | :---- | :----------------------------------------------- | :----------- |
|                | Example site.Water transfer system.Tank area | Source tank | area1.tank1 | 10     | opc.tcp  | localhost       | 11111 | \<[http://abb.vtrin.com>](http://abb.vtrin.com>) |              |
|                | Example site.Water transfer system.Tank area | Target tank | area1.tank2 | 20     |          |                 | 22222 |                                                  | true         |

The instance table above generates the following JSON file:

```json Simple Tank Instances
{
  "ClassData": [
       {
      "Class": "DataAccessSource",
      "Instances": [
        {
          "TargetReference": "/Path_Tank[Level]/c9542eac-faa4-4671-b285-bacdec05e7aa|",
          "AccessPath": "opc.tcp://localhost:11111///area1.tank1;UaIdType=s;UaNamespace=http://abb.vtrin.com",
          "OwnerComponent": 0,
          "AccessType": 0
        },
        {
          "TargetReference": "/Path_Tank[Level]/9b67e67b-e31e-42ee-a323-09fbec3176b6|",
          "AccessPath": "opc.tcp://localhost:22222///area1.tank2;UaIdType=s;UaNamespace=http://abb.vtrin.com",
          "OwnerComponent": 0,
          "AccessType": 0,
          "Output": true
        }
      ]
    },
    {
      "Class": "Path_Tank",
      "Instances": [
        {
          "Id": "c9542eac-faa4-4671-b285-bacdec05e7aa",
          "Name": "Example site.Water transfer system.Tank area.Source tank",
          "Volume": "10"
        },
        {
          "Id": "9b67e67b-e31e-42ee-a323-09fbec3176b6",
          "Name": "Example site.Water transfer system.Tank area.Target tank",
          "Volume": "20"
        }
      ]
    }
  ]
}
```

Instances are listed under Path\_\<model name>, their locations in Equipment Model are the value of "Name". If a user has defined that historized properties are to be collected, these information instances are stored under DataAccessSource, in the JSON file.

After committing the JSON file, we will get the following Equipment Model:


<Image src="https://files.readme.io/1c15be3-simple_tank_instances.png" alt="After has committed JSON to database." align="center" />


### Good to know

- Tables' order doesn't matter.
- All header names  **except the URI parts** are case-sensitive, e.g. referencetarget is not the same as ReferenceTarget.
- If ReferenceTarget is used to refer an enumeration, make sure the enumeration is already created.
- Special class reference can be defined in Type-column as ERef: Tank. [See example](#section-water-pump-system).
- Empty rows are skipped.
- Tables with empty headers will be trimmed at the first empty column of that specific table.

# Water Pump System Example


<Image src="https://files.readme.io/9c44951-ExampleMode.png" alt="Water Pump System" align="center" />


## Defining the model

Let's model the Water Pump System above. According to the image, there are three types of equipment: Tank, Pipe, and Pump.

The equipments have the following properties:

- Tank: level, and volume
- Pipe: diameter, and flow
- Pump: current power, nominal power, source tank, target tank, operational state, and power state.

Pumps are electrical devices, all electrical devices have voltage-property:

- Electrical device: voltage.

Pump inherits all electrical device's properties.

On the other hand, pipes are mechanical devices. In this example mechanical device doesn't have a property.

All devices have a common property, manufacturer:

- Device: manufacturer

## Modeling the system

| AbstractClass : Device | Type   | Description                |
| :--------------------- | :----- | :------------------------- |
| Manufacturer           | String | Manufacturer of the device |

_Device_ is an abstract class which has one property Manufacturer. Manufacturer type is string.

| AbstractClass : Electrical device : Device | Type   | Description    |
| :----------------------------------------- | :----- | :------------- |
| Voltage                                    | Double | Device Voltage |

_Electrical device_ is an abstract class which has Voltage as property. _Electrical device_ inherits properties from _Device_, Manufacturer. Voltage's type is double, 64 bits floating-point.

| AbstractClass : Mechanical device : Device |
| :----------------------------------------- |
|                                            |

_Mechanical device_ is an abstract class that inherits properties from _Device_. All the above models are abstract, thus can't be instantiated.

| Class : Tank : Mechanical device | Type   | Description                 | Unit | Historized |
| :------------------------------- | :----- | :-------------------------- | :--- | :--------- |
| Level                            | Double | Water level inside the tank | m    | True       |
| Volume                           | Double | Volume of the water tank    | m3   |            |

_Tank_ has Level- and Volume-property. Tank inherits properties from the Mechanical device class (including from _Device_). Level is marked as historized. Both properties are double typed, 64 bits floating-point.

| Class : Pipe : Mechanical device | Type   | Description                         | Unit  | ValueMax |
| :------------------------------- | :----- | :---------------------------------- | :---- | :------- |
| Diameter                         | Double | Diameter of the pipe                | cm    |          |
| Flow                             | Double | Current water flow through the pipe | l/min | 1000     |

Pipe has Diameter- and Flow-property. Pipe inherits properties from the Mechanical device class (inhered three properties). Both properties are double. Only Flow-property is capped to a thousand liters per minute.

| Class : Pump : Electrical device | Type    | Description                                 | Unit | ReferenceTarget |
| :------------------------------- | :------ | :------------------------------------------ | :--- | :-------------- |
| Current power                    | Double  | Current power of the pump                   | W    |                 |
| Nominal power                    | Double  | Nominal power of the pump                   | W    |                 |
| Source tank                      |         | The tank, from which the pump is pumping    |      | Class:Path_Tank |
| Target tank                      |         | The tank to which the pump is pumping water |      | Class:Path_Tank |
| Operational state                | Boolean | Is pump running or not                      |      |                 |
| Power state                      | Boolean | Is pump powered or not                      |      |                 |

Pump has six properties defined and two from inheritance, eight properties in total. _Source tank_ and _Target tank_ type is Tank, which means the defined Tank-model above. This is also known as _class reference_.

### Instances

After the models are defined, they can be instantiated according to the image. In this example the system has one pump, two pipes, and two tanks:

| Instance : Tank | Parent                                       | Name        | GOT IGNORED | Level       | Volume | Level.UaNamespace                                     | Protocol | Server location | Port  | UaNamespace                                      | Level.input | Level.output |
| :-------------- | :------------------------------------------- | :---------- | :---------- | :---------- | :----- | :---------------------------------------------------- | :------- | :-------------- | :---- | :----------------------------------------------- | :---------- | :----------- |
|                 | Example site.Water transfer system.Tank area | Source tank | foo         | area1.tank1 | 20     |                                                       | opc.tcp  | localhost       | 11111 | \<[http://abb.vtrin.com>](http://abb.vtrin.com>) | True        |              |
|                 | Example site.Water transfer system.Tank area | Target tank | bar         | area1.tank2 | 30     | [www.opcfoundation.org](http://www.opcfoundation.org) |          |                 | 22222 |                                                  |             | True         |

Create two tanks for source and target. The location of the source and target tanks in the Equipment Model-tree is the combination of Parent and Names: _Example site.Water transfer system.Tank area_ and _Example site.Water transfer system.Target tank_. Headers that don't belong to the property list nor predefined list will be ignored, such as GOT IGNORED. Level-property is historized and data collection location is defined in Protocol, Server location and Port columns. The second instance inherits the protocol and server location values of their columns. However, port number and UaNamespace of the second instance have been specifically assigned to 22222 and [www.opcfoundation.org](http://www.opcfoundation.org), respectively.

| Instance : Pipe | Parent                             | Name          | Diameter | Flow | Protocol | Server location |
| :-------------- | :--------------------------------- | :------------ | :------- | :--- | :------- | :-------------- |
|                 | Example site.Water transfer system | Pipe          |          |      |          |                 |
|                 | Example site.Water transfer system | Flowback pipe |          |      |          |                 |

Create two pipes. Their location in Equipment Model-tree is a combination of _Parent_ and _Name_. Empty property columns can be removed, see below table.

| Instance: Pump | Parent                                          | Name |
| :------------- | :---------------------------------------------- | :--- |
|                | Example site.Water transfer system.Pump section | Pump |

Create one pump instance in _Example site.Water transfer system.Pump section.Pump_ of Equipment Model.

After parsing the above tables, the following JSON file will be generated:

```json water pump system
{
  "ClassData": [
    {
      "Class": "Equipment",
      "Instances": [
        {
          "Abstract": true,
          "Id": "e9a13557-db32-467a-aed6-1690331322c3",
          "Name": "Device",
          "ClassName": "Path_Device"
        },
        {
          "Abstract": true,
          "Id": "19e8c8a5-2007-4778-a423-bb18e731a769",
          "Name": "Mechanical device",
          "ClassName": "Path_Mechanical_device",
          "Base": "e9a13557-db32-467a-aed6-1690331322c3"
        },
        {
          "Abstract": true,
          "Id": "f7289e92-1e52-4a6c-b47d-d5b2f4257c20",
          "Name": "Electrical device",
          "ClassName": "Path_Electrical_device",
          "Base": "e9a13557-db32-467a-aed6-1690331322c3"
        },
        {
          "Id": "69ed728d-a04a-4529-8783-7c57642684cf",
          "Name": "Pipe",
          "ClassName": "Path_Pipe",
          "Base": "19e8c8a5-2007-4778-a423-bb18e731a769"
        },
        {
          "Id": "d5f38ddb-86ff-4414-9217-d3e0dc023f15",
          "Name": "Tank",
          "ClassName": "Path_Tank",
          "Base": "19e8c8a5-2007-4778-a423-bb18e731a769"
        },
        {
          "Id": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Name": "Pump",
          "ClassName": "Path_Pump",
          "Base": "f7289e92-1e52-4a6c-b47d-d5b2f4257c20"
        }
      ]
    },
    {
      "Class": "EquipmentPropertyInfo",
      "Instances": [
        {
          "Id": "844baf25-1412-4a09-9f60-fd79de5bdc68",
          "DisplayName": "Manufacturer",
          "Equipment": "e9a13557-db32-467a-aed6-1690331322c3",
          "Type": 18,
          "Description": "Manufacturer of the device"
        },
        {
          "Id": "174fe4c9-be2b-4e64-ae27-2f13c0a682b6",
          "DisplayName": "Voltage",
          "Equipment": "f7289e92-1e52-4a6c-b47d-d5b2f4257c20",
          "Type": 14,
          "Description": "Device Voltage"
        },
        {
          "Id": "1c9ec2f9-7491-4f7f-b43e-1488321e55c7",
          "DisplayName": "Diameter",
          "Equipment": "69ed728d-a04a-4529-8783-7c57642684cf",
          "Type": 14,
          "Description": "Diameter of the pipe",
          "Unit": "cm"
        },
        {
          "Id": "bd455aa0-fdab-4257-b7b8-baf06a65dcef",
          "DisplayName": "Flow",
          "Equipment": "69ed728d-a04a-4529-8783-7c57642684cf",
          "Type": 14,
          "Description": "Current water flow through the pipe",
          "Unit": "l/min",
          "ValueMax": "1000"
        },
        {
          "Id": "0a34ba37-c22f-49fd-bef4-fec200f1896b",
          "DisplayName": "Level",
          "Equipment": "d5f38ddb-86ff-4414-9217-d3e0dc023f15",
          "Type": 14,
          "Description": "Water level inside the tank",
          "Unit": "m",
          "Historized": true
        },
        {
          "Id": "c56da211-ab3a-46e7-8e66-ff18d9c57f18",
          "DisplayName": "Volume",
          "Equipment": "d5f38ddb-86ff-4414-9217-d3e0dc023f15",
          "Type": 14,
          "Description": "Volume of the water tank",
          "Unit": "m3"
        },
        {
          "Id": "46932777-f86c-4d61-9ef0-a18c394d6699",
          "DisplayName": "Current power",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 14,
          "Description": "Current power of the pump",
          "Unit": "W"
        },
        {
          "Id": "62b369c9-887a-492d-b9d2-747b6c3d2f5e",
          "DisplayName": "Nominal power",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 14,
          "Description": "Nominal power of the pump",
          "Unit": "W"
        },
        {
          "Id": "b1c8456d-008c-4fd8-93b0-bae973f499ec",
          "DisplayName": "Source tank",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 241,
          "Description": "The tank that pump is pumping water from"
        },
        {
          "Id": "154b3e9c-a1de-4f8a-9256-2876e2d3d92a",
          "DisplayName": "Target tank",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 241,
          "Description": "The tank that pump is pumping water into"
        },
        {
          "Id": "65b6a38e-90a9-40de-b2cb-74e2a5be8d48",
          "DisplayName": "Operational state",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 3,
          "Description": "Is pump running or not"
        },
        {
          "Id": "4e5db708-63d1-46c4-bf67-af3295600cd5",
          "DisplayName": "Power state",
          "Equipment": "1d5caeca-31b4-41fc-8d66-d3cd03c28bdb",
          "Type": 3,
          "Description": "Is pump powered or not"
        }
      ]
    },
    {
      "Class": "DataAccessSource",
      "Instances": [
        {
          "TargetReference": "/Path_Tank[Level]/b8a0b4bc-228d-45fb-a8b3-742b2ae9eb5a|",
          "AccessPath": "opc.tcp://localhost:11111///area1.tank1;UaIdType=s;UaNamespace=http://abb.vtrin.com",
          "OwnerComponent": 0,
          "AccessType": 0,
          "Input": true
        },
        {
          "TargetReference": "/Path_Tank[Level]/24e1875a-ed07-475a-8675-ef4a753e5af6|",
          "AccessPath": "opc.tcp://localhost:22222///area1.tank2;UaIdType=s;UaNamespace=www.opcfoundation.org",
          "OwnerComponent": 0,
          "AccessType": 0,
          "Output": false
        }
      ]
    },
    {
      "Class": "Path_Tank",
      "Instances": [
        {
          "Id": "b8a0b4bc-228d-45fb-a8b3-742b2ae9eb5a",
          "Name": "Example site.Water transfer system.Tank area.Source tank",
          "Volume": "1"
        },
        {
          "Id": "24e1875a-ed07-475a-8675-ef4a753e5af6",
          "Name": "Example site.Water transfer system.Tank area.Target tank",
          "Volume": "5"
        }
      ]
    },
    {
      "Class": "Path_Pipe",
      "Instances": [
        {
          "Id": "b7a1198a-f9f8-4c9b-81a7-2c908e52681d",
          "Name": "Example site.Water transfer system.Pipe"
        },
        {
          "Id": "b4fcc330-ef56-4681-a9a3-867f8b55a624",
          "Name": "Example site.Water transfer system.Flowback pipe"
        }
      ]
    },
    {
      "Class": "Path_Pump",
      "Instances": [
        {
          "Id": "0d6fbf47-d88c-4265-beb2-8b43bcc3159b",
          "Name": "Example site.Water transfer system.Pump section.Pump"
        }
      ]
    }
  ]
}
```

The generated JSON is a bit longer. The complete water system model is ready to be imported to database.

<img src="https://files.readme.io/1b1dd13-water_pump_system.png" alt="Water Pump System in Engineering UI" style={{maxWidth: '100%', margin: '0 auto'}} />

# Creating a button for parsing in Excel application

1. Import RTDB\Config\ExcelPopulateEquipment.bas in VBA editor
2. Create a shape (button) and assign the imported macro.
3. Define a directory location of ExcelPopulateEquipmentDLL.dll in excel.
4. Make sure macro is enabled in the Excel application.
5. Save the excel file as a xlsm.

Included model setup image below, so users can compare with.


<Image src="https://files.readme.io/c66f601-creating_parse_button_in_excel.png" alt="Setup for a parsing button" align="center" />


# Exporting Tutorials

Here are some examples to export equipments from RTDB to local machine. The exported file is an excel file.

```powershell Export Examples
#Exporting All Equipments and Instances from RTDB
ExcelPopulateEquipment --connectionstring C:\RTDB1\rtdbdata

#Exporting Specific Equipments and Instances From RTDB
ExcelPopulateEquipment --connectionstring C:\RTDB1\rtdbdata --export-equipment ThisEquipment --export-equipment OtherEquipment

#Exporting to Specific Location
ExcelPopulateEquipment --connectionstring C:\RTDB1\rtdbdata --output T:\ModelAndInstances.xlsm

#Exporting from Remote RTDB
ExcelPopulateEquipment --connectionstring wss://remotemachine/history --acceptnewkeys --output T:\ModelAndInstances.xlsm --user UserName
```

# Links

### Download examples for bulk load tutorials

- Download [ProtocolsExamples.xlsx](https://github.com/cpmPlus/Examples/tree/master/EquipmentModelBulkLoadTool) for a sample.

![](https://files.readme.io/b0eae6934c151bd70e01acff202df31bff2112dbada8cb8c5225e7d82621ed26-favicon.ico)
