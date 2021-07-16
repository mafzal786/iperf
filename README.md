# iperf

## Clone the repository to your local machine
#### git clone https://github.com/mafzal786/iperf.git

## Login to openshift using oc login or exporting kubeconfig file and create a new project named "iperf"
#### oc new-project iperf

## Create a new build configuration by running the following
#### oc new-build --name iperf --binary

## Build the image with the files provide from Github repo.
#### cd iperf
#### oc start-build iperf --from-dir=. --follow --wait

## Create the new app using the build we created earlier
#### oc new-app iperf --name iperf-server
#### oc new-app iperf --name iperf-client


## Get the list of PODs created

[root@e26-linuxjb iperf]# oc get pods -o wide
NAME                            READY   STATUS      RESTARTS   AGE     IP             NODE                             NOMINATED NODE   READINESS GATES
iperf-1-build                   0/1     Completed   0          9m44s   10.254.3.214   worker1.sjc02-cdip.cisco.local   <none>           <none>
iperf-74c985c9f8-wjxqv          1/1     Running     0          114s    10.254.3.218   worker1.sjc02-cdip.cisco.local   <none>           <none>
iperf-client-656f77f666-bvtrp   1/1     Running     0          55s     10.254.3.219   worker1.sjc02-cdip.cisco.local   <none>           <none>
iperf-server-599c7797c8-pf5hr   1/1     Running     0          2m14s   10.254.3.217   worker1.sjc02-cdip.cisco.local   <none>           <none>
[root@e26-linuxjb iperf]#
  
  
## Initiate iperf server in iperf-server POD
#### [root@e26-linuxjb iperf]# oc exec -it iperf-server-599c7797c8-pf5hr -- iperf3 -i 5 -s
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
  
## Initiate iperf client in iperf-client POD
#### [root@e26-linuxjb ocp-install2]# oc exec -it iperf-client-656f77f666-bvtrp -- iperf3 -i 5 -t 60 -c $(oc get pod iperf-server-599c7797c8-pf5hr -o jsonpath='{.status.podIP}')
Connecting to host 10.254.3.217, port 5201
[  5] local 10.254.3.219 port 51912 connected to 10.254.3.217 port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-5.00   sec  34.2 GBytes  58.7 Gbits/sec    0    591 KBytes
[  5]   5.00-10.00  sec  34.0 GBytes  58.4 Gbits/sec    0   2.60 MBytes
[  5]  10.00-15.00  sec  34.1 GBytes  58.6 Gbits/sec    0   2.60 MBytes
[  5]  15.00-20.00  sec  33.8 GBytes  58.0 Gbits/sec    0   2.60 MBytes
[  5]  20.00-25.00  sec  34.0 GBytes  58.3 Gbits/sec    0   2.60 MBytes
[  5]  25.00-30.00  sec  33.9 GBytes  58.3 Gbits/sec    0   2.60 MBytes
[  5]  30.00-35.00  sec  33.7 GBytes  58.0 Gbits/sec    0   2.60 MBytes
[  5]  35.00-40.00  sec  33.8 GBytes  58.1 Gbits/sec    0   2.60 MBytes
[  5]  40.00-45.00  sec  33.8 GBytes  58.1 Gbits/sec    0   3.90 MBytes
[  5]  45.00-50.00  sec  33.8 GBytes  58.0 Gbits/sec    0   3.90 MBytes
[  5]  50.00-55.00  sec  33.9 GBytes  58.3 Gbits/sec    0   3.90 MBytes
[  5]  55.00-60.00  sec  34.0 GBytes  58.4 Gbits/sec    0   3.90 MBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-60.00  sec   407 GBytes  58.3 Gbits/sec    0             sender
[  5]   0.00-60.04  sec   407 GBytes  58.2 Gbits/sec                  receiver

iperf Done.
  

  




 
 
