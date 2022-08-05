Install ISTIO:

`curl -L https://istio.io/downloadIstio | sh -`


`cd istio-1.14.3`


`chmod +x bin/istioctl`


`cp  bin/istioctl  /usr/local/bin/istioctl`


`istioctl install --set values.global.proxy.privileged=true `    #Important


`kubectl label namespace $NAMESPACE istio-injection=enabled`


Deploy App:

`kubectl create ns istio-mtls`

`kubectl label ns istio-mtls istio-injection=enabled`

`kubectl apply -f echoserver.yaml -n istio-mtls`  including istio ingress example flagged as 'starting the ingress part'

Deploy nginx:

`kubectl apply -f nginx.yaml -n istio-mtls`

access nginx pod bash/sh: (1)

`kubectl exec -it pod/nginx-v1-5b5c47574b-s6mr7 -n istio-mtls -- sh`

and run the following:
- Install Curl:

`apk add curl`

- Access echo server from nginx pod: (2)

`curl echoserver:8080`


deploy Peer PeerAuthentication at the application level for echoserver:

`kubectl apply -f peer-serviceL.yaml -n istio-mtls`

now nginx should access echoserver with mtls 

to prevent the access deploy destination:

`kubectl apply -f dest-serviceL.yaml -n istio-mtls`

now if you try (1,2) nginx can't access echoserver






