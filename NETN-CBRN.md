
# NETN-CBRN
|Version| Date| Dependencies|
|---|---|---|
|v2.0|2023-03-28|NETN-Physical, NETN-SE|

The NATO Education and Training Network (NETN) CBRN FOM Module (NETN-CBRN) provides a common standard interface for the representation of CBRN release, detection, effects, and protective measures in a federated distributed simulation. E.g the exposure effect on individual humans in a CBRN contaminated Hazard area where the human is represented in one simulation and the effect is modelled in another federate simulation.

CBRN are Chemical, Biological, Radiological and Nuclear materials that can be delivered intentionally as a weapon using conventional bombs, explosive materials and enhanced blast weapons (e.g., dirty bombs) or unintentionally caused by human error or natural or technological reasons, such as spills, accidental releases or leakages. 

The NETN-CBRN FOM Module is a specification of how to model CBRN-related events in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and is primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module is used to specify how data is represented and exchanged in the federation. The NETN-CBRN FOM module is available as an XML file for use in HLA-based federations. 

The NETN-CBRN FOM module covers:  
        
1.	CBRN Source release modelling 
2.	CBRN Detector modelling 
3.	CBRN Effects modelling 
4.	CBRN Protective measures modelling
5.	Hazard area modelling  
 
 Meteorological conditions and CBRN material properties for modelling the dispersion of CBRN material are not explicitly represented in the NETN-CBRN FOM Module. NETN-METOC FOM module can be used to model weather conditions that may impact the dispersion of CBRN materials and cause dynamic change to hazard areas.


## Overview

Modelling and Simulation of CBRN involve the following aspects:
1. Triggering the release of CBRN agents 
2. Modelling CBRN agent concentration and spreading 
3. Modelling the result of decontamination activities
3. Modelling the effects of the CBRN agents on simulated entities
4. CBRN Sensor modelling
5. Detection of CBRN agents and CBRN alarms 

Depending on the federation design, these aspects are modelled in one or more CBRN federates.

### CBRN Release 

The `Release` interaction is a trigger for starting the simulation of a CBR hazard area with contours defining the spread and concentration of CBRN agents. In some cases, the `Release` is directly related to an RPR-FOM Warfare `MunitionDetonation` event with a `WarheadType` parameter indicating a CBRN release.

### CBRN Hazard Area 

A CBRN hazard area is a representation of a CBRN-contaminated area published in the federation. The hazard area can be either:
* The output from a hazard prediction algorithm (a warning area defined in Allied Tactical Publication (ATP)-45) in response to a detector alarm or observation. (Perceived Truth)
*   Raw output from a dispersion model (contours) in a simulation. (Ground Truth)

<img src="./images/cbrn_hazard.pngkl" width="700px" />

Figure: CBRN Hazard Area Objects.

The `ATP45HazardArea` object represents the hazard region as calculated by a warning and reporting model. Currently, only a subset of the ATP-45 definition is used.

The `ProbabilityHazardContourGroup` object provides a representation of the hazard area of the casualty effects of this CBRN hazard, calculated from the properties of the particular material.

The `RawDataHazardContourGroup` object provides a representation of the raw dispersion output, which is purely the quantity of the material calculated by a dispersion model.






### CBRN Effects 

Previous systems utilising HLA as the interoperability mechanism have used representations of damage from existing object models for representing kinetic weapons to represent CBRN exposure. The representation of damage from kinetic models does not necessarily map well to represent CBRN exposure.

This section presents the use cases and data types for CBRN casualty modelling.

### Lifeform CBRN Effects Modelling

The `CBRN_Human` object is an extension of the `NETN_Human` object from the NETN Physical FOM module. The CBRN federate will update the appropriate attributes as the casualty state of the entity changes. The TriageLevel uses the NATO representation of triage category scores. The IPEType attribute denotes the level of Individual Protective Equipment (IPE) that the unit is wearing. These levels are those defined by the Nuclear, Biological and Chemical (NBC) dress states.
 
<img src="./images/cbrn_human.pngc" width="400px%"/>

Figure: CBRN_Human Object.


The `CBRN_Casualty` interaction is provided to give support to legacy systems that do not use the CBRN FOM module. A translation federate can then map the `CBRN_Casualty` interaction into a data type that the legacy system can handle.

<img src="./images/cbrn_casualty.pngc" width="350px"/>

Figure: CBRN_Casualty Interaction.

