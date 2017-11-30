```
Welcome to Cloud Shell! Type "help" to get started.
ln: failed to create symbolic link ‘/home/google149420_student/README-cloudshell.txt’: File exists
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud auth list
          Credentialed Accounts
ACTIVE  ACCOUNT
*       google149420_student@qwiklabs.net

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud config list project
[core]
project = qwiklabs-gcp-8d21ce49b66453c1

Your active configuration is: [cloudshell-22094]
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud config set project qwiklabs-gcp-8d21ce49b66453c1
Updated property [core/project].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud config list project
[core]
project = qwiklabs-gcp-8d21ce49b66453c1

Your active configuration is: [cloudshell-22094]
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud config set compute/zone europe-west1-b
Updated property [compute/zone].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud config set compute/region europe-west1
Updated property [compute/region].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ cat << EOF > startup.sh
> #! /bin/bash
> apt-get update
> apt-get install -y nginx
> service nginx start
> sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
> EOF
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute instance-templates create nginx-template \
>          --metadata-from-file startup-script=startup.sh
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/instanceTemplates/nginx-template].
NAME            MACHINE_TYPE   PREEMPTIBLE  CREATION_TIMESTAMP
nginx-template  n1-standard-1               2017-11-30T12:15:53.588-08:00
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute target-pools create nginx-pool
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/regions/europe-west1/targetPools/nginx-pool].
NAME        REGION        SESSION_AFFINITY  BACKUP  HEALTH_CHECKS
nginx-pool  europe-west1  NONE
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute instance-groups managed create nginx-group \
>          --base-instance-name nginx \
>          --size 2 \
>          --template nginx-template \
>          --target-pool nginx-pool
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/zones/europe-west1-b/instanceGroupManagers/nginx-group].
NAME         LOCATION        SCOPE  BASE_INSTANCE_NAME  SIZE  TARGET_SIZE  INSTANCE_TEMPLATE  AUTOSCALED
nginx-group  europe-west1-b  zone   nginx               0     2            nginx-template     no
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute instances list
NAME        ZONE            MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
nginx-kffc  europe-west1-b  n1-standard-1               10.132.0.3   35.205.200.129  RUNNING
nginx-v1xt  europe-west1-b  n1-standard-1               10.132.0.2   35.189.223.80   RUNNING
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute firewall-rules create www-firewall --allow tcp:80
Creating firewall...\Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/firewalls/www-firewall].
Creating firewall...done.
NAME          NETWORK  DIRECTION  PRIORITY  ALLOW   DENY
www-firewall  default  INGRESS    1000      tcp:80
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ curl http://35.205.200.129
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Google Cloud Platform - nginx-kffc!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to Google Cloud Platform - nginx-kffc!</h1>
<p>If you see this page, the Google Cloud Platform - nginx-kffc web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://Google Cloud Platform - nginx-kffc.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://Google Cloud Platform - nginx-kffc.com/">nginx.com</a>.</p>

<p><em>Thank you for using Google Cloud Platform - nginx-kffc.</em></p>
</body>
</html>
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ curl http://35.189.223.80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Google Cloud Platform - nginx-v1xt!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to Google Cloud Platform - nginx-v1xt!</h1>
<p>If you see this page, the Google Cloud Platform - nginx-v1xt web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://Google Cloud Platform - nginx-v1xt.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://Google Cloud Platform - nginx-v1xt.com/">nginx.com</a>.</p>

<p><em>Thank you for using Google Cloud Platform - nginx-v1xt.</em></p>
</body>
</html>
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$

google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute forwarding-rules create nginx-lb \
>          --region europe-west1 \
>          --ports=80 \
>          --target-pool nginx-pool
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/regions/europe-west1/forwardingRules/nginx-lb].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute forwarding-rules list
NAME      REGION        IP_ADDRESS     IP_PROTOCOL  TARGET
nginx-lb  europe-west1  35.205.224.19  TCP          europe-west1/targetPools/nginx-pool
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$


google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute http-health-checks create http-basic-check
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/httpHealthChecks/http-basic-check].
NAME              HOST  PORT  REQUEST_PATH
http-basic-check        80    /
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute instance-groups managed \
>        set-named-ports nginx-group \
>        --named-ports http:80
Updated [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/zones/europe-west1-b/instanceGroups/nginx-group].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$


google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute backend-services create nginx-backend \
>       --protocol HTTP --http-health-checks http-basic-check --global
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/backendServices/nginx-backend].
NAME           BACKENDS  PROTOCOL
nginx-backend            HTTP
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute backend-services add-backend nginx-backend \
>     --instance-group nginx-group \
>     --instance-group-zone europe-west1-b \
>     --global
Updated [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/backendServices/nginx-backend].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$


google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute url-maps create web-map \
>     --default-service nginx-backend
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/urlMaps/web-map].
NAME     DEFAULT_SERVICE
web-map  backendServices/nginx-backend
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute target-http-proxies create http-lb-proxy \
>     --url-map web-map
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/targetHttpProxies/http-lb-proxy].
NAME           URL_MAP
http-lb-proxy  web-map
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute forwarding-rules create http-content-rule \
>         --global \
>         --target-http-proxy http-lb-proxy \
>         --ports 80
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-8d21ce49b66453c1/global/forwardingRules/http-content-rule].
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ gcloud compute forwarding-rules list
NAME               REGION        IP_ADDRESS      IP_PROTOCOL  TARGET
http-content-rule                35.227.224.182  TCP          http-lb-proxy
nginx-lb           europe-west1  35.205.224.19   TCP          europe-west1/targetPools/nginx-pool
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$


google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$ curl 35.205.224.19
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Google Cloud Platform - nginx-kffc!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to Google Cloud Platform - nginx-kffc!</h1>
<p>If you see this page, the Google Cloud Platform - nginx-kffc web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://Google Cloud Platform - nginx-kffc.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://Google Cloud Platform - nginx-kffc.com/">nginx.com</a>.</p>

<p><em>Thank you for using Google Cloud Platform - nginx-kffc.</em></p>
</body>
</html>
google149420_student@qwiklabs-gcp-8d21ce49b66453c1:~$
```
