Installation HELM 2:
  1. Download the HELM 2.*.* versions
  2. Extract the tar file and move the helm file to /usr/local/bin/
  3. Create a Service account:
      kubectl -n kube-system create serviceaccount tiller
  4. Create Cluster Role binding and assign the cluster-admin role:
      kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
  5. Start the tiller
      helm init --service-account tiller
  6. Basic commands after installation:
       helm home
       helm repo
       helm search jenkins
       
Deleting installed HELM 2:
   1. helm reset --remove-helm-home
   2. execute kubectl get all -n kube-system | grep -i 'tiller'
   3. If any tiller object is running then delete those using kubectl command

Create your own Helm Chart:
   1. Create a directory and inside it create a file called Chart.yaml
   2. Then create a templates directory inside the Chart directory.
   3. Inside template directory add all the deployment.yaml, service.yaml etc
   4. Execute below command for helm install. Just make sure you are inside your chart directory
       helm install --name my-nginx .
   5. If you want to upgrade the chart then
       helm upgrade my-nginx .
   6. To delete an running helm chart
        helm delete --purge my-nginx
        
 Parametric Helm Chart:
   4. Create a values.yaml for adding the key:values
   5. Add these keys inside the deployment/service.yaml files
   6. The format is, instead of static value give variable as {{ .Values.parameter_name}}
   7. With "helm upgrade" the latest changes will reflect in the helm chart
   
Helm Local Repository:
  1. First remove your existing local repo
      helm repo remove local
  2. Start a new repo. Make sure you are in the Chart directory
      helm serve --repo-path . --address "0.0.0.0:5000"
  3. Add the new repo to helm repo listrepo
      helm repo add myrepo http://localhost:5000/charts
  4. Update the repo:
       helm repo update
  5. List all the repos:
       helm repo list
  6. Create a helm chart for the custom code:
       helm package my-nginx
  7. Index the chart in the repo:
      helm repo index .
  8. Update the repo:
       helm repo update
  9. Search charts in the repo
       helm search myrepo/
  10. Install chart after fetching from repo
        helm install --name abcd myrepo/my-nginx --version=0.5.0
