## Laravel Docker Template

This repository takes the work of Emad Zaamout (links below) as a starting point and provides the necessary AWS CloudFormation templates to complete the walkthrough utilizing INF-as-Code.

Original YouTube walkthrough: https://www.youtube.com/watch?v=ksLuij03lbw

Original GitHub repository: https://github.com/emad-zaamout/laravel-docker-nginx-php-fpm-opcache

Order in which to execute AWS CloudFormation stacks:

1. users.yml
2. vpc-outputs.yml
3. p-db.yml
4. p-ecr.yml
5. p-ecs-alb.yml
6. p-codebuild.yml
7. p-route53.yml

Note: The templates do not address (1) purchsaing a domain, or (2) updating the domain's name servers to match the newly created Hosted Zone.

Changes to the original code from the repository link posted above:

1. Update 'entrypoint.sh' and 'entrypoint.local.sh' to create Laravel's storage symbolic link (if it doesn't already exist):
````
if [ ! -e ./public/storage ]; then
    echo "Creating storage symbolic link"
    php artisan storage:link
else
    echo "Storage symbolic link exists"
fi
````

2. Update 'buildspec.yml' to automatically force the ECS Service to produce a new build after updating the ECR image:
````
aws ecs update-service --cluster $CLUSTER --service $SERVICE --force-new-deployment --region $REGION
````
