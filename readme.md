
Repository Init Content
=======================

# Decesion server for policy.

cd /Users/anuragsaran/Documents/MW/summit2017/bxms-xpaas-policyquote/decesion-server

```
oc project brms-server-policyquote
oc create -f decisionserver-63-is.yaml
oc create -f decisionserver-basic-s2i.yaml
```


```
export application_name=policyquote-app
export source_repo=https://github.com/anurag-saran/bxms-xpaas-policyquote.git
export nexus_url=http://nexus-nexus.apps.anuragsdemo.com/
export kieserver_password=kieserver1!
export is_namespace=brms-server-policyquote
export kie_container_deployment="policyquote=com.redhat.gpte.xpaas:policyquote:1.0-SNAPSHOT"
```

```
oc new-app --template=decisionserver63-basic-s2i -p KIE_SERVER_PASSWORD=$kieserver_password,APPLICATION_NAME=$application_name,SOURCE_REPOSITORY_URL=$source_repo,IMAGE_STREAM_NAMESPACE=$is_namespace,KIE_CONTAINER_DEPLOYMENT=$kie_container_deployment,KIE_CONTAINER_REDIRECT_ENABLED=false,MAVEN_MIRROR_URL=$nexus_url/content/groups/public/
```

```
export policyquote_app=<URL of the policyquote app route>

export policyquote_app=policyquote-app-brms-server-policyquote.apps.anuragsdemo.com
export kieserver_password=kieserver1!
```

## To check the health of the server:
```
curl -X GET -H "Accept: application/json" --user kieserver:$kieserver_password "$policyquote_app/kie-server/services/rest/server"
```

## To check which KIE containers are deployed on the server.
```
curl -X GET -H "Accept: application/json" --user kieserver:$kieserver_password "$policyquote_app/kie-server/services/rest/server/containers"

```

## Make a POST Call
```
curl -s -X POST -H "Content-Type: application/json" -H "Accept: application/json" --user kieserver:$kieserver_password -d @policyquote-payload.json "$policyquote_app/kie-server/services/rest/server/containers/instances/policyquote"
```

## To filter out the price field, use grep:
```
curl -s -X POST -H "Content-Type: application/json" -H "Accept: application/json" --user kieserver:$kieserver_password -d @policyquote-payload.json "$policyquote_app/kie-server/services/rest/server/containers/instances/policyquote" | grep '"price"'
```