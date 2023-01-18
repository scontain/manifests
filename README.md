# manifests

This repo contains the default manifests for the SCONE custom resources.

The manifests are versioned using a directory $VERSION for each SCONE $VERSION.

Our `kubectl` plugin (aka `kubectl provision`) uses these manifest by default. 
One can download the manifests, adjust them to one needs and then passed the manifests 
to `kubectl provision` using option `-f`. 


