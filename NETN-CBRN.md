# NETN-CBRN

Copyright (C) 2019 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENSE.md).

## Introduction

This section provides the data definitions for CBRN FOM module within the NETN FOM. These data types will represent those needed to perform CBRN dispersion calculations (with the notable exception of meteorological information) and the dissemination of information about the dispersion effects on entities and the environment.

### Purpose

### Scope

Note that meteorological information and CBRN material properties are not considered in this document. Meteorology is a wide and complex field that has requirements in many different modelling fields, not just CBRN. For that reason it is recommended that it has its own FOM module. Different dispersion models require different material properties; therefore it is better left to the dispersion model to handle these material properties.

### Overview

The CBRN FOM module consists of five sections:
a.	CBRN Source release modelling;
b.	Detector modelling;
c.	CBRN effects modelling;
d.	Protective measures modelling; and
e.	Hazard area information.

Whilst designing this CBRN FOM module considerations have been taken as to how the CBRN FOM module can be used by legacy systems that cannot be updated to implement the CBRN FOM module. These considerations have resulted in the CBRN FOM module including interactions that mirror the changes that can be made to objects. These interactions allow for a translation federate to sit between the CBRN and simulation federate to translate the CBRN data into data that the simulation federate can understand.

## CBRN SOURCE RELEASE MODELLING

### CBRN_Release

The CBRN_Release interaction defines all of the CBRN source release information required for a CBRN federate to model a CBRN source release.
 
Figure 10-1: CBRN_Release Interaction.

### CBRN Warheads on Conventional Munitions

Ideally the CBRN_Release interaction would be sent from a simulation federate to the CBRN federate to inform it that a release had occurred, allowing the CBRN federate to model the release. However if a legacy simulation federate that can’t be updated to use the CBRN FOM module is being used, then the MunitionDetonation from the RPR FOM can be used instead. In this situation an additional federate will be created that can subscribe to the MunitionDetonation interactions and use the WarheadType parameter to check that the MunitionDetonation is a CBRN release. If it is a CBRN release then the additional federate will create a CBRN_Release using the WarheadType parameter, and publish the CBRN_Release interaction allowing the CBRN federate to start its CBRN modelling.

## DETECTOR MODELLING

There are two use cases for a detector in a HLA simulation:
a.	The first is for detector properties to be created and published by a simulation federate. A detector model within a CBRN federate would calculate the detector readings and publish the alarm status. This process would use the CBRN_Detector object and CBRN_DetectorAlarm interaction.
b.	The second would have a complete detector model in a federate; this would request concentration readings from a CBRN federate and calculate its own alarm status. This would use the CBRN_Sensor object and CBRN_SensorUpdate interaction.

Both the CBRN_Detector and CBRN_Sensor objects extend the BaseEntity.PhysicalEntity.Sensor object in the RPR Physical FOM module.
 
### CBRN_Detector

The CBRN_Detector object is used when a CGF creates a detector with a CBRN federate performing the modelling of the detector. When the federate controlling the detector calculates a concentration above a threshold, it will issue a CBRN_DetectorAlarm interaction.

### CBRN_Sensor

The CBRN_Sensor object is used when there is a detector federate performing the modelling of the detector. The CBRN_Sensor registers the agents that it is interested in and receives readings in the form of a CBRN_SensorUpdate interaction.
 
Figure 10-2: CBRN Detector Objects.
 
Figure 10-3: CBRN Detector Interactions.

## CBRN EFFECTS MODELLING

Previous systems utilising HLA as the interoperability mechanism have used representations of damage from existing object models for representing kinetic weapons in order to represent CBRN exposure. The representation of damage from kinetic models does not necessarily map well to represent CBRN exposure.

This section presents the use cases and data types for CBRN casualty modelling.

### Lifeform CBRN Effects Modelling

#### CBRN_Human 

The CBRN_Human object is an extension of the NETN_Human object from the NETN Physical FOM module. The CBRN federate will update the appropriate attributes as the casualty state of the entity changes. The TriageLevel uses the NATO representation of triage category scores [1]. The IPEType attribute denotes the level of Individual Protective Equipment (IPE) that the unit is wearing. These levels are those defined by the Nuclear, Biological and Chemical (NBC) dress states [2].
 
