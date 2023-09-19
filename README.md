# OpenShift Administration
 -sudo yum install httpd-tools
 
 ## Create HTPasswdIdentity Provide for the following users:
  - htpasswd -c -b -B /tmp/passwd admin password
  - htpasswd -b -B /tmp/htpasswd anna password
  - htpasswd -b -B /tmp/htpasswd linda password
  - htpasswd -b -B /tmp/htpasswd developer password
  - htpasswd -b -B /tmp/htpasswd lisa password

## Create secret from HTPasswd
oc create secret generic htpass-users --from-file htpasswd=/tmp/htpasswd -n openshift-config

## Get oauth file and Edit oauth
- oc get oauth cluster -o yaml > /tmp/oauth.yaml
- oc replace -f /tmp/oauth.yaml

## Add cluster role to users
- oc adm policy add-role-users cluster-admin admin
## Delete kubeadmin from cluster
oc delete secrets kubeadmin -n kube-system

## Create groups
- oc adm groups new admins
- oc adm groups new developers
- oc adm groups new viewers

## Add users to groups
- oc adm groups add-users admins admin anna
- oc adm groups add-users developers linda developer
- oc adm groups add-users viewers lisa

  ## Add users
  - oc create user joe
## Setting up Authorization

## Show cluster roles
  - oc describe clusterrole.rbac | grep ^Name | grep -v 'system:'
    
## Add group permission
- oc adm policy add-cluster-role-to-group cluster-admin admins
- oc adm policy add-cluster-role-to-group edit developers
- oc adm policy add-cluster-role-to-group view developers
- oc adm policy add-cluster-role-to-group view viewers
- 
  ## Add user to namespace
- oc create namespace test-namespace
- oc adm policy add-role-to-user view anouk -n test-namespace
- oc adm policy add-role-to-user rdit anouk -n test-namespace

# Create a project template
- oc adm create-bootstrap-project-template -o yaml > template.yaml
- vi template.yaml
## Add labels
- metadata.labels.name = ${PROJECT_NAME}
## Add Network Policy
- apiVersion: networking.k8s.io/v1
- kind: NetworkPolicy
- metadata.name: allow-from-openshift-ingress
- spec:
-   podSelector: {}
-   ingress:
   - from:
   - namespaceSelector:
       matchLabels:
          network.openshift.io/policy-groups: ingress
  
- apiVersion: networking.k8s.io/v1
- kind: NetworkPolicy
- metadata.name: allow-same-namespace
- spec:
-   podSelector: {}
-   ingress:
   - from:
     - podSelector: {}

# Create a secure application
## Create an certificate
- mkdir openssl
- openssl genrsa -des3 -out myCA.key 2048
  ## Sign the certificate
  - openssl req -x509 -new -nodes -key myCA.key -sha256 -days 3650 -out myCA.pem

 ## Application Certificate
 openssl genrsa -out tls.key 2048
 - openssl req -new -key tls.key -out tls.csr
 - openssl x509 -req -in tls.csr -CA myCA.pem -CAKey myCA.key -CAcreateserial -out 
 

  

 
 
