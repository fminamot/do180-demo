# DO180 Demo

## はじめに
```
$ lab openshift-resources start
$ source /usr/local/etc/ocp4.config
$ oc login -u ${RHT_OCP4_DEV_USER} -p ${RHT_OCP4_DEV_PASSWORD} ${RHT_OCP4_MASTER_API}
$ oc new-project ${RHT_OCP4_DEV_USER}-do180-demo
```

## (1) Podのみ

vi pod.yml
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: hello-pod
  name: hello-pod
spec:
  containers:
  - image: quay.io/redhattraining/hello-world-nginx:v1.0
    name: hello-world-nginx
    ports:
    - containerPort: 8080
      protocol: TCP
```

```
$ oc create -f pod.yml
```

## (2) Deploy/Service (Kubernetes)
```
kubectl create deployment hello-pod2 --image quay.io/redhattraining/hello-world-nginx:v1.0
kubectl expose deployment hello-pod2 --port=8080
```

## (3) Deploy/Service (OpenShift)
```
oc new-app --name hello-pod3 --docker-image quay.io/redhattraining/hello-world-nginx:v1.0 
oc expose service hello-pod3
oc get route
```

## (4) Podをスケールアップ
```
oc scale --replicas=3 deployment/hello-pod3
```

## (5) Deployment.spec.replicasの値が3になっていることを確認
```
oc edit deployment/hello-pod3
```

## 終了
```
$ oc delete project ${RHT_OCP4_DEV_USER}-do180-demo
$ lab openshift-resources finish
```

