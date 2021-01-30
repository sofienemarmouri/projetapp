# Procedure before launching the deployment

# Install Helm
Installation done with this link :
```
https://helm.sh/docs/intro/install/
```
# Ingress controller
Installation of the ingress controller with the following commands.
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
kubectl create namespace ingress-basic
helm install nginx-ingress ingress-nginx / ingress-nginx --namespace ingress-basic --set controller.replicaCount = 2 --set controller.nodeSelector. "beta \ .kubernetes \ .io / os" = linux - set defaultBackend.nodeSelector. "beta \ .kubernetes \ .io / os" = linux --set controller.admissionWebhooks.patch.nodeSelector. "beta \ .kubernetes \ .io / os" = linux
```
# SSL certificate
Generate the ssl certificate and apply it to Ingress.
```
openssl req -x509 -nodes -days 365 -newkey rsa: 2048 -out aks-ingress-tls.crt -keyout aks-ingress-tls.key -subj "/ CN = lesplusde10 / O = aks-ingress-tls"
kubectl create secret tls aks-ingress-tls --namespace ingress-basic --key aks-ingress-tls.key --cert aks-ingress-tls.crt
```
# kubectl apply/delete
You can use this command to create or update or delete resources..
```
kubectl apply ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)])
kubectl delete ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)])
```
