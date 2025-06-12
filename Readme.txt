# Install git and clone your project
sudo yum install git
git clone -b master --single-branch https://github.com/Birendrakum/AWS-Project.git

#Install python3, django and other dependencies
sudo yum install python3 python3-pip
pip install django boto3 django-storages[boto3]

#Go to AWS-project
cd AWS-Project/

#Install nginx
sudo yum install nginx

# open django.conf file and in server_name give ur ec2 public ip

#copy django.conf inside AWS-Project directory to /etc/nginx/conf.d
cp ./django.conf /etc/nginx/conf.d

#Restart nginx
sudo systemctl restart nginx

# Add your access key, secret key, bucket name and region in settings.py file
# Also in Hostname give '*'
cd webpage_code/
sudo nano ./myproject/settings.py

# start django server
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver

#Access ur website
Copy the public ip of your ec2 and access it in incognito mode
