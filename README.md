# Little speak arround Helm and Kustomize

bash_history can be found .demoit/.bash_history

```bash
# Helm - charts
helm create helm-create
helm template . --name my-release-name
helm upgrade my-release-name . --install --dry-run --debug
# Helm - package and repository
helm search httpbin
helm package .
l ~/.helm/repository/local
# Helm - dependency
helm dependency update
# Helm - release
helm install local/helm-httpbin --name cloudatine
k get po
k get svc
watch kubectl get po
watch kubectl get svc
k port-forward svc/cloudatine-helm-httpbin 4242:80
helm delete cloudatine --purge
# Kustomize - demo
kustomize build base
mkdir -p overlays/official
cd overlays/official
touch kustomization.yaml
kustomize edit fix
kustomize edit add base ../../base
kustomize edit set image mccutchen/go-httpbin=kennethreitz/httpbin:latest
tee deployment-port.yaml <<EOF \
apiVersion: apps/v1beta2\
kind: Deployment\
metadata:\
  name: kustomize-httpbin\
spec:\
  template:\
    spec:\
      containers:\
      - name: httpbin\
        ports:\
        - containerPort: 8080\
          \$patch: delete\
        - name: http\
          containerPort: 80\
          protocol: TCP\
EOF
kustomize edit add patch deployment-port.yaml
# Kustomize - deploy
k apply --dry-run -o yaml -k base
kustomize edit set namespace cloudatine
kustomize edit set namesprefix cloudatine-
# Work with files
kustomize edit add configmap conf --from-file=conf/sample.conf.toml
kustomize edit add secret very-secret --from-file=tls/*
```

Links :
* https://docs.google.com/presentation/d/1GEJLhFaks7MDZLV1TvN2tHqHgesZVP3gQ9wDSMUeADo/edit#slide=id.g528821b9bb_11_45
* https://github.com/kubernetes-sigs/kustomize/tree/master/docs
* https://helm.sh/docs/developing_charts/#the-chart-repository-guide
* https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/#labels



# Credits

* demo framework is [gracefully open sourced](https://twitter.com/dgageot/status/966010649926135808?lang=fr) by David GAGEOT and source can be found here : https://github.com/dgageot/demoit
