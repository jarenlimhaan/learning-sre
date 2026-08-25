openssl genrsa -out bob.key 2048

openssl req -new -key bob.key -out bob.csr -subj "/CN=bob/O=admin"

base64 -w0 bob.csr | jq -Rs '{
  apiVersion: "certificates.k8s.io/v1",
  kind: "CertificateSigningRequest",
  metadata: {name: "bob"},
  spec: {
    request: ., signerName: "kubernetes.io/kube-apiserver-client",
    expirationSeconds: 86400, usages: ["client auth"]
  }
}' | kubectl --context=minikube create -f -

kubectl --context=minikube certificate approve bob

kubectl --context=minikube get csr bob \
  -o jsonpath='{.status.certificate}' | base64 -d > bob.crt

kubectl config set-credentials bob \
  --client-key="$(realpath bob.key)" \
  --client-certificate="$(realpath bob.crt)" \
  --embed-certs=true

kubectl config set-context bob \
  --cluster=minikube \
  --user=bob

kubectl --context=bob auth whoami
kubectl --context=bob auth can-i get pods --namespace=dev
