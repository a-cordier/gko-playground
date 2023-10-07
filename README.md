# G.K.O. playground

## Prerequisites
 
  - [docker](https://docs.docker.com/engine/install/)
  - [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
  - [helm](https://helm.sh/docs/intro/install/)
  - [kubectl](https://kubernetes.io/docs/tasks/tools/)

## 👷‍♀️ Start a kind cluster

```sh
kind create cluster --config kind.yaml
```

## 👷‍♀️ Deploy APIM

```sh
helm repo update graviteeio
helm upgrade --install --create-namespace -n gravitee apim graviteeio/apim -f values.yaml
```

## 👷‍♀️ Deploy the operator

```sh
helm upgrade --install --create-namespace -n gravitee gko graviteeio/gko --set manager.logs.json=false
```

## 👷‍♀️ Add httpbin to your namespace

```sh
kubectl apply -f httpbin.yml
```

## 🧪 Creating a simple API

```sh
kubectl apply -f resources/echo.yml
curl -i localhost:30002/echo
```

## 🧪 Syncing an API with your APIM instance

⚠️ The files need to be edited before running the commands

```sh
kubectl apply -f resources/management-context.yml
kubectl apply -f resources/echo-context.yml
curl -i localhost:30002/echo-context
kubectl delete -f resources/echo-context.yml
```

## 🧪 Setting up a simple policy flow

```sh
kubectl apply -f resources/echo-auth.yaml
curl -i localhost:30002/echo-auth 
curl -i -u "user:password" localhost:30002/echo-auth
```

## 🧪 Re-using resources

⚠️ The files need to be edited before running the commands

```sh
kubectl apply -f resources/inline-auth-resource.yml
kubectl apply -f resources/echo-auth.yaml
curl -i localhost:30002/echo-auth 
curl -i -u "user:password" localhost:30002/echo-auth
```

## 🧪 Creating an ingress

⚠️ The file needs to be edited before running the commands

```sh
kubectl apply -f resources/httpbin-ingress.yaml
curl -i localhost:30002/httpbin/hostname
curl -i -H "Host: httpbin.example.com" localhost:30002/httpbin/hostname
kubectl delete -f resources/httpbin-ingress.yaml
```

## 🧪 Extending you ingress with a template

⚠️ The file needs to be edited before running the commands

```sh
kubectl apply -f resources/auth-template.yaml 
kubectl apply -f resources/httpbin-ingress.yaml
curl -i -H "Host: httpbin.example.com" localhost:30002/httpbin/hostname
curl -i -H "Host: httpbin.example.com" -u user:password localhost:30002/httpbin/hostname
kubectl delete -f resources/httpbin-ingress.yaml
```

⚠️ The files needs to be edited before running the commands

## 🧹 Cleanup ...

```sh
kind delete cluster --name gravitee-gko
```
