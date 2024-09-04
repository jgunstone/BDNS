---
format: 
  html: default
  pdf: default
---
# Building Device and Asset Naming Standards initiative

Status: *release 1.1.0*

# Specification for naming syntax

## Purpose and scope

A consistent approach to naming of assets, devices and building control system control points is fundamental to:

*   Achieve consistent identification and data management of devices and assets through the life of a building.
*   Enable the deployment of digital buildings and their applications.
*   Take advantage of machine learning and artificial intelligence.

The purpose of this document is to indicate the process to be followed for building projects to name and label assets and devices.

The following definitions apply:

*   **Device/Asset Role**: a specific function in a building that is fulfilled by a device. A device role remains fixed for the function even if the hardware is replaced.
*   **Device/Asset Instance**: a specific individual piece of hardware. If the hardware is replaced for a particular device role, the device instance changes.
*   **Machine Generated/Readable Device/Asset Instance Globally Unique Identifier (Device/Asset Instance GUID)**: a sequence of digits which is machine generated, is immutable and guaranteed to be globally unique among all identifiers used for the purpose of identification of devices within the digital building system. This type of information can be generated by the manufacturer, BIM or asset management software and is unique to the _device instance_. If a device is replaced, its _Device Instance GUID_ will change. This type of information _is in the scope of this document.
*   **Human Readable Device/Asset Type**: a type of device that has been selected to achieve a specific function. A device type remains fixed (provided direct replacement) if the hardware is replaced. (This is an optional field).
*   **Human Readable Device/Asset Role Name (Device/Asset Role Name)**: an alphanumeric sequence that is selected by a human operator according to a global standard that guarantees correlation to building devices categories and uniqueness within the building for a _device role_. The device role human readable name will remain unchanged when specific hardware fulfilling the role is changed. This type of information _is in the scope of this document.
*   **Physical Label**: a physical piece of material affixed to a building control device including the printed name of the device and additional information in the form of QR code, NFC tag or other data encoding technologies. This type of information _is in the scope of this document.
*   **Network Hostname**: the hostname is a sequence of characters assigned to a device (a host) on a network. A hostname should uniquely identify a single device role on the network at a given point in time. This type of information _is not in the scope of this document.
*   **Control point name**: the name of each attribute of a device, such as a sensor value or a set point value. This type of information _is not in the scope of this document.
*   **Metadata**: data associated with a device that provide information about its attributes. This type of information _is not in the scope of this document.

## Specification clause example

The following specification clause is provided so that BDNS naming can be included in projects:

*The convention for building control device naming shall/must follow the Building Device Naming Standard (BDNS) during the project design and construction stages.*

*For a physical device i.e. FCU, CAV etc. the naming conventions for creating these names shall/must be taken from the following links:*

* https://github.com/theodi/BDNS/blob/master/BDNS_Specification_naming_syntax.md
* https://github.com/theodi/BDNS/blob/master/BDNS_Abbreviations_Register.csv

## Device/Asset identification and naming

Each device or asset will be assigned as a minimum, the following attributes:

*   A Device/Asset Instance Global Unique Identifier (GUID)
*   A Device/Asset Role Name

The structure and purpose of these attributes is described in the subsequent sections.

Where a device or asset is represented in control system software then it shall be named in accordance with this naming standard within the control system software, e.g. a controller or an inverter must be named.

Wherever possible, the naming shall be applied as close to the edge as possible, i.e. within the device itself rather than at a gateway device, e.g. an M-Bus meter may not be able to adopt the naming within the meters software itself due to the simplicity of the M-Bus protocol, meaning that a mapping would be required between the meter’s software identifier and the naming convention at a gateway.


### Device/Asset instance GUID (asset.guid)

All devices or assets shall have a unique identifier. Once generated, this shall not be changed at any point for a specific device or asset instance.

#### Definition