The triage levels used in the `CBRNDamageEnum8` enumeration uses the ‘T system’ to denote the priority of treatment for casualties where the levels are defined as:
*   T3 – Delayed priority.
*   T2 – Urgent priority.
*   T1 – Immediate priority.
*   T4 – Expectant priority (treatment would be ineffective).

## CBRN Platforms 

To calculate further contamination and exposure for a platform, extensions are made to seven NETN leaf nodes in the NETN Physical FOM module. These are all extensions of the Platform object from the RPR Physical FOM module. Each extension contains an array of the mass of all materials that are contaminating the vehicle.

<img src="./images/cbrn_platform.pngc" />

Figure: CBRN Platform Objects.


Using the same pattern as that used for the `CBRN_Human` object, the contaminating mass inside a platform due to embedded units can be updated by an external federate using the `CBRN_PlatformUpdate` interaction.
 
 <img src="./images/cbrn_platformupdate.pngc" width="350px" />

Figure: CBRN_PlatformUpdate Interaction.

## Protective Measures Modelling

The use case for protective measures covers both the modelling of CBRN treatment and the modelling of CBRN protective equipment. This includes both individual (i.e. a respirator) and collective protection (i.e. Collective Protection (COLPRO)).

The `COLPRO` and `DecontaminationStation` object classes extends the `NETN_CulturalFeature` object class and provides information on how many entities it can contain as well as what agents it provides protection against.
 
<img src="./images/cbrn_protectobjects.pngc" width="700px" />

Figure: CBRN Protective Measures Objects.

### Protective Measures Interactions

<img src="./images/cbrn_protectinteractions.pngc" width="800px" />

Figure: CBRN Protective Measures Interactions.

The `CBRN_FacilityUpdate` interactions allows entities to be instructed to enter or leave a CBRN facility.

* The `COLPROUpdate` interaction extends the CBRN_FacilityUpdate and allows entities to be instructed to enter or leave a COLPRO object.
* The `DecontaminationStationUpdate` interaction extends the CBRNFacilityUpdate and allows entities to be instructed to enter or leave a DecontaminationStation object.

The `IPECommand` interaction allows entities to be instructed to don IPE by an external federate.

The `CBRN_TreatmentCommand` can be used for both pre-mission countermeasures and for treatment to be applied post-exposure. The `CBRN_Human` object has an attribute which denotes the treatments that have been applied to the unit.
 



 



### CBRN Detection

```mermaid
sequenceDiagram
autonumber
Simulation->>CBRN Agent:Release
Simulation->>CBRN Effect: Entities update
CBRN Agent->>CBRN Effect: CBRN Agents' update
Simulation->>CBRN Effect: Decontamination
CBRN Effect->>Simulation: Effects
```


```mermaid

sequenceDiagram
autonumber
Simulation->>CBRN Agent:Release
Simulation->>CBRN Detector: Detector Object
CBRN Detector->>CBRN Sensor: Sensor Object
CBRN Agent->>CBRN Sensor: CBRN Agents' update
CBRN Sensor->>CBRN Detector: SensorUpdate
CBRN Detector->>Simulation: DetectorAlert
```

Modelling the detection of CBRN agents in a federated simulation environment can use three different design approaches. 

1. Simulation of Either the detection modelling is handled by same federate that models the CBRN sensors or these are sparated in two 

```mermaid
sequenceDiagram
autonumber
Sim->>Detector: CBRN_Detector
Detector->>Sim:DetectorAlarm
Detector->>Sensor: CBRN_Sensor
Sensor->>Detector:SensorUpdate
```




1. Sim publish Detector, Detector modelled by CBRN federate and sends DetectorAlarms

2. Sim models detector, Sensor modelled by CBRN federate that produce sensor updates


There are two use cases for a detector in an HLA simulation:
1.  The first is for detector properties to be created and published by a simulation federate. A detector model within a CBRN federate would calculate the detector readings and publish the alarm status. This process would use the CBRN_Detector object and CBRN_DetectorAlarm interaction.
2.  The second would have a complete detector model in a federate; this would request concentration readings from a CBRN federate and calculate its alarm status. This would use the CBRN_Sensor object and CBRN_SensorUpdate interaction.


Both the `CBRN_Detector` and `CBRN_Sensor` objects extend the BaseEntity.PhysicalEntity.Sensor object in the RPR Physical FOM module.
 
### CBRN_Detector

