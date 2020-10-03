See "Actions" tab to view different failed workflows.

# What is it

A sample project to demonstrate various failed attempts at using Github Actions
with buildx and cache.

There are currently 4 workflow:

    - build-baseline.yml:
        The baseline building of the containers using normal docker build.

    - buildx-load.yml:
        Try using buildx with --load 

    - buildx-localregistry.yml:
        Try using buildx with --push to local registry

    - buildx-push-action.yml: Try using


# Goals

The ultimate goal is to have Github Actions building Docker containers with
build cache and uploading some of the images to Github Container Registry. 

I am trying to use buildx because from what I understand, that is the direction
things are heading, and provides a lot more control. Not to mention the
usefullness of multiarchitectural builds.