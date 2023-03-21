# GitOps Flux Dive

<img src="https://github.com/AlexBenavente/Images/blob/main/flux-stacked-color.png" align="right"
     alt="Flux logo" width="120" height="178">

<!-- TABLE OF CONTENTS -->
## Table of Contents
  <ul>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#workflow">Workflow</a>
      <ul>
        <li><a href="#python-application">Python Application</a></li>
        <li><a href="#continuous-deployment">Continuous Deployment</a></li>
        <li><a href="#gitops-flux">GitOps Flux</a></li>
      </ul>
  </ul>




<!-- ABOUT THE PROJECT -->
## About The Project

I am developing my GitOps skill set through this project. GitOps is an approach to automating DevOps pipelines. It utilizes a version control system, such as Git, for creating and maintaining infrastructure and application source code. This code is stored in a Git repository and acts as your single source of truth. Gitops mechanisms will ensure that the real state (the one in your Kubernetes cluster for example) matches your desired state (the one defined in Git) and will reconcile both.


### Built With

* ![Python]
* ![Flask]
* ![Kubernetes]
* ![GitOps]
* ![Flux]

<!-- GETTING STARTED -->
## Workflow

For my automated CI/CD workflow, I am using GitHub Actions and Flux. GitHub Actions builds and pushes container images to my Docker Hub image repository. Flux is installed and configured within my environment to deploy my application into my Kubernetes cluster and keep them synchronized with the GitHub and Docker Hub repositories.

![GitOps Flux Dive](https://github.com/AlexBenavente/Images/blob/main/gitops-flux-dive.png)

### Python Application

First I created a simple python application using the Flask framework:

    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
      return "Hello Alex!"

    if __name__ == "__main__":
      app.run(host='0.0.0.0', port=7000)
    
I placed my Flask source code in a directory with a requirements file that specifies the dependencies required to run the Flask application. Then I created a Dockerfile which specifies how to build the container image for my application.

### Continuous Deployment

I used GitHub Actions to build and tag my Docker container image whenever I update my application version tag using a Push commit to my GitHub repository. GitHub Actions then pushes the container image to my Docker Hub for use in my kubernetes cluster.

### GitOps Flux

I use the Flux tool as the GitOps agent for my CI/CD workflow. I installed Flux in my kubernetes environment and linked it to my GitHub repo. I then create a kustomization file for Flux to track changes to my application cluster and reconcile them against what is defined in my Git repository.

Flux monitors the declared infrastructure of my kubernetes environment and automatically deploys the defined resources into my kubernetes cluster.

![Flux app 1](https://github.com/AlexBenavente/Images/blob/main/flux-app-1.png)

If I open my web browser, I can see my Flask application is running on my cluster.

![Flask app 1](https://github.com/AlexBenavente/Images/blob/main/flask-app-1.png)

If I update my deployment manifest to increase the number of replicas and push the change to GitHub, Flux will detect this change and automatically update the resources.

![Flux app 2](https://github.com/AlexBenavente/Images/blob/main/flux-app-2.png)

I used Flux's additional components: image reflector and image automation controllers to automatically update my containers to the latest image when a new image is uploaded to my image repository.

First I trigger a new Docker image build and push through GitHub Actions.

![CI Build](https://github.com/AlexBenavente/Images/blob/main/ci-build.png)

![Image Repo](https://github.com/AlexBenavente/Images/blob/main/image-repo-flux.png)

Then Flux detects the new container image and corrects the drift in my environment by updating the deployment to my kubernetes cluster as well as updating my Git repository. 

Then if I open my web browser again, I can see that my Flask application is now running the new version on my cluster!

![Flask app 2](https://github.com/AlexBenavente/Images/blob/main/flask-app-2.png)

<!-- MARKDOWN LINKS & IMAGES -->
[Python]: https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white
[Flask]: https://img.shields.io/badge/Flask-6DB33F?style=for-the-badge&logo=flask&logoColor=white
[Kubernetes]: https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=Kubernetes&logoColor=white
[GitOps]: https://img.shields.io/badge/GitOps-FC6D26?style=for-the-badge&logo=Git&logoColor=white
[Flux]: https://img.shields.io/badge/Flux-4050FB?style=for-the-badge&logo=flux&logoColor=white
