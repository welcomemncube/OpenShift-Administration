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

## Add role to users
- oc adm policy add-role-users cluster-admin admin


 
 