The `CBRN_Detector` object is used when a CGF creates a detector with a CBRN federate performing the modelling of the detector. When the federate controlling the detector calculates a concentration above a threshold, it will issue a `CBRN_DetectorAlarm` interaction.

### CBRN_Sensor

The `CBRN_Sensor` object is used when there is a detector federate performing the modelling of the detector. The `CBRN_Sensor` registers the agents that it is interested in and receives readings in the form of a `CBRN_SensorUpdate` interaction.
 



## Object Classes

Note that inherited and dependency attributes are not included in the description of object classes.

```mermaid
graph RL
BaseEntity-->HLAobjectRoot
DIM_HazardRegion-->HLAobjectRoot
PhysicalEntity-->BaseEntity
Lifeform-->PhysicalEntity
Sensor-->PhysicalEntity
Platform-->PhysicalEntity
CulturalFeature-->PhysicalEntity
Human-->Lifeform
CBRN_Detector-->Sensor
CBRN_Sensor-->Sensor
COLPRO-->CulturalFeature
DecontaminationStation-->CulturalFeature
CBRN_Hazard-->DIM_HazardRegion
```

### Human

A human lifeform.

|Attribute|Datatype|Semantics|
|---|---|---|
|Exposures|ArrayOfCBRNExposureStruct|Array of agents to which this entity has been exposed to. Defaults to an empty array.|
|Treatments|ArrayOfTreatmentStruct|The types of treatment that this entity has used. Defaults to an empty array.|
|TriageLevel|CBRNDamageEnum8|Triage level of this entity. Defaults to Uninjured.|
|IPEType|IPETypeEnum8|Type of IPE that the entity is using. Defaults to None.|

### CBRN_Detector

Represents a CBRN detector. This object is used to pass information to a CBRN federate, which will model the detector internally and publish a CBRN_DetectorAlarm.

|Attribute|Datatype|Semantics|
|---|---|---|
|ProcessingTime|TimeSecondInteger32|Duration between the detector being exposed to a concentration of agent above its threshold and the detector raising an alarm.|
|AveragingTime|TimeSecondInteger32|Duration the detector will collect samples.|
|DetectableAgents|ArrayOfAgentConcentrationStruct|Array of detectable agents and their thresholds.|
|Alarm|CBRNAlarmStruct|Data representing the alarm of this detector. Defaults to no alarm if the attribute is not set.|

### CBRN_Sensor

Represents a CBRN sensor. This object is used to pass information to a CBRN federate, which will return sensor readings by publishing CBRN_SensorUpdate interactions.

|Attribute|Datatype|Semantics|
|---|---|---|
|UpdateFrequency|TimeSecondInteger32|Duration that this sensor would like between updated readings.|
|AveragingTime|TimeSecondInteger32|Duration the sensor will collect samples.|
|DetectableAgents|ArrayOfAgentTypeEnum|Array of agents that this sensor wishes to detect.|
|SensorReadings|ArrayOfCBRNSensorReadingStruct|Latest sensor readings. Defaults to no readings if this attribute is not set.|

### Platform

A physical object under the control of armed forces upon which sensor, communication, or weapon systems may be mounted.

|Attribute|Datatype|Semantics|
|---|---|---|
|Contamination|ArrayOfAgentMassStruct|CBRN hazardous agent inside vehicle due to embedded units.|

### COLPRO

Represents a feature that provides Collective Protection (COLPRO) against a CBRN threat.

|Attribute|Datatype|Semantics|
|---|---|---|
|Capacity|QuantityInt32|The number of entities that this COLPRO can handle.|
|Protection|ArrayOfProtectionEffectivenessStruct|The effectiveness that this COLPRO offers for each agent.|

### DecontaminationStation

Represents a feature that provides treatment for CBRN exposure.

|Attribute|Datatype|Semantics|
|---|---|---|
|Capacity|QuantityInt32|The number of entities that this decontamination station can handle.|
|DecontaminationPeriod|TimeSecondInteger32|Duration it takes to decontaminate an entity.|
|Treatments|ArrayOfTreatmentStruct|The types of treatment that this decontamination station currently offers.|

### CBRN_Hazard

A generic representation of a CBRN Hazard.

|Attribute|Datatype|Semantics|
|---|---|---|
|AgentClass|AgentClassEnum8|Required: The class of the agent.|
|AgentType|AgentTypeEnum16|Requierd: Agent reflected in this contour.|
|EffectType|ExposureEffectTypeEnum16|The type of effects seen following an exposure.|
|Contours|ArrayOfContourStruct|Optional: Array of contours. These should be ordered in a sequence of ascending PercentProbabilityLevel.|

