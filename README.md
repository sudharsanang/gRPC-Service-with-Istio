# gRPC-Service-with-Istio
Setup gRPC Service with Istio on Kubernetes 
===========================================
1) used a simple proto code under /app
2) used helm for packaging 
3) using kind for light weight kubernetes cluster to do the deployment
4) Build gRPC image using docker command  to match the architecture chosen
5) Install istio demo profile
6) Deploy helm chart
7) Scales the pod for load testing
8) used ghz for load test
9) Run a sustained load test to trigger autoscaling

**Installation/setup procedures:**
1) Installed kind using brew
2) setup cluster using kind create cluster
3) Setup istio profile
4) Build docker image to match chosen architecture using ocker buildx build --platform=linux/arm64 -t
5) Load the image using kind load docker-image grpc-echo:local --name grpc-istio-dem
6) perform helm upgrade to deploy the helm chart helm upgrade --install grpc-echo .
7) install ghz using brew install ghz
8) kubectl port-forward svc/grpc-echo 50051:50051
9) perform load testing
    ghz \ 
  --insecure \
  --proto echo.proto \
  --call EchoService.Echo \
  -d '{"message":"hello"}' \
  localhost:50051
10) perform Load testing with Autoscaling
    ghz \
  --insecure \
  --proto echo.proto \
  --call echo.EchoService/Echo \
  -d '{"message":"load"}' \
  -c 200 \
  -z 2m \
  --rps 2000 \
  <service>:50051
