//vpc:
create vpc -12.0.0.0/16 
create internetgateway
create subnets -12.0.0.0/20-public
                -12.0.16.0/20 private
create routes
---edit routes
---edit subnet associstion
creation of ec2 -2 instance private,public edit network config: private-12.0.2.0/24

in puttyconsole -public nano aws_privatekey_ec2.pem > copy the private key
chmod 400 aws_privatekey_ec2.pem
ssh -i "aws_privatekey_ec2.pem" ec2-user@<EC2-PUBLIC-IP>
 connected to private...


 //Docker