The device/asset instance globally unique identifier (GUID) is a machine generated and machine readable unique identifier. It is an auto-generated 128-bit number, which can be encoded using the modalities of the RFC 4122 standard or as an IFC GlobalId object of type [ifcGloballyUniqueId](http://www.buildingsmart-tech.org/ifc/IFC4x1/final/html/schema/ifcutilityresource/lexical/ifcgloballyuniqueid.htm).


#### Reference standards

[RFC 4122](https://tools.ietf.org/html/rfc4122)

[Industry Foundation Classes](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/)


#### Originator

Generated by a BIM operator, design team, project team, manufacturer or facilities team, depending on the project stage.

In projects which are utilising BIM, this would be generated by the software without human intervention and shall be converted to the latest Industry Foundation Classes encoding.

For new projects which do not utilise BIM or for application to existing projects this can be generated using one of several freely available GUID tools (e.g. [https://www.guidgenerator.com/](https://www.guidgenerator.com/)).

Alternatively, a unique identifier may be provided by the manufacturer and input to a project master database such as the BIM model.


#### Format

Auto-generated 128-bit integer number.

If generated from BIM processes, the GUID is represented by 22 [IFC base 64](https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcutilityresource/lexical/ifcgloballyuniqueid.htm) characters encoding.

If generated by GUID tools, the GUID is usually represented by a 32 digits hexadecimal integer number.

Flexibility of format is to allow for multiple workflows, for example some projects may choose to automatically generate a GUID using BIM software.



#### Examples

`58e6591f-0cef-4bb1-ac9c-6ef03ec58111 `(32 digits hexadecimal integer)

`04aEp5ymD_$u5IxhJN2aGi `([IFC base 64](https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcutilityresource/lexical/ifcgloballyuniqueid.htm) format)


#### Validation tests

For BIM workflows, the validation procedure shall include the following tests:


1. Regular expression matching to validate the ID format and avoid non-base64[^1] characters and longer or shorter lengths than 22 characters

2. Uniqueness of the ID across all device instances

##### Regular expression matching

`([0-9a-fA-F]){8}-([0-9a-fA-F]){4}-([0-9a-fA-F]){4}-([0-9a-fA-F]){4}-([0-9a-fA-F]){12} `(32 digits hexadecimal integer)

`[A-Za-z0-9_$]{22} `([IFC base 64](https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcutilityresource/lexical/ifcgloballyuniqueid.htm) format)


### Device/Asset type (asset.type)

#### Definition

The device or asset type is a human-generated identifier that is unique for a type of assets to the building. It combines a standard abbreviation for device/asset types and a numerical ID that is unique to each specific device/asset type.

#### Originator

Generated by the BIM operator, design team, project team or facilities team, depending on project stage.

#### Format

```
<X><Z>
```

where:

`X `= <type_abbreviation> (alphabetic characters only, variable length, between 2 and 6 characters)

`Z `= <asset_type_incremental_number> (variable length, optional unique integer numbers by asset type in each building, non zero padded)

The type abbreviation shall be according to the [Building Device and Asset Abbreviation Registry](https://github.com/theodi/BDNS/blob/master/BDNS_Abbreviations_Register.csv).

The format is composed of a 2 to 6 uppercase alphabetical characters long type abbreviation according to the name abbreviation (`X`) and a variable length character numeric sequence that is unique to the building (`Z`) for each asset type. No leading zeros are allowed in the asset type incremental number to avoid ambiguity.

Only uppercase alphabetic characters and numeric characters are allowed, according to the regular expression below. There is no separator character between the type abbreviation and the incremental number for the `asset.type`.

If there is only one asset type per abbreviation, the asset type incremental number can be omitted.

#### Examples

Asset type example for a lighting fixture asset type: `LT3`

Asset type example for an air handling unit asset type: `AHU10`

Asset type example for a distribution board asset type: `DB7`

Asset type example for a lighting fixture asset type when there is only one type: `LT`

Asset type example for an air handling unit asset type when there is only one type: `AHU`

Asset type example for a distribution board  asset type when there is only one type: `DB`


#### Validation tests

The validation procedure shall include the following tests:

1. Regular expression matching to validate the name format.
2. Uniqueness of the name across all asset types in the building.

##### Regular expression matching

```
[A-Z]{2,6}(?:[1-9][0-9]*)?
```

### Device/Asset role name (asset.name)


#### Definition

The device or asset role name is a human-generated identifier that is unique to the building. It combines a standard abbreviation for device/asset types and a numerical ID that is unique to each specific device/asset.

#### Originator

Generated by the BIM operator, design team, project team or facilities team, depending on project stage.

#### Format

```
<X>-<Y>
```

or

```
<XZ>-<Y>
```

where:

`X `= <type_abbreviation> (alphabetic characters only, variable length, between 2 and 6 characters)

`Y `= <building_unique_incremental_number> (variable length, unique integer numbers by building, non zero padded)

`Z `= <asset_type_incremental_number> (variable length, unique integer numbers by asset type in each building, non zero padded)

The use of the `Z` field is optional, only in case there is a need to include `asset.type` definitions.

The type abbreviation shall be according to the [Building Device and Asset Abbreviation Registry](https://github.com/theodi/BDNS/blob/master/BDNS_Abbreviations_Register.csv).

The format is composed of a 2 to 6 uppercase alphabetical characters long type abbreviation according to the name abbreviation (`X`) and a variable length character numeric sequence that is unique to the building (`Y`). No leading zeros are allowed in the building unique incremental number to avoid ambiguity.

Only uppercase alphabetic characters and numeric characters are allowed, according to the regular expression below. The separator character between the type abbreviation and incremental number is a hyphen (`-`).

For practical reasons, at the discretion of the user and as long as there are no name clashes between devices in the same building, the digits forming the second part of the device role name can be assigned following spatial or discipline specific scoping methods. For instance, the lighting fixtures on zone 1 of level 3 of a 20 storey building can all share the same first part of the name `LT-103####` and the subsequent integer numbers can be attributed incrementally.


#### Examples

Name example for a lighting fixture: `LT-15`

Name example for an air handling unit: `AHU-3`

Name example for a distribution board: `DB-2`

Name example for a lighting fixture including an asset type: `LT3-375`

Name example for an air handling unit including an asset type: `AHU10-46`

Name example for a distribution board including an asset type: `DB3-21`

#### Validation tests

The validation procedure shall include the following tests:

1. Regular expression matching to validate the name format.
2. Uniqueness of the name across all devices in the building.

##### Regular expression matching


```
[A-Z]{2,6}(?:[1-9][0-9]*)?-[1-9][0-9]*
```

## Physical device/asset labels

Physical device/asset labels shall contain the following items:

1. A human readable label with the device/asset name.
2. A machine readable label with encoded identification in at least one of the following two formats:
    1. Quick response (QR) code
    2. Near field communication (NFC) tag

QR codes and NFC tags are intended for use cases facilitating the maintenance of equipment and for providing a code or a web based user interface to a physical object.


<img width="120" alt="QR Code Example" src="https://user-images.githubusercontent.com/1533891/102150820-5bf1e600-3e69-11eb-97a7-f01112c5cb41.png">



### Physical device human readable label


#### Definition

The human readable label shall be based on the **device/asset role name** (`asset.name`) as indicated above.

For quick interpretation and conciseness, the recommendation is to keep the label to a minimum.

Optionally, the human-readable label could be augmented by:

*   A prefix, indicating the building identification to guarantee global uniqueness beyond the building.
*   A suffix, where concise arbitrary notation can be added to facilitate identification of device properties.

#### Originator

System integrator, master system integrator or 3rd party subcontractor, using information from manufacturer, BIM and/or asset tag registry.

#### Format


##### Optional label prefix


```
<COUNTRY>-<CITY>-<BUILDING>
```

where:

`<COUNTRY>-<CITY>-<BUILDING>` = assigned location code for building, composed of:

`<COUNTRY>` = [ISO Alpha-2 Country Code](https://www.nationsonline.org/oneworld/country_code_list.htm)

`<CITY>` = [UN LOCODE City Code](https://github.com/datasets/un-locode)

`<BUILDING>` = the project specific abbreviation for the building

The location code identification guarantees global uniqueness.

The name deliberately omits information like floor, area or connection that can change in it. The optional suffix in the physical label can however contain additional information such as floor level if useful for the specific use case.


#### Compulsory device/asset label name (same as `asset.name`)


```
<X>-<Y>
```



##### Optional label suffix


```
_<text>
```


where

`text` = a free text alphanumeric sequence without underscores and without white spaces that adds useful information to the label that can be immediately seen. Text can be hyphenated (-) to represent floor information, room information, distribution board or related equipment.

The suffix in the physical label (not in `device.name`) allows practical use of the labels to maintenance technicians and facility managers to avoid disrupting traditional workflows. With time, apps and automation, the suffix is likely to disappear.


#### Examples

Compulsory short format name label


```
TSTAT-1
```


Prefix format label


```
UK-LON-BLD1_TSTAT-1
```


Suffix format label example (optional for capturing existing tagging information)


```
TSTAT-1_RH-2-3-25-CO2
TSTAT-2_EF
```


<img width="120" alt="QR Code Example" src="https://user-images.githubusercontent.com/1533891/102150891-83e14980-3e69-11eb-88bd-8657b347a42a.png">


### Physical device/asset machine readable identification


#### Definition

The machine readable label is a physical adhesive label that encodes identification and naming to enable automation for identification, provisioning, commissioning and maintenance of devices throughout their lifecycle, from initial configuration and installation to operation and decommissioning.


#### Reference standards

[ISO/IEC 18004:2015](https://www.iso.org/standard/62021.html)

[ISO/IEC 13157-1:2010](https://www.iso.org/standard/53430.html)

[ISO/IEC 18000-3:2010](https://www.iso.org/standard/53424.html)

[ISO/IEC 18092:2013](https://www.iso.org/standard/56692.html)


#### Originator

Master system integrator or 3rd party subcontractor, using information from the contractor’s BIM workflow, asset tag registry and device manufacturers.


#### Physical format

The QR code shall be created according to ISO/IEC 18004:2015. The QR code payload shall be [minified](https://en.wikipedia.org/wiki/Minification_(programming)) to reduce the number of characters and shall include the following information.


##### Model format (encoding) example for BIM models


```
{
  "asset": {
    "guid": "ifc://04aEp5ymD_$u5IxhJN2aGi",
    "site": "GB-LON-BLD1",
    "name": "AHU-1"
  }
}
```



##### Minified model format (encoding) example


```
{"asset":{"guid":"ifc://04aEp5ymD_$u5IxhJN2aGi","site":"GB-LON-BLD1","name":"AHU-1"}}
```



##### Model format (encoding) when no BIM ID is available


###### Machine generated using [uuidgen](http://man7.org/linux/man-pages/man1/uuidgen.1.html)


```
{
  "asset": {
    "guid": "uuid://d3501378-dd50-47c5-ae26-806726e1b749",
    "site": "GB-LON-BLD1",
    "name": "AHU-1"
  }
}
```



###### **Linked to the device name indicated in the drawings**


```
{
  "asset": {
    "guid": "drw://AHU001",
    "site": "GB-LON-BLD1",
    "name": "AHU-1"
  }
}
```


### Notes

[^1]:

Note that the IFC standard is slightly different than the web-standard base64 encoding, specifically using `_` and `$` instead of `/` and `+`. 

If necessary, it would be easy to do a simple 1:1 mapping for `_` ⇔ `/` and `$` ⇔ `+` if necessary for other parts of the system that may treat `$` or `_` as reserved characters.