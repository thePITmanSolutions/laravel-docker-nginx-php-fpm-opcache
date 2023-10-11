## Laravel Docker Template

This repository takes the work of Emad Zaamout (links below) as a starting point and provides the necessary AWS CloudFormation templates to complete the walkthrough utilizing INF-as-Code.

Original YouTube walkthrough: https://www.youtube.com/watch?v=ksLuij03lbw

Original GitHub repository: https://github.com/emad-zaamout/laravel-docker-nginx-php-fpm-opcache

## Instructions

Order in which to execute AWS CloudFormation stacks: *

1. ./aws/users.yml
2. ./aws/vpc-outputs.yml
3. ./aws/p-db.yml
4. ./aws/p-ecr.yml
5. ./aws/p-ecs-alb.yml
6. ./aws/p-codebuild.yml
7. ./aws/p-route53.yml **

\* Be sure to check each YML file for comments prior to executing

\*\* Does not address (1) purchsaing a domain, or (2) updating the domain's name servers to match the newly created Hosted Zone.

Below are the changes included in this repository that differ from the original code in the repository linked above:

1. Updated `entrypoint.sh` and `entrypoint.local.sh` to create Laravel's storage symbolic link (if it doesn't already exist):
````
if [ ! -e ./public/storage ]; then
    echo "Creating storage symbolic link"
    php artisan storage:link
else
    echo "Storage symbolic link exists"
fi
````

2. Updated `buildspec.yml` to automatically force the ECS Service to produce a new build after updating the ECR image:
````
aws ecs update-service --cluster $CLUSTER --service $SERVICE --force-new-deployment --region $REGION
````
