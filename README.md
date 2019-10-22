# NETN-CBRN
NATO Education and Training Network (NETN) Chemical, Biological, Radiological, Nuclear (CBRN) Module

## Introduction
CBRN are Chemical, Biological, Radiological and Nuclear materials that can be delivered intentially as a weapon using conventional bombs, explosive materials and enhanced blast weapons (e.g., dirty bombs) or unintentionally caused by human error or natural or technological
reasons, such as spills, accidental releases or leakages. 

The NATO Education and Training Network CBRN Module (NETN-CBRN) is a specification of how to model CBRN releated concepts in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module is used to specify how data is represented and exchanged in the federation. The NETN-CBRN FOM module is available as an XML file for use in HLA based federations.

### Purpose

NETN-CBRN provides a common standard interface for representation of CBRN release, detection, effects, and protective measueres in a federated distributed simulation. E.g the expoure effect on individual humans in a CBRN contaminated Hazard area where the human is represented in one simulation and the effect is modelled in another federate simulation.

### Scope

The NETN-CBRN FOM module covers:

1.	CBRN Source release modelling
2.	CBRN Detector modelling
3.	CBRN Effects modelling
4.	CBRN Protective measures modelling
5.	Hazard area modelling

Meteorological conditions and CBRN material properties for modelling the dispersion of CBRN material are not explicitly represented in the NETN-CBRN FOM Module. NETN-METOC FOM module can be used to model wether conditions that may impact the dispersion of CBRN materials and cause dynmaic change to hazard areas.

## License

Copyright (C) 2019 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md). 

The work includes the [NETN-CBRN.xml](CBRN_v1.1.9.xml FOM Module.

Above license gives you the right to use and redistribute the NETN FOM Module (XML file) in its entirety without modification. You are also allowed to develop your own new FOM Modules (in separate XML files and separate documentation) that build-on/extends the NETN module by reference and including neccessary scaffolding classes. You are NOT allowed to modify this FOM Module without prior permission by the NATO Modelling and Simulation Group. 

## Versions, updates and extentions

All updates and versioning of this work is coordinated by the NATO Modelleing and Simulation Coordination Office (MSCO), managed by the NATO Modelling and Simulation Group (NMSG) and performed as NATO Science and Technology Organization (STO) technical activities in support of the NMSG Modelling and Simulation Standards Subgroup (MS3).

Feedback on the use of this work, suggestions for improvements and identified issues are welcome and can be provided using GitGub issue tracking. To engage in the development and update of this FOM Module please contact your national NMSG representative.

Version numbering of this FOM Module and associated documentation is based on the following principles:

* New official version number is assigned and in effect only when new release is made in the Master branch.
* Updates in the Develop branch will not change version number.
* In the FOM Module useHistory information include only information on official releases.
* Update of the major version number is made if the change constitute a major restructuring, merging, addition or redefinition of semantics that breaks backward compatibility or cover entirely new concepts.
* Update of the minor version number is made if the change constitute a minor additions to existing concepts and editorial changes that do not break backward compatibility but may require updates of software to use new concepts.
* Patches are released to fix minor issues that do not break backward compatibility.

|Version|Description|
|---|---|
| v 1.1.9 | Iitial public release of the NETN-CBRN FOM Module developed by MSG-106 & MSG-134 and included in AMSP-04 Ed A. |

## Documentation

[Full Documentation](NETN-CBRN.md)
