# Exemplar-tooling #

Repository that contains helm-charts and kustomize files for the build tooling.
The child ArgoCD will read these.

It will have:

- Helm Charts for tooling (example: Jenkins, Nexus, buildconfig-pipeline)

- Helm Chart for the App of Apps pattern

- (...)

### How to run ###

To login with argocd from CLI using sso:

```
argocd login $(oc get route argocd-server --template='{{ .spec.host }}' -n labs-ci-cd):443 --sso --insecure
```

else if no sso:

```
argocd login --grpc-web $(oc get routes argocd-server -o jsonpath='{.spec.host}' -n labs-ci-cd) --insecure
```

Finally create the Argo app for build tooling:

argocd app create build-tooling \
    --dest-namespace labs-ci-cd \
    --dest-server https://kubernetes.default.svc \
    --repo https://bitbucket.org/motabilityoperations/exemplar-tooling.git \
    --path "build-tooling-appofapps" --values "values-tooling.yaml"
argocd app sync build-tooling