Figure 10-4: CBRN_Human Object.

#### CBRN_Casualty 

The CBRN_Casualty interaction is provided to give support to legacy systems that do not use the CBRN FOM module. A translation federate can then map the CBRN_Casualty interaction into a data type that the legacy system can use.

#### CBRNDamageEnum8 

The triage levels used in the CBRNDamageEnum8 enumeration uses the ‘T system’ to denote the priority of treatment for casualties where the levels are defined as:
a.	T3 – Delayed priority.
b.	T2 – Urgent priority.
c.	T1 – Immediate priority.
d.	T4 – Expectant priority (treatment would be ineffective).
 
Figure 10-5: CBRN_Casualty Interaction.

### Platform CBRN Effects Modelling

#### CBRN Platforms 

In order to calculate further contamination and exposure for a platform, extensions are made to seven NETN leaf nodes in the NETN Physical FOM module. These are all extensions of the Platform object from the RPR Physical FOM module. Each extension contains an array of the mass of all materials that are contaminating the vehicle.
 
Figure 10-6: CBRN Platform Objects.

#### CBRN_PlatformUpdate 

Using the same pattern as that used for the CBRN_Human object, the contaminating mass inside a platform due to embedded units can be updated by an external federate using the CBRN_PlatformUpdate interaction.
 
Figure 10-7: CBRN_PlatformUpdate Interaction.

## PROTECTIVE MEASURES MODELLING

The use case for protective measures covers both the modelling of CBRN treatment and the modelling of CBRN protective equipment. This includes both individual (i.e. a respirator) and collective protection (i.e. Collective Protection (COLPRO)).
 
### COLPRO

The COLPRO object extends the BaseEntity.PhysicalEntity.CulturalFeature.NETN_CulturalFeature and provides information on how many entities it can contain as well as what agents it provides protection against.

###	DecontaminationStation

The DecontaminationStation object also extends the BaseEntity.PhysicalEntity.CulturalFeature.NETN_CulturalFeature and provides information on how many entities it can contain as well as what agents it can provide decontamination for.
 
Figure 10-8: CBRN Protective Measures Objects.

### CBRN_FacilityUpdate

The CBRN_FacilityUpdate interaction is a base interaction that allows entities to be instructed to enter or leave a CBRN facility, it is not possible to publish or subscribe to this interaction.

### COLPROUpdate

The COLPROUpdate interaction extends the CBRN_FacilityUpdate and allows entities to be instructed to enter or leave a COLPRO object.

### DecontaminationStationUpdate

The DecontaminationStationUpdate interaction extends the CBRNFacilityUpdate and allows entities to be instructed to enter or leave a DecontaminationStation object.
 
### IPECommand

The IPECommand interaction allows entities to be instructed to don IPE by an external federate.

### CBRN_TreatmentCommand

The CBRN_TreatmentCommand can be used for both pre-mission countermeasures and for treatment to be applied post-exposure. The CBRN_Human object has an attribute which denotes the treatments that have been applied to the unit.
 
Figure 10-9: CBRN Protective Measures Interactions.

## HAZARD AREA MODELLING

The modelling of hazard areas allows the representation of a contamination area to be sent to a simulation federate in the simplest possible form. This could be in the form of:
a.	The output from a hazard prediction algorithm (a warning area defined in Allied Tactical Publication (ATP)-45) [3] in response to a detector alarm or observation.
b.	Raw output from a dispersion model (contours) during a simulation run.

### ATP45HazardArea

The ATP45HazardArea object represents the hazard region as calculated by a warning and reporting model. Currently only a subset of the ATP-45 definition is used.

### ProbabilityHazardContourGroup

The ProbabilityHazardContourGroup object provides a representation of the hazard area of the casualty effects of this CBRN hazard, calculated from the properties of the particular material.

### RawDataHazardContourGroup
The RawDataHazardContourGroup object provides a representation of the raw dispersion output which is purely the quantity of the material calculated by a dispersion model.
 
Figure 10-10: CBRN Hazard Area Objects.
