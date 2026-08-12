# AKS Homelab GitOps Repo

Push this repo to your own Git provider (GitHub/GitLab/Azure Repos),
then update every `# <-- CHANGE ME` placeholder (repo URL, domain name,
passwords/secrets) before syncing in Argo CD.

## Layout

```
clusterissuer/          cert-manager ClusterIssuer (Let's Encrypt) - apply once
argocd/
  projects/              AppProject - guardrails for these apps
  apps/                  Argo CD Application manifests (apply these to bootstrap)
apps/
  homepage/              static site: Deployment + Service + ConfigMap + Ingress
  searxng/                helm values + extra/ (Ingress, settings ConfigMap)
  paperless/              helm values + extra/ (Ingress)
  immich/                 helm values + extra/ (Postgres, PVC, Ingress)
```

## Routing summary

| URL                              | App       | Notes                                   |
|-----------------------------------|-----------|------------------------------------------|
| https://yourdomain.com/           | homepage  | static, path-based                        |
| https://yourdomain.com/search     | searxng   | path-based (needs X-Script-Name header)   |
| https://yourdomain.com/paperless  | paperless | path-based (needs FORCE_SCRIPT_NAME)      |
| https://photos.yourdomain.com     | immich    | MUST be a subdomain - no subpath support  |

See the accompanying walkthrough for the full install steps
(ingress-nginx, cert-manager, Argo CD, DNS, bootstrap order).
