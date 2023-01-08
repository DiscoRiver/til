```
aws ec2 describe-instances --filters Name=instance.group-name,Values=omnivore --query 'Reservations[*].Instances[*].[PublicIpAddress]' --output text
```