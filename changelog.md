## NETN-CBRN Changelog

### Changes in vX
Version X of the NETN-CBRN was updated by MSG-163 and included in NETN-FOM v3.0 and AMSP-04 Ed B.

#### NETN-CBRN#1 Update modelIdentification
* Change `modelIdentification` `securityClassification` from `unclassified` to `Not Classified`
* Change `modelIdentification` `other` to include license information
* Change `modelIdentification` `reference` to only refer to directly dependent FOM Modules
* Add `modelIdentification` `useLimitation` to reflect Scope of FOM Module as defined in NETN-CBRN#2
* Update `modelIdentification` `purpose` to reflect Purpose of FOM Module as defined in NETN-CBRN#2
* Update `modelIdentification` `description` to reflect Introduction of FOM Module as defined in NETN-CBRN#2

#### NETN-CBRN#2 Update Introduction documentation
* Introduction section restructured and clarified including description, purpose and useLimitation.


### Changes in v1.1.9
Version 1.1.9 of the NETN-CBRN was developed by MSG-106 & MSG-134. 
This version is included in NETN-FOM v2.0 and AMSP-04 Ed A.

* v1.0.0 - Initial; object, interaction classes and datatypes.
* v1.0.1 - Added semantics.
* v1.0.2 - Added missing QuantityUInt32 simple data type, fixed IPEEnum values.
* v1.1.0 - Added new attributes to ATP45HazardArea, CBRN_Sensor and CBRN_Detector. Added parent interaction type to COLPROUpdate and DecontaminationStationUpdate. Re-used existing RPR and NETN simple data types. Renamed several data types and attribute names.
* v1.1.1 - Added references to dependencies.
* v1.1.2 - Added ValidityTime attribute to ATP45HazardArea.
* v1.1.3 - Updated the description.
* v1.1.4 - Added CBRN_PlatformUpdate interaction.
* v1.1.5 - Updated semantics.
* v1.1.6 - Standardised on Agent for agent name. Used correct naming convention for CBRN_FacilityUpdate.
* v1.1.7 - Changed update conditions for certain attributes. Added UniqueID attributes to hazard contours.
* v1.1.8 - Added TIC agents to AgentTypeEnum16.
* v1.1.9 - Added POC information.