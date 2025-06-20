#Go to AWS-project
cd AWS-Project/

# open django.conf file and in server_name give ur ec2 public ip
sudo sed -i "s/server_name .*;/server_name $PUBLIC_IP;/g" /home/ec2-user/AWS-Project/django.conf

#copy django.conf inside AWS-Project directory to /etc/nginx/conf.d
cp ./django.conf /etc/nginx/conf.d

#Restart nginx
sudo systemctl restart nginx

# Add your access key, secret key, bucket name and region in settings.py file
cd webpage_code/
sudo nano ./myproject/settings.py
secret_key=$(aws secretsmanager get-secret-value --secret-id "Secret_key" --query "SecretString" --output text)
access_key=$(aws secretsmanager get-secret-value --secret-id "Access_key" --query "SecretString" --output text)
sudo sed -i "s/AWS_ACCESS_KEY_ID .*;/AWS_ACCESS_KEY_ID = '$access_key';/g" /home/ec2-user/AWS-Project/Webpage_code/myproject/settings.py
sudo sed -i "AWS_SECRET_ACCESS_KEY .*;/AWS_SECRET_ACCESS_KEY = '$secret_key';/g" /home/ec2-user/AWS-Project/Webpage_code/myproject/settings.py

# start django server
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver


