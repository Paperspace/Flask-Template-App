# Flask Template Paperspace App

## Intro
This is a template Flask app that can be run on Paperspace. This repo is to be used as a reference for users who are looking to create their own Flask app on Paperspace.

- The Flask app is located in `app/main.py`
- The Dockerfile is used to create an image which was pushed as a public image to [paperspace/flask-template-app:latest](https://hub.docker.com/repository/docker/paperspace/flask-template-app)
- The above image can be deployed to Paperspace using the deployment spec located in `paperspace.yaml`

## Project Structure
```
├── Dockerfile
├── requirements.txt
├── paperspace.yaml
├── app
    ├── main.py
```

[paperspace.yaml](paperspace.yaml):
```
apiVersion: latest
enabled: true
name: my-flask-app
image: paperspace/flask-template-app:latest
port: 80
healthChecks:
  startup:
    path: /
  readiness:
    path: /
  liveness:
    path: /
resources:
  replicas: 1
  instanceType: A4000
  autoscaling:
    enabled: true
    maxReplicas: 2
    metrics:
      - metric: requestDuration
        summary: average
        value: 1.2
      - metric: cpu
        summary: average
        value: 75
```

## Develop locally
- Clone the repo to your workspace: `git clone https://github.com/gradient-ai/Flask-Template-App.git`
- Make updates to your application (e.g. application files, Dockerfile, requirements.txt)
- Build a new image by running `docker build -t my-image:tag .`
- Push image to the container registry of your choice by running `docker push my-image:tag`
- Update the deployment spec in paperspace.yaml to the location of your new image
- Deploy your application on Paperspace by running `pspace up`. Ensure you have the [Paperspace CLI](https://github.com/Paperspace/cli#installation) installed.

## How to deploy
- Download the [Paperspace CLI](https://github.com/Paperspace/cli#installation)
- run `pspace init -t https://github.com/gradient-ai/Flask-Template-App` to initialize you app. This will initialize the project locally and clone this GitHub repo as your project template.
- run `pspace up` to deploy your app on Paperspace. This will send the spec in [paperspace.yaml)(paperspace.yaml) to Paperspace to spin up your application.

## Deployment GitHub Action
Use the [Paperspace Deploy GitHub action](https://github.com/Paperspace/deploy-action) to integrate the build/push process with your CI/CD pipeline.

## Additional Information
Find more information about Paperspace apps in our [docs](https://docs.paperspace.com/gradient/deployments).