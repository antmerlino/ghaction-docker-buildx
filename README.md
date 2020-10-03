A sample project to demonstrate various failed attempts at using Github Actions
with buildx and cache.

# build-baseline Workflow
The baseline building of the containers using normal docker build.

## Result

 ![Build Baseline](https://github.com/antmerlino/ghaction-docker-buildx/workflows/Build%20Baseline/badge.svg)

# buildx-load Workflow
Try using buildx with --load 

## Result
![Load to Docker](https://github.com/antmerlino/ghaction-docker-buildx/workflows/Load%20to%20Docker/badge.svg)

```
Run docker build -t container-2:latest -f container-2/Dockerfile --cache-from "type=local,src=/tmp/.buildx-cache" --cache-to "type=local,dest=/tmp/.buildx-cache" --load .
time="2020-10-03T01:55:38Z" level=warning msg="invalid non-bool value for BUILDX_NO_DEFAULT_LOAD: "
#1 [internal] load .dockerignore
#1 transferring context: 2B done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 87B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/container-1:latest
#3 ERROR: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed

#5 importing cache manifest from local:15821783609297185231
#5 DONE 0.0s

#4 [1/1] FROM docker.io/library/container-1:latest
#4 resolve docker.io/library/container-1:latest 0.1s done
#4 ERROR: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
------
 > [internal] load metadata for docker.io/library/container-1:latest:
------
------
 > [1/1] FROM docker.io/library/container-1:latest:
------
failed to solve: rpc error: code = Unknown desc = failed to load cache key: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
Error: Process completed with exit code 1.
```

# buildx-localregistry Workflow

Try using buildx with --push to local registry

## Result

![Local Registry](https://github.com/antmerlino/ghaction-docker-buildx/workflows/Local%20Registry/badge.svg)

# buildx-push-action Workflow

Try using the buildx-push-action github action.
This fails similarly to buildx-load.yml

## Result

![build-push-action](https://github.com/antmerlino/ghaction-docker-buildx/workflows/build-push-action/badge.svg)
  
```

1s
Run docker/build-push-action@v2
� Buildx version: 0.4.2
� Starting build...
/usr/bin/docker buildx build --tag container-2:latest --iidfile /tmp/docker-build-push-6dYOj5/iidfile --file ./container-2/Dockerfile --load .
time="2020-10-03T01:55:44Z" level=warning msg="invalid non-bool value for BUILDX_NO_DEFAULT_LOAD: "
#2 [internal] load .dockerignore
#2 transferring context: 2B done
#2 DONE 0.0s

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 87B done
#1 DONE 0.0s

#3 [internal] load metadata for docker.io/library/container-1:latest
#3 ERROR: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed

#4 [1/1] FROM docker.io/library/container-1:latest
#4 resolve docker.io/library/container-1:latest
#4 resolve docker.io/library/container-1:latest 0.1s done
#4 ERROR: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
------
 > [internal] load metadata for docker.io/library/container-1:latest:
------
------
 > [1/1] FROM docker.io/library/container-1:latest:
------
failed to solve: rpc error: code = Unknown desc = failed to load cache key: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
Error: The process '/usr/bin/docker' failed with exit code 1
```

# Goals

The ultimate goal is to have Github Actions building Docker containers with
build cache and uploading some of the images to Github Container Registry. 

I am trying to use buildx because from what I understand, that is the direction
things are heading, and provides a lot more control. Not to mention the
usefullness of multiarchitectural builds.