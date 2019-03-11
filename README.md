# CircleCI Demo 

A quick demo to show deploying a static site to AWS on a S3 Bucket for Advanced Web at Huddersfield University.

URL: http://hud-uni-advanced-web.s3-website-eu-west-1.amazonaws.com/

## Usage

In order to trigger a deploy a change to master must made. 
When a change has been detected on the master branch CirceCI run and deploy to S3.

## CircleCI Config file

Inside this file we have a job called `deploy`. This job uses the aws-cli docker image from Mesosphere and copies the files from your repository on GitHub to the AWS S3 bucket. 

This job is then triggered by your workflow. The workflow specifes that a s3-deployment will be run when a change is detected in master.

```yaml
version: 2
jobs:
  deploy:
    docker:
      - image: mesosphere/aws-cli

    steps:
      - checkout
      - run:
          name: Deploy to S3
          command: ./aws.sh s3 sync --delete . "s3://hud-uni-advanced-web"
          
workflows:
  version: 2
  s3-deployment:
    jobs:
      - deploy:
          filters:
            branches:
              only: master
```

## License
[MIT](https://choosealicense.com/licenses/mit/)