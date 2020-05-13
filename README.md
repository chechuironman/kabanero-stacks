# Kabanero-stacks

This Kabanero stack hub hosts these application stacks:
- [React]()- Added libraries for IBM Carbon Design
- [Python-Flask]() - Added commands `oc` and `htpasswd`

Once the stacks are packaged and added to the stack hub, as it is shown in the repos of each stack, the stack files are stored in `~/.appsody/stacks/dev.local/`. The YAML file needs to be modified changing the location of the image, by default it points to your local Docker repo `dev.local/xxx`, it needs to point to a repo where your Openshift cluster has access to:
1. Tag your image with the new repo accesible from Openshift:
```console 
foo@bar:~$ docker tag dev.local/appsody/python-stack:latest your_repo/python-stack:0.4.2 
```
2. Push your image to that repo:
```console 
foo@bar:~$ docker push your_repo/python-stack:0.4.2
```
3. Change the `image` key on the YAML file in the local Appsody folder from:

```yaml 
image: dev.local/appsody/python-stack:latest
```
to
```yaml
image: your_repo/python-stack:0.4.2
```

4. Upload the `your_stack_.source.tar.gz`, `your_stack_.source.tar.gz`, and the YAML and JSON files to a release to Gihub. You can see an example [here](https://github.com/chechuironman/kabanero-stacks/releases)

5. Add your new Kabanero stack hub to your Openshift Kabanero CRD:
```console 
foo@bar:~$ oc edit kabanero kabanero -n kabanero
```
and add the new repo on the repositories key:
```yaml
    repositories:
    - https:
        url: https://github.com/kabanero-io/kabanero-stack-hub/releases/download/0.6.3/kabanero-stack-hub-index.yaml
      name: central
    - https:
        url: https://github.com/chechuironman/kabanero-stacks/releases/latest/download/chechuironman-index.yaml
      name: chechuironman 
```
