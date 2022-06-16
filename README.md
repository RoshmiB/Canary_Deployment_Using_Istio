# Canary_Deployment_Using_Istio
Canary_Deployment_Using_Istio
### create eks cluster using terraform with s3 as be: -
    cd learn-terraform-provision-eks-cluster
    terraform init
    terraform plan
    terraform apply
    aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)
### install istio : -
    curl -L https://istio.io/downloadIstio | sh -
    cd istio-1.14.1
    export PATH=$PWD/bin:$PATH
    istioctl install --set profile=demo -y && kubectl apply -f samples/addons
    kubectl -n istio-system edit svc kiali  ==> change service type to LoadBalancer
    kubectl -n istio-system edit svc grafana ==> change service type to LoadBalancer
### Canary deployment of weather-app-ui using helm : -
    kubectl create namespace prod
    kubectl create namespace stage
    kubectl label namespace prod istio-injection=enabled
    kubectl label namespace stage istio-injection=enabled
    helm install demoappv1 ./weatherapp-ui --set deployment.tag=v1 --namespace prod
    helm install demoappv2 ./weatherapp-ui --set deployment.tag=v2 --namespace stage
    kubectl apply -f IG_VS.yaml
    
![image](https://user-images.githubusercontent.com/42956498/174123776-40bc462f-eb3c-4b35-a3bb-92a161540abb.png)

![image](https://user-images.githubusercontent.com/42956498/174124190-9e696d61-54c3-459b-b7a7-f8947659c491.png)

    
    
    

