sudo yum install git
git clone -b <branch> --single-branch <<github-project-url>>
sudo yum install python3 python3-pip
pip install django boto3 django-storages[boto3]

Install nginx
copy django.conf to /etc/nginx/conf.d
restart your nginx
