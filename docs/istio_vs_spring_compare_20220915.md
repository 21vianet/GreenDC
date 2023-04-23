Virtual Machine:
  OS: CentOS Linux release 7.9.2009 (Core)
  CPU: 
    model name: Intel Core Processor (Haswell, no TSX, IBRS)
    MHz: 2399.996
    cores: 4C
  Memory: 
    size: 16G(15.5GB available)
    swap: 0
  Disk:
    size: 200GB
    used: 72GB
Spring Cloud App:
  version: 2021.0.1.0
  size: 17MB
Spring Boot:
  version: 2.7.6
  size: 104MB
Sentinel Dashboard:
  version: 1.8.6
  jar:
    name: sentinel-dashboard-1.8.6.jar
    size: 22MB
Sentinel app:
  jar:
    name: sentineldemo-0.0.1-SNAPSHOT.jar
    size: 23MB
Kubernetes:
  Cluster:
    version: 1.21.5
    node: 1
  Istio:
    version: 1.16.2
  Container:
    count: 2
  Image:
    istio/proxyv2:
      size: 249MB
    istio/pilot:
      size: 190MB
    istio/examples-bookinfo-productpage-v1:1.17.0
      size: 203MB
JMeter:
  version: 5.5
  form: http-request
  pressure: 50 thread/request 
  duration: 120s



| Time | CPU(Spring Cloud) | CPU(Istio) | Memory (Spring Cloud) | Memory (Istio) |
| ---- | ----------------- | ---------- | --------------------- | -------------- |
| 2    | 9.39%             | 6.62%      | 3.29                  | 3.34           |
| 4    | 10.60%            | 6.74%      | 3.31                  | 3.34           |
| 6    | 10.40%            | 6.68%      | 3.28                  | 2.76           |
| 8    | 11.30%            | 6.50%      | 3.31                  | 2.86           |
| 10   | 10.30%            | 6.20%      | 3.24                  | 2.88           |
| 12   | 10.50%            | 6.53%      | 3.32                  | 3.53           |
| 14   | 11.46%            | 6.66%      | 3.42                  | 3.31           |
| 16   | 10.55%            | 6.54%      | 3.34                  | 2.63           |
| 18   | 10.41%            | 6.35%      | 3.37                  | 3.16           |
| 20   | 11.03%            | 6.67%      | 3.29                  | 2.98           |
