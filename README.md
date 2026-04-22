# FlinkOnKubernetes

kubectl create namespace flinktest

ServiceAccount: flinktest-account
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: flinktest-account
  namespace: flinktest
EOF


kubectl create clusterrolebinding flinktest-admin --clusterrole=cluster-admin --serviceaccount=flinkecomm01:flinktest-account

kubectl apply -f flink-configuration-configmap.yaml -n flinktest

kubectl apply -f jobmanager-service.yaml -n flinktest

kubectl apply -f jobmanager-session-deployment-non-ha.yaml -n flinktest

kubectl apply -f taskmanager-session-deployment.yaml -n flinktest


kubectl get rolebinding -n flinktest

kubectl get clusterrolebinding

kubectl -n kubernetes-dashboard create token admin-user

kubectl port-forward svc/flink-jobmanager 8081:8081 -n flinktest

nohup kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard --address IP 8443:443 > /dev/null 2>&1 &

nohup  kubectl -n flinktest port-forward svc/flink-jobmanager --address IP 8081:8081 > /dev/null 2>&1 &
