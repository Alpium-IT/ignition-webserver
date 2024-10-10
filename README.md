# About

dead-simple ningx-based static file server for RHCOS ignition files.

- Docker image used for rootless NGINX:
  `ghcr.io/nginxinc/nginx-unprivileged:1-alpine-slim`

- data dir is: `/usr/share/nginx/html`

## Create/Test
Get ignition files from an existing cluster:

- To retrieve the Master ignition configuration run the following command:
  ```
  oc extract -n openshift-machine-api secret/master-user-data --keys=userData --to=- > master.ign
  ```

- To retrieve the Worker igniton configuration run the following command:
  ```
  oc extract -n openshift-machine-api secret/worker-user-data --keys=userData --to=-  > worker.ign
  ```


## Run

```
$ kustomize build .

$ oc apply -k .

$ ROUTE_HOST=$(oc -n ignition-server get route ignition-server -o jsonpath='{.status.ingress[0].host }')

$ curl -LI http://$ROUTE_HOST/worker.ign

$ echo "run: sudo coreos-installer install --insecure-ignition --ignition-url=http://$ROUTE_HOST/worker.ign  /dev/vda --copy-network"

```
