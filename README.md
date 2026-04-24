# k8-volumes
k8-volumes


# Volumes
1. Empty Dir
2. Host Path


# Empty Dir
- Create volume at POD level
- emptyDir volume is initially empty when POD is created
- All containers in the pod can read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each container
- Mount volume to the container inside pods, Volume will be created when POD creates, Volume will be deleted when POD deletes
- Volume is shared between all the containers inside the POD, Volume is stored in the node where POD is running
- Volume is not persistent, data will be lost when POD is deleted
- Use case: Temporary data, cache, scratch space, etc.