## Interaction Classes

Note that inherited and dependency parameters are not included in the description of interaction classes.

```mermaid
graph RL
CBRN_Interaction-->HLAinteractionRoot
ETR_Task-->HLAinteractionRoot
Release-->CBRN_Interaction
PlatformUpdate-->CBRN_Interaction
CasualtyUpdate-->CBRN_Interaction
SensorUpdate-->CBRN_Interaction
DetectorAlarm-->CBRN_Interaction
RequestTask-->ETR_Task
ApplyIPE-->RequestTask
AdministerTreatment-->RequestTask
```

### Release

Communicates information associated with the release of hazardous agent.

|Parameter|Datatype|Semantics|
|---|---|---|
|Agent|AgentTypeEnum16|The type of released CBRN hazardous agent.|
|Location|LocationStruct|Initial location of the release in the geocentric location system.|
|Mass|MassKilogramFloat32|Total released agent mass in kg.|
|Duration|TimeSecondInteger32|Duration the release takes place.|
|ReleaseSize|ReleaseSizeStruct|The initial size of the release including initial Gaussian sigmas of the released puff and mean & variance of released particles.|
|ReleaseDynamics|ReleaseDynamicsStruct|Temperature differance and density ratio of released material relative to the atmosphere.|
|ReleaseVelocity|VelocityVectorStruct|Velocity of the source term.|

### PlatformUpdate

Represents an update to the contaminating mass inside a vehicle due to embedded entities.

|Parameter|Datatype|Semantics|
|---|---|---|
|Platform|UUID|The unique ID of the platform.|
|Contamination|ArrayOfAgentMassStruct|New state of CBRN hazardous agent inside vehicle due to embedded units.|

### CasualtyUpdate

Informs the federate representing the entity of the casualty effects of exposure.

|Parameter|Datatype|Semantics|
|---|---|---|
|Entity|UUID|Required: Reference to the entity affected by CBRN.|
|TriageLevel|CBRNDamageEnum8|Required: Triage level of this entity.|
|Exposures|ArrayOfCBRNExposureStruct|Optional: Array of agents to which this unit has been exposed.|

### SensorUpdate

Sends information about the current state of a previously registered CBRN sensor.

|Parameter|Datatype|Semantics|
|---|---|---|
|Sensor|UUID|Required: Unique representation of the sensor’s ID.|
|Concentration|MassConcentrationFloat32|Required: Mean Concentration in kg m-3.|
|Agent|AgentTypeEnum16|Required: Type of the agent.|

### DetectorAlarm

Represents the alarm trigger of a previously registered CBRN detector.

|Parameter|Datatype|Semantics|
|---|---|---|
|Agent|AgentTypeEnum16|Enumeration representation of the agent.|
|Location|LocationStruct|Required: Location of the detection (location of Detector)|
|DetectorId|UUID|Optional: Reference to detector object.|

### ApplyIPE

Represents an task for the specified entities to use individual protective equipment.

|Parameter|Datatype|Semantics|
|---|---|---|
|TaskParameters|ApplyIPETaskStruct|Required: Task parameters.|

### AdministerTreatment

Represents an order for the specified entities to receive the list of treatments.

|Parameter|Datatype|Semantics|
|---|---|---|
|TaskParameters|AdministerTreatmentTaskStruct|Required: Task parameters.|

## Datatypes

Note that only datatypes defined in this FOM Module are listed below. Please refer to FOM Modules on which this module depends for other referenced datatypes.

