
#grafana

how to:
1. run ec2 - gafana and promethus
2. run ./promethus
3. remember to change ip of promethus ec2 in grafana
### => EC2: grafanaaa-ec2

start ec2

 ```
 aws ec2 start-instances --instance-ids i-03862aa79e43b564e
 ```

check ip 
```
aws ec2 describe-instances --instance-ids i-03862aa79e43b564e --query 'Reservations[*].Instances[*].PublicIpAddress' --output text
```


connect to ssh on aws-ec2
```
IP_ADDRESS="54.204.148.14"
ssh -i "grafana-ec2.pem" ubuntu@"$IP_ADDRESS"
```

browser : 
```
http://54.204.148.14:3000/login
```

reset password for grafna on ec2:
```
sudo grafana-cli admin reset-admin-password <new_password>
```
### => EC2: Prometh-ec2

start ec2

 ```
 aws ec2 start-instances --instance-ids i-0568448bdde937cca
 ```

check ip 
```
aws ec2 describe-instances --instance-ids i-0568448bdde937cca --query 'Reservations[*].Instances[*].PublicIpAddress' --output text
```


connect to ssh on aws-ec2
```
IP_ADDRESS="34.203.233.81"
ssh -i "prometh-ec2.pem" ubuntu@"$IP_ADDRESS"
```

```
http://34.203.233.81:9090
```
run prometheus
```
./prometheus --config.file=prometheus.yml --storage.tsdb.path=./data
```

### => EC2: vyos-pay4

start ec2

 ```
 aws ec2 start-instances --instance-ids i-050b4c732050eef78
 ```

check ip 
```
aws ec2 describe-instances --instance-ids i-050b4c732050eef78 --query 'Reservations[*].Instances[*].PublicIpAddress' --output text
```

34.207.136.18
connect to ssh on aws-ec2
```
IP_ADDRESS="34.235.158.170"
ssh -i "vyos-pay-ed.pem" vyos@"$IP_ADDRESS"
```

browser : 
```
http://34.235.158.170:3000/login
```

```
http://34.235.158.170:9273/metrics
```

***
***
shut down from ssh=cli
```
sudo shutdown -h now
```

stop ec2
```
aws ec2 stop-instances --instance-ids i-03862aa79e43b564e
```
force:
```
aws ec2 stop-instances --instance-ids i-03862aa79e43b564e --force
```
 check status of ex2
 ```
 aws ec2 describe-instances --instance-ids i-03862aa79e43b564e --query 'Reservations[*].Instances[*].State.Name'

 ```
***
 ===> grafana
 ```
sudo service grafana-server start
sudo service grafana-server status
sudo systemctl enable grafana-server
 ```



***

secure .pm key
```
chmod 600 prometh-ec2.pem
```

