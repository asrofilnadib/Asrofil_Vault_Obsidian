dig beszel.asrofil.my.id +short
#### nambahin repo helm ke k3s
1. buat nyari repo lewat helm
```yaml
helm search hub <nama image> -o yaml

#contoh hasilnya
- app_version: 10.11.5
  description: A Helm chart for Jellyfin Media Server
  repository:
    name: jellyfin--jellyfin-helm
    url: https://jellyfin.github.io/jellyfin-helm
  url: https://artifacthub.io/packages/helm/jellyfin--jellyfin-helm/jellyfin
  version: 2.7.0
```
2. tambahin repo
```yaml
helm repo add jellyfin 
# contoh
helm repo add jellyfin https://jellyfin.github.io/jellyfin-helm

helm repo update
```
3. buat namespace
```yaml
kubectl create namespace jellyfin
```
4. install repo
```yaml
helm install <RELEASE_NAME> <REPO_NAME>/<CHART_NAME> -n <NAMESPACE>

#contoh
helm install jellyfin jellyfin/jellyfin -n jellyfin
```

#### ekspose image ke nodeport