### Overview
|Name|Semantics|
|---|---|
|ATP45HazardAreaTypeEnum8|Type of ATP-45 Hazard Area (simple or detailed).|
|AdministerTreatmentTaskStruct|Task-specific data.|
|AgentClassEnum8|Class of Agent for an ATP-45 Hazard Area.|
|AgentConcentrationStruct|Concentration value associated with a specific agent.|
|AgentMassStruct|Mass of contaminant inside a vehicle brought in by an embedded unit.|
|AgentTypeEnum16|Type of CBRN hazardous agent.|
|ApplyIPETaskStruct|Task-specific data.|
|ArrayOfAgentConcentrationStruct|Array of agents and their concentrations.|
|ArrayOfAgentMassStruct|Array of agents and their masses.|
|ArrayOfAgentTypeEnum|Array of agents.|
|ArrayOfCBRNExposureStruct|Array of agents and their dosages.|
|ArrayOfCBRNSensorReadingStruct|Array of sensor readings.|
|ArrayOfContourStruct|A set of contour boundaries defining concentration levels in ascending order.|
|ArrayOfProbabilityHazardContourStruct|Array of Probability Hazard Contours.|
|ArrayOfProtectionEffectivenessStruct|Array of the protection’s effectiveness.|
|ArrayOfRawDataHazardContourStruct|Array of Raw Data Hazard Contours.|
|ArrayOfSigmas6|Size of the initial Gaussian puff sigma values.|
|ArrayOfTreatmentStruct|Array of TreatmentStruct types.|
|CBRNAlarmStruct|Properties of a CBRN alarm.|
|CBRNDamageEnum8|Level of damage due to CBRN exposure.|
|CBRNExposureStruct|Dosage exposure value associated with a specific agent.|
|CBRNSensorReadingStruct|Timed CBRN sensor reading.|
|ContourStruct|Countour description.|
|DensityRatioFloat32|Ratio of density of two materials in range [0, 1].|
|DosageKgSecondPerMeterCubedFloat32|Dosage in SI units.|
|ExposureEffectTypeEnum16|The type of CBRN agent will define the type of effects seen following an exposure, these are: Intoxication: This is due to a chemical (and toxin) exposure. Infection: This is due to a live biological agent exposure. Irradiation: This is due to ionising radiation exposure. Injuries: This is due to exposure to trauma or climatic stress (heat) either in isolation or as a combined injury (CBRN and trauma). The delivery method of the CBRN agent may cause trauma as well as physical degradation from individual protective equipment (IPE) use (heat injury).|
|ExposureFloat32|Data type for exposure.|
|ExposureTypeEnum8|Type of exposure represented in a contour group.|
|GeocentricPolygon|A polygon defined as a closed polygonal chain of geocentric world locations where the last location is connected to the first.|
|HazardTypeEnum8|Type of dispersion output represented in a contour group.|
|IPETypeEnum8|Types of Individual Protective Equipment.|
|MeanMetersFloat32|Mean of a Gaussian distribution, based on SI unit meter, unit symbol m.|
|ProbabilityHazardContourStruct|Represents the footprint covered by a CBRN hazard. This object covers the agent-specific effects which would be the output from a casualty model.|
|ProtectionEffectivenessStruct|Protection effectiveness associated with a specific agent.|
|RawDataHazardContourStruct|Contour locations bounding a given exposure value.|
|ReleaseDistributionStruct|Mean and variance of the distribution of the particles or droplets in a release.|
|ReleaseDynamicsStruct|Defines the dynamic properties of a release.|
|ReleaseSizeStruct|Defines the properties of the initial size of a release.|
|TaskDefinitionVariantRecord|Variant record for task definition data.|
|TaskProgressVariantRecord|Variant record for task progress data.|
|TaskTypeEnum|Task types.|
|TreatmentStruct|Defines the properties for a CBRN treatment.|
|VarianceMetersSquaredFloat32|Variance of a Gaussian distribution, based on SI unit meter squared, unit symbol m2.|
        
### Simple Datatypes
|Name|Units|Semantics|
|---|---|---|
|DensityRatioFloat32|NA|Ratio of density of two materials in range [0, 1].|
|DosageKgSecondPerMeterCubedFloat32|Kg Second Per Meter Cubed|Dosage in SI units.|
|ExposureFloat32|NA|Data type for exposure.|
|MeanMetersFloat32|meters|Mean of a Gaussian distribution, based on SI unit meter, unit symbol m.|
|VarianceMetersSquaredFloat32|meters squared|Variance of a Gaussian distribution, based on SI unit meter squared, unit symbol m2.|
        
