# Exploring nuScenes in FiftyOne
Due to the multi-sensor structure of nuScenes, the dataset in FiftyOne will be a [Grouped Dataset](https://docs.voxel51.com/user_guide/groups.html?_gl=1*1uqzezz*_gcl_au*MTI3OTEyMjEwMy4xNzI2MTQ5NDk3) with some [Dynamic Group Views](https://docs.voxel51.com/user_guide/using_views.html?_gl=1*1s195kz*_gcl_au*MTI3OTEyMjEwMy4xNzI2MTQ5NDk3#view-groups) thrown in there as well. At a high level, we will group together our samples by their associated scene in nuScenes. At regular intervals of each keyframe or approximately every 0.5 seconds (2Hz), we incorporate data from every sensor type, including their respective detections. This amalgamation of data results in distinct groups, each representing the sensor perspective at a given keyframe. We do have each sensor input for every frame, but since only keyframes are annotated, we choose to only load those in.  

## Ingesting nuScenes
To get started with nuScenes in FiftyOne, first we need to set up our environment for nuScenes. It will require downloading the dataset or a snippet of it as well as downloading the nuScenes python sdk. Full steps on installing can be found [here](https://www.nuscenes.org/nuscenes?tutorial=nuscenes).
We want two files, `v1.0-mini.tgz` and `nuScenes-lidarseg-mini-v1.0.tar.bz2`

The tgz files sshould be dropped into the `data/` folder and unpacked with:
```
cd data
tar zxf v1.0-mini.tgz
tar xf nuScenes-lidarseg-mini-v1.0.tar.bz2
```

Once your nuScenes is downloaded, we can kick things off. Let’s start by initializing both nuScenes as well as our FiftyOne dataset. We define our dataset as well as add a group to initialize the dataset to expect grouped data.