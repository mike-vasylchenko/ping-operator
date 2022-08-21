#PING OPERATOR
The project is created for deploy "ping" application to kubernetes cluster

##Build and Deploy operator

User must be logged in to kubernetes cluster and container registry before deploy and build 

For example, here is used `hub.docker.com` with `mikevasylchenko` account for docker
```
make docker-build docker-push IMG="mikevasylchenko/ping-operator:v0.0.6"
make deploy IMG="mikevasylchenko/ping-operator:v0.0.2"
```

##Deploy application
```aidl
kubectl apply -f config/samples/demo_v1alpha1_ping.yaml
```
There will be created all necessary resources for the application.

In case if configuration have to be changed just update it in the yaml file `config/samples/demo_v1alpha1_ping.yaml` and apply one more time

For invoke service for start check hosts from config map that was configured in our "Ping" custom resource:
```aidl
kubectl port-forward service/ping-sample 8081:80
```
Then run `http://localhost:8081/check-hosts` using curl or some browser

In pods logs there will be info that contains host url and response code:
```aidl
...
[2022-08-21 07:07:48,449| INFO| Process-2] URL: http://ping-app2/ping; Status Code: 200
[2022-08-21 07:07:48,548| INFO| Process-1] URL: http://ping-app2/ping; Status Code: 200
10.96.1.44 - - [21/Aug/2022 07:07:53] "GET /ping HTTP/1.1" 200 -
10.96.1.44 - - [21/Aug/2022 07:07:53] "GET /ping HTTP/1.1" 200 -
[2022-08-21 07:07:54,458| INFO| Process-2] URL: http://ping-app2/ping; Status Code: 200
[2022-08-21 07:07:54,561| INFO| Process-1] URL: http://ping-app2/ping; Status Code: 200
[2022-08-21 07:08:00,471| INFO| Process-2] URL: http://ping-app2/ping; Status Code: 200
[2022-08-21 07:08:00,571| INFO| Process-1] URL: http://ping-app2/ping; Status Code: 200
...
```
If response code will not be 200 then warning will be added to log

If response body will not contain "pong" text then warning will be added as well 