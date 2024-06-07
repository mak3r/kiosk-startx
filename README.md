# kiosk-startx
A simple base container image for building a containerized UI application

`Dockerfile.example` contains an example of how to build your kiosk app
`Dockerfile.bci` contains the docker instructions to build the base image

## Assumptions:
* no other console display or window manager is running
* your app is able to serve a UI
* your server is connected to a display

### Run the container in podman standalone
`podman run --log-level=debug --rm --name nowm --privileged -v /run/udev/data:/run/udev/data -v /tmp/.X11-unix/ -v /home/user/xauthority --env DISPLAY=":0" --hostns keep-id docker.io/mak3r/nowm:0.0.13`

### Run the container in podman using play kube
`podman play kube nowm.yaml`

### Run the container in kubernetes
`kubectl apply -f nowm.yaml`

# Example of building your app from the BCI
`Dockerfile.example`
```
# Use the no window manager image 
# This image, by default starts firefox in kiosk mode and pulls up the suse registry
FROM docker.io/mak3r/nowm:latest

# Run commands to install your application in this base container image
RUN zypper -n in xeyes

# Pass the command line which starts up your application
CMD ["xeyes", "-geometry", "600x800"]
```