### Enumerated Datatypes
|Name|Representation|Semantics|
|---|---|---|
|ATP45HazardAreaTypeEnum8|HLAoctet|Type of ATP-45 Hazard Area (simple or detailed).|
|AgentClassEnum8|HLAoctet|Class of Agent for an ATP-45 Hazard Area.|
|AgentTypeEnum16|HLAinteger16BE|Type of CBRN hazardous agent.|
|CBRNDamageEnum8|HLAoctet|Level of damage due to CBRN exposure.|
|ExposureEffectTypeEnum16|HLAinteger16BE|The type of CBRN agent will define the type of effects seen following an exposure, these are: Intoxication: This is due to a chemical (and toxin) exposure. Infection: This is due to a live biological agent exposure. Irradiation: This is due to ionising radiation exposure. Injuries: This is due to exposure to trauma or climatic stress (heat) either in isolation or as a combined injury (CBRN and trauma). The delivery method of the CBRN agent may cause trauma as well as physical degradation from individual protective equipment (IPE) use (heat injury).|
|ExposureTypeEnum8|HLAoctet|Type of exposure represented in a contour group.|
|HazardTypeEnum8|HLAoctet|Type of dispersion output represented in a contour group.|
|IPETypeEnum8|HLAoctet|Types of Individual Protective Equipment.|
|TaskTypeEnum|HLAinteger32BE|Task types.|
        
### Array Datatypes
|Name|Element Datatype|Semantics|
|---|---|---|
|ArrayOfAgentConcentrationStruct|AgentConcentrationStruct|Array of agents and their concentrations.|
|ArrayOfAgentMassStruct|AgentMassStruct|Array of agents and their masses.|
|ArrayOfAgentTypeEnum|AgentTypeEnum16|Array of agents.|
|ArrayOfCBRNExposureStruct|CBRNExposureStruct|Array of agents and their dosages.|
|ArrayOfCBRNSensorReadingStruct|CBRNSensorReadingStruct|Array of sensor readings.|
|ArrayOfContourStruct|ContourStruct|A set of contour boundaries defining concentration levels in ascending order.|
|ArrayOfProbabilityHazardContourStruct|ProbabilityHazardContourStruct|Array of Probability Hazard Contours.|
|ArrayOfProtectionEffectivenessStruct|ProtectionEffectivenessStruct|Array of the protection’s effectiveness.|
|ArrayOfRawDataHazardContourStruct|RawDataHazardContourStruct|Array of Raw Data Hazard Contours.|
|ArrayOfSigmas6|LengthMeterFloat32|Size of the initial Gaussian puff sigma values.|
|ArrayOfTreatmentStruct|TreatmentStruct|Array of TreatmentStruct types.|
|GeocentricPolygon|LocationStruct|A polygon defined as a closed polygonal chain of geocentric world locations where the last location is connected to the first.|
        
### Fixed Record Datatypes
|Name|Fields|Semantics|
|---|---|---|
|AdministerTreatmentTaskStruct|Entities, Treatment, Duration|Task-specific data.|
|AgentConcentrationStruct|MeanConcentration, Agent|Concentration value associated with a specific agent.|
|AgentMassStruct|ContaminatedMass, Agent|Mass of contaminant inside a vehicle brought in by an embedded unit.|
|ApplyIPETaskStruct|IPEType|Task-specific data.|
|CBRNAlarmStruct|Location, Agent|Properties of a CBRN alarm.|
|CBRNExposureStruct|Exposure, Agent|Dosage exposure value associated with a specific agent.|
|CBRNSensorReadingStruct|Time, Reading|Timed CBRN sensor reading.|
|ContourStruct|Boundary, ConcentrationLevel|Countour description.|
|ProbabilityHazardContourStruct|PercentProbabilityLevel, Locations|Represents the footprint covered by a CBRN hazard. This object covers the agent-specific effects which would be the output from a casualty model.|
|ProtectionEffectivenessStruct|Effectiveness, Agent|Protection effectiveness associated with a specific agent.|
|RawDataHazardContourStruct|ExposureLevel, Locations|Contour locations bounding a given exposure value.|
|ReleaseDistributionStruct|Mean, Variance|Mean and variance of the distribution of the particles or droplets in a release.|
|ReleaseDynamicsStruct|DensityRatio, TemperatureDifference|Defines the dynamic properties of a release.|
|ReleaseSizeStruct|SigmaArray, ReleaseDistribution|Defines the properties of the initial size of a release.|
|TreatmentStruct|Effectiveness, Duration, TreatmentWindow, TreatableAgents|Defines the properties for a CBRN treatment.|
        
### Variant Record Datatypes
|Name|Discriminant (Datatype)|Alternatives|Semantics|
|---|---|---|---|
|TaskDefinitionVariantRecord|TaskType (TaskTypeEnum)|ApplyIPE, AdministerTreatment|Variant record for task definition data.|
|TaskProgressVariantRecord|TaskType (TaskTypeEnum)|Treatment|Variant record for task progress data.|
    