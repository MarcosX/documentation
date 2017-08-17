# Platform Repository Conventions

## Structure
Platform repositories should expose three batch files in their root:

  - `./build.sh` should preform any preparatory steps, for example - building docker images and pushing them to a container registry
  - `./deploy.sh` should deploy the application to the target
  - `./remove.sh` should remove all traces of the application from the target.

## Variables and Configuration
Platform repositories should be able to be customised to their implementation using environment variables.  For example [platform-app-gocd-kube](https://github.com/techops-platform/platform-app-gocd-kube#installation--usage) builds and deploys GoCD to a kubernetes cluster.  It is a generic implementation that a product team customises by setting environment variables like `KUBE_NAMESPACE` and so on.

It is good practice to have the `build`, `deploy` and `remove` scripts validate the environment variables have been set, and confirming to the user before any action.  An example of how to do this can be [seen here](https://github.com/techops-platform/platform-app-gocd-kube/blob/master/common.sh#L28).

## Customisations
We want to ensure that we allow product teams to customise the repositories in such a way which minimises the changes of them encountering merge conflicts when they're pulling in updates, whilst at the same time giving them the flexibility to meet their requirements.

An example of such behaviour can be seen in the [platform-app-gocd-kube](https://github.com/techops-platform/platform-app-gocd-kube#product-team-specific-changes) repository which exposes two folders where product teams can place customisations:

  - `postbuild` - for scripts that should be run whilst building the docker image
  - `preboot`  - for scripts that should be run when the container boots.

If you go down a similar route, try to stick to the convention of `[00-49]_script_name.sh` for platform scripts, and then product teams can use `[50-99]_script_name.sh`.
