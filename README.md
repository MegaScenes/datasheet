# Datasheet for MegaScenes

Questions from the [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) paper, v7.

Jump to section:

- [Motivation](#motivation)
- [Composition](#composition)
- [Collection process](#collection-process)
- [Preprocessing/cleaning/labeling](#preprocessingcleaninglabeling)
- [Uses](#uses)
- [Distribution](#distribution)
- [Maintenance](#maintenance)

## Motivation

### For what purpose was the dataset created?

MegaScenes was created to provide the research community scene-level, open-licensed data to train 3D-aware models.

### Who created the dataset (e.g., which team, research group) and on behalf of which entity (e.g., company, institution, organization)?

The dataset was curated by researchers at Cornell University in [Prof. Noah Snavely](https://www.cs.cornell.edu/~snavely/)'s group. This dataset was collected for a project in collaboration with researchers at Stanford University and Adobe Research: https://megascenes.github.io/. 

### Who funded the creation of the dataset?

This work was funded in part by the National Science Foundation (IIS-2008313, IIS-2211259, IIS-2212084). Gene Chou was funded by a NSF Graduate Research Fellowship.

## Composition

### What do the instances that comprise the dataset represent (e.g., documents, photos, people, countries)?

MegaScenes comprises of scenes (buildings, landmarks, or natural scenes) distributed around the world. Each scene corresponds to a category from [Wikimedia Commons](https://commons.wikimedia.org); each contains photos (grouped by subategory) and metadata. Certain scenes include point cloud reconstructions.

### How many instances are there in total (of each type, if appropriate)?
The entire dataset consists of around 430K scenes and 8.8M images. Across these scenes, there are 30M image pairs with estimated two-view geometries. A subset of around 80K scenes have at least one sparse point cloud reconstruction; there are around 100K point clouds in total. A subset of 2M images are registered in these point clouds.

### Does the dataset contain all possible instances or is it a sample (not necessarily random) of instances from a larger set?

The scenes in MegaScenes can be considered a sample of scenes around the world. Due to the crowd-sourced nature of the data from [Wikimedia Commons](https://commons.wikimedia.org), scenes are more frequent in Europe and North America compared to other continents. Scenes that represent more well-known landmarks, like tourist attractions, contain more images than less-visited scenes. Photos are taken at various time periods, and thus exhibit diverse lighting, weather, and transient objects.

### What data does each instance consist of?

Each scene consists of internet photos and metadata sourced from [Wikimedia Commons](https://commons.wikimedia.org). This includes subcategory labels for images, as well as a class hierarchy for scenes. Raw [Wikidata](https://www.wikidata.org/) entries for each scene are also provided. Images contain EXIF metadata and other metadata from Wikimedia Commons. Each scene have a COLMAP database, which have SIFT keypoint locations for each image and calculated two-view geometries for image pairs. Certain scenes include point cloud reconstructions in COLMAP format.

### Is there a label or target associated with each instance?

MegaScenes is released such that researchers can create their own subsets of data to train models for their own desired applications. Thus, we have not defined labels or targets.

### Is any information missing from individual instances?

No.

### Are relationships between individual instances made explicit (e.g., users’ movie ratings, social network links)?

We associate scenes and images with a subcategory graph. We associate scenes to broader classes with a class hierarchy.

### Are there recommended data splits (e.g., training, development/validation, testing)?

In our application to single-image novel view synthesis [here](https://github.com/MegaScenes/nvs), we created a train and test set from the MegaScenes dataset. This subset contains image pairs with similar lighting conditions and visual overlap.

### Are there any errors, sources of noise, or redundancies in the dataset?

A potential source of noise when creating data splits is that duplicate images can appear across multiple subategories within a scene. Duplicate images can also appear across scenes. The point clouds are pseudo-ground truth since they are reconstructed via structure-from-motion.

### Is the dataset self-contained, or does it link to or otherwise rely on external resources (e.g., websites, tweets, other datasets)?

MegaScenes is self-contained and hosted on AWS. One exception: some scene metadata from Wikidata may point to external Wikidata entries that are not in the dataset.

### Does the dataset contain data that might be considered confidential (e.g., data that is protected by legal privilege or by doctor-patient confidentiality, data that includes the content of individuals’ non-public communications)?

No. Images and metadata from this dataset are sourced from and publicly available on Wikimedia Commons and Wikidata, with open licenses.

### Does the dataset contain data that, if viewed directly, might be offensive, insulting, threatening, or might otherwise cause anxiety?

No.

### Does the dataset relate to people?

No.


## Collection process

### How was the data associated with each instance acquired?

The images and metadata derive from crowdsourcing efforts of contributors to Wikimedia Commons and Wikidata. We curate a list of scenes by finding all Wikidata entries that relate to several broad classes of structures, then finding the Wikimedia Commons category associated with these Wikidata entries. Each Wikimedia Commons category discovered through this method is considered a scene in MegaScenes. We download the images for each scene, then run structure-from-motion to generate 3D data.

### What mechanisms or procedures were used to collect the data (e.g., hardware apparatus or sensor, manual human curation, software program, software API)?

Image and scene metadata are collected from Wikimedia Commons and Wikidata using download scripts. We use COLMAP to run feature extraction, matching, and mapping.

### If the dataset is a sample from a larger set, what was the sampling strategy (e.g., deterministic, probabilistic with specific sampling probabilities)?
N/A.

### Who was involved in the data collection process (e.g., students, crowdworkers, contractors) and how were they compensated (e.g., how much were crowdworkers paid)?

The authors of this paper are solely involved in the data collection process.

### Over what timeframe was the data collected?

Data was collected from Wikimedia Commons and Wkidata over a period from June 2023 and June 2024.

### Were any ethical review processes conducted (e.g., by an institutional review board)?

No. Wikimedia Commons and Wikidata enforce their own policies on allowable content [here](https://commons.wikimedia.org/wiki/Category:Commons_policies) and [here](https://www.wikidata.org/wiki/Wikidata:List_of_policies_and_guidelines). Since MegaScenes is sourced from these websites, the content of this dataset indirectly inherits these filters. To ensure there is no CSAM, the images in this dataset are also processed with [PhotoDNA](https://www.microsoft.com/en-us/photodna).

### Does the dataset relate to people?

No.

## Preprocessing/cleaning/labeling

### Was any preprocessing/cleaning/labeling of the data done (e.g., discretization or bucketing, tokenization, part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)?

Most images are downsized to a maximum of 1800 pixels in width. The scenes are processed with SIFT feature extraction, vocabulary tree matching, and mapping with COLMAP.

### Was the “raw” data saved in addition to the preprocessed/cleaned/labeled data (e.g., to support unanticipated future uses)?

N/A. 

### Is the software used to preprocess/clean/label the instances available?

For reconstruction, we use [COLMAP](https://colmap.github.io/index.html).

## Uses

_These questions are intended to encourage dataset creators to reflect on the tasks
for which the dataset should and should not be used. By explicitly highlighting these tasks,
dataset creators can help dataset consumers to make informed decisions, thereby avoiding
potential risks or harms._

### Has the dataset been used for any tasks already?

As described in the paper, a subset of MegaScenes has been used to train a single-image novel view synthesis model for scenes.

### Is there a repository that links to any or all papers or systems that use the dataset?

At the moment, we have no plans to create a repository to track papers that use the dataset. Works that cite MegaScenes can be found on sites such as arXiv, Semantic Scholar, and Google Scholar.

### What (other) tasks could the dataset be used for?

We forsee that MegaScenes can be used to train 3D-aware models for pose estimation, feature matching, and reconstruction.

### Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses?

MegaScenes is a snapshot of Wikimedia Commons and Wikidata. In contrast, these websites are continually updated by contributors. Category names and image organization in MegaScenes may differ from Wikimedia Commons as time passes.

### Are there tasks for which the dataset should not be used?

No.

## Distribution

### Will the dataset be distributed to third parties outside of the entity (e.g., company, institution, organization) on behalf of which the dataset was created?

MegaScenes is publicly accessible.

### How will the dataset will be distributed (e.g., tarball on website, API, GitHub)?

MegaScenes is hosted on [Amazon S3](https://aws.amazon.com/s3/) thanks to the [AWS Open Data Sponsorship Program](https://aws.amazon.com/opendata/). For details and documentation, see our project page at [this URL](https://megascenes.github.io/). 

### When will the dataset be distributed?

The dataset is public as of June 17, 2024.

### Will the dataset be distributed under a copyright or other intellectual property (IP) license, and/or under applicable terms of use (ToU)?

This dataset is licensed under the Creative Commons Attribution 4.0 International License. The photos in the dataset have their own licenses.

### Have any third parties imposed IP-based or other restrictions on the data associated with the instances?

The dataset photos each have their own individual open licenses.

### Do any export controls or other regulatory restrictions apply to the dataset or to individual instances?

No.

## Maintenance

### Who is supporting/hosting/maintaining the dataset?

The dataset is being maintained by the authors at Cornell University.

### How can the owner/curator/manager of the dataset be contacted (e.g., email address)?

Issues and discussions can be publicly posted at [this GitHub page](https://github.com/MegaScenes/dataset). Alternatively, the dataset maintainers can be emailed at megascenes.dataset@gmail.com.

### Is there an erratum?

At the moment, there is no erratum.

### Will the dataset be updated (e.g., to correct labeling errors, add new instances, delete instances)?

Yes, the dataset may be updated at the will of the authors.

### If the dataset relates to people, are there applicable limits on the retention of the data associated with the instances (e.g., were individuals in question told that their data would be retained for a fixed period of time and then deleted)?

N/A.

### Will older versions of the dataset continue to be supported/hosted/maintained?

N/A.

### If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do so?

At the moment, there is no mechanism to build on MegaScenes.
