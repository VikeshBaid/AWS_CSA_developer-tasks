# Commands use to create task 2 Architecture
1. Create a New Security Group which allow http request to the instance
### command:  aws ec2 create-security-group --group-name aws_csa_developer_http --description "allow http request from the user"

For creating a rule in security group:
### command aws ec2 authorize-security-group-ingress --group-name aws_csa_eveloper_http --protocol tcp --port 80 --cidr 0.0.0.0/0

2. Launch EC2 Instance:
### command: aws ec2 run-instances --image-id \<value> --instance-type \<value> --key-name \<value> --security-group-ids \<value> --subnet-id \<value> --count \<value>

3. Create External Volume:
### command: aws ec2 create-volume --availability-zone \<value> --size \<value>

4. Attach the External Volume to Instance:
### command: aws ec2 attach-volume --device \<value> --instance-id \<value> --volume-ids \<value>

5. Install the httpd server and start the service in the instance:
We can ssh into the instance and can install the http server and start the service. I used Amazonlinux 2 AMI which is rpm based system. so i used the following commands.
### command: sudo yum install httpd -y and sudo systemctl start httpd

6. Mount the External volume:
To make our infrastructure more secure we mount the external volume to the "/var/www/html" directory, which is the default directory from where httpd server reads code files. So if in any case we need to delete the instance or mistaken deletes the instance we don't need to re-write the code as or code will be save in the external volume.

   we can relaunch the instance and can attach the external volume to this instance.

7. Create S3 bucket:
  - First we create the s3 bucket:
  ### command: aws s3api create-bucket --bucket \<value> --region \<value> --create-bucket-configuration LocationConstraint=\<value>
  - Then we copy data into the bucket:
  ### command: aws s3 cp \{object} s3://\<bucket_name> --acl \<value>

8. Create CloudFront distribution and generate Universal URL for the object:
CloudFront provides CDN as a Service. It uses the edge lcoation to store dat temprary to provide low latency in accessing the data.

### command: aws cloudfront create-distribution --origin-domain-name \<value>

9. URL provided by the CloudFront:
A new domain name will be created by CloudFront which can be used by anyone in the world to access the data. Since there is nothing mention in the url (unlike the url provided by S3) from where the data coming it is also good to use cloudfront generated domain name from security point of view.















