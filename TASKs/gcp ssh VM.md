
https://www.youtube.com/watch?v=elXJCyBSHUk&list=PL_aVpGFtwv24lQXR0_rsQqHXsu5pfglQ_&index=6
1. careate VM
    networking - disable external adress NONE
2. vpc create a firewall rule 
    source ipv4 ranges : 35.235.240.0/20
    port tcp 22

gcloud compute ssh VM_NAME \
    --zone=us-central1-a \
    --tunnel-through-iap

DONE