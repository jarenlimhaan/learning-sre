kubectl apply -f .
kubectl get ns
kubectl get pods -n argocd
kubectl get svc -n argocd
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" -n argocd | base64 -d 
kubectl port-forward svc/argocd-server -n argocd 8080:80
kubectl get deploy
kubectl get pods --watch
kubectl get app -n argocd
kubectl delete sa k8s-dashboard-admin -n k8s-dashboard
kubectl scale deploy guestbook-app-helm-guestbook --replicas=5
kubectl get pod --watch
kubectl get secrets -n argocd
kubectl create secret generic private-repo-https --from-literal type=git --from-literal password=<<PAT Token>> --from-literal username=jarenlimhaan --from-literal url=https://github.com/jarenlimhaan/guestboook.git -n argocd
kubectl label secret private-repo-https -n argocd argocd.argoproj.io/secret-type=repository
kubectl port-forward service/argo-rollouts-dashboard 31000:3100 -n argo-rollouts
kubectl argo rollouts dashboard 
kubectl argo rollouts list rollouts
kubectl get rollouts get rollout [rollout-name] --watch
kubectl argo rollouts get rollout simple-color-app
kubectl argo rollouts promote simple-color-app
kubectl argo rollouts retry rollout simple-color-app
kubectl get svc -n bluegreen-lab