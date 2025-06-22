


**Environment:** DEV/TEST   
**Summary**: Inside k8s cluster doe-dev-mhs-management-e8k2ymhs-management-euw3 it is not possible to reach the image registry [harbor.bare.pandrosion.org](http://harbor.bare.pandrosion.org) (34.34.166.219) on port 443.  
**Description of incident**: Images cannot be pulled from at least that cluster.  
Affected clusters so far:  

- doe-dev-mhs-management-e8k2y/mhs-management-euw3  
    
- doe-dev-chs-workload-th1ov/chs-workload-euw3  
    

**Please provide a full hostname (FQDN):**  
**Error reference**: From inside the cluster:  
gitlab-runner-848865dd47-rmrdh:/$ nc -v [harbor.bare.pandrosion.org](http://harbor.bare.pandrosion.org) 443  
nc: [harbor.bare.pandrosion.org](http://harbor.bare.pandrosion.org) ([34.34.166.219:443](http://34.34.166.219:443)): Network unreachable


# first thing to do is to check routing this time on-prem stop sending 0.0.0/0 route