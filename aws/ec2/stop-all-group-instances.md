```
aws ec2 describe-instances --filters Name=instance.group-name,Values=omnivore --query 'Reservations[*].Instances[*].[InstanceId]' --output text | awk '{ print $1 }' | aws ec2 stop-instances --instance-ids $(paste -s -d ' ' -)
```