# Calcium Imaging Element

## Description of modality, user population 
Over the past two decades, in vivo two-photon  laser-scanning imaging of calcium signals 
has evolved into a mainstream modality for neurophysiology experiments to record population activity in intact neural circuits. 
The tools for signal acquisition and analysis continue to evolve but common patterns and elements of standardization have emerged.

## Acquisition tools

### Hardware 
The primary acquisition systems are: 
+ Sutter (we estimate 400 rigs in active use - TBC)
+ Thorlabs  (we estimate 400 rigs in active use - TBC)
+ Bruker  (we estimate 400 rigs in active use - TBC)
+ Neurolabware (we estimate 400 rigs in active use - TBC)

We do not include Miniscopes in these estimates. 
In all there are perhaps on the order of 3000 two-photon setups globally but their processing needs may need to be further segmented.

### Software
+ ScanImage
+ ThorImageLS
+ ScanBox

Vidrio’s ScanImage is the data acquisition software for two types of home-built scanning two-photon systems: 
based on Thorlabs and Sutter hardware. ScanImage has a free version and a licensed version. 
They have about 200 active paid licenses. 
Thorlabs also provides their own acquisition software - ThorImageLS (probably half of the systems).

## Preprocessing toolchain: development teams
The preprocessing workflow for two-photon laser-scanning microscopy includes 
motion correction (rigid or non-rigid), cell segmentation, and calcium event extraction 
(sometimes described as "deconvolution" or "spike inference"). 
Some include raster artifact correction, cropping and stitching operations. 

Until recently, most labs have developed custom processing pipelines, sharing them with others as academic open-source projects. 
Recently, a few leaders have emerged as standardization candidates for the initial preprocessing.

+ [CaImAn](https://github.com/flatironinstitute/CaImAn) (Originally developed by Andrea Giovannucci, current support by FlatIron Institute: Eftychios A. Pnevmatikakis, Johannes Friedrich)
+ [Suite2p](https://github.com/MouseLand/suite2p) (Carsen Stringer and Marius Pachitariu at Janelia), 200+ users, active support

## Precursor projects and interviews
Over the past few years, several labs have developed DataJoint-based data management and processing pipelines for two-photon Calcium imaging. 
Our team collaborated with several of them during their projects. 
Additionally, we interviewed these teams to understand their experiment workflow, pipeline design, associated tools, and interfaces. 

These teams include:
+ MICrONS (Andreas Tolias Lab, BCM) - https://github.com/cajal
+ BrainCoGs (Princeton) - https://github.com/BrainCOGS
+ Moser Group (Kavli Institute/NTNU) - private repository

## Pipeline Development
Through our interviews and direct collaboration on the precursor projects, 
we identified the common motifs to create the Calcium ImagingElement 
with the repository hosted at https://github.com/datajoint/elements-imaging.

Major features of the Calcium Imaging Element include:
+ Pipeline architecture defining:
    + Calcium-imaging scanning metadata, also compatible with mesoscale imaging and multi-ROI scanning mode
    + Tables for all processing steps: motion correction, segmentation, cell spatial footprint, fluorescence trace extraction, spike inference and cell classification
    + Store/track/manage different curations of the segmentation results
+ Ingestion support for data acquired with ScanImage and ScanBox acquisition systems
+ Ingestion support for processing outputs from both Suite2p and CaImAn analysis suites
+ Sample data and complete test suite for quality assurance

The processing workflow is typically performed on a per-scan basis, 
however, depending on the nature of the research questions, 
different labs may opt to perform processing/segmentation on a concatenated set of data from multiple scans. 
To this end, we have extended the Calcium Imaging Element and provided a design version capable of supporting a multi-scan processing scheme.

## Alpha release: Validation sites
+ Anne Churchland Lab (UCLA) - Joao Couto, James Roach
    + The lab uses ScanBox software for data acquisition, and data processing is performed with Suite2p package
    + Multiple scans are collected per session, and the processing are done on the concatenated dataset from several scans - we have extended the Calcium Imaging Element and provided a design version to support this multi-scan processing scheme
    + The lab uses local institutional resource for data infrastructure and hosting
    + Repository: (private repository)
+ BrainCoGs (Princeton) - Efthymia (Mika) Diamanti, Alvaro Luna  
    + The lab collects mesoscale imaging data using ScanImage acquisition software and Suite2p for data processing steps. A DataJoint pipeline for animal, lab and behavior management has been developed as part of the U19. A MATLAB-based DataJoint pipeline for Calcium imaging has also been developed previously, supporting analysis routines built in-house. The lab is in need of a parallel pipeline supporting Suite2p, an ideal opportunity to adopt the Calcium Imaging Element.
    + In a training workshop conducted by DataJoint NEURO on 3/01- 03. The Calcium Imaging Element was adopted, connected to an existing BrainCoGs project pipeline. The ingestion routine was successfully validated on the existing dataset (acquired with ScanImage) and Suite2p outputs.
    + The lab uses local institutional resource for data infrastructure and hosting
    + Repository: https://github.com/BrainCOGS/U19-pipeline_python

## Beta release
As the validation progresses, we expect to produce a beta version of the workflow for users to adopt independently by May 1, 2021.