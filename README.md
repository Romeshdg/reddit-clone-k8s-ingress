# Reddit-Clone-with-Kubernetes

Highlights of this project:

🚀 Automated a highly available Reddit Clone application.

🚀 Used Docker, DockerHub, and Kubernetes to streamline the deployment process.

🚀 Implemented continuous deployment for easy updates and scalability.

🚀 Used Kubernetes Ingress to link the application to the domain for continuous availability.

# Key technologies used: Docker, Docker Hub, Kubernetes, Minikube, CI/CD, Containerization, Automation, Monitoring, Logging, Reddit Clone Application.

![1678264485910](https://user-images.githubusercontent.com/113555417/224940169-c4f0e728-a5a4-45d2-8e24-300953bd42fe.jpeg)


## For Prerequisites [click here](https://trainwithshubham.hashnode.dev/prerequisite-for-deployment-of-a-reddit-copy-on-kubernetes-with-ingress-enabled)

## Step 1 : Create Two Servers: >> t2.micro - CI SERVER  , >> t2.medium- Deployment Server .

Images: 

![Screenshot 2023-03-11 210626](https://user-images.githubusercontent.com/113555417/224940389-3d195129-0830-4374-b0fd-28b05c205b28.jpg)


## Step 2 : Clone the source code

First step is to clone the source code you can do it using bellow command

```bash
git clone https://github.com/Romeshdg/reddit-clone-k8s-ingress.git
```

Images:

![Screenshot 2023-03-11 191647](https://user-images.githubusercontent.com/113555417/224941154-225cc531-06bb-43ca-9a27-6746fa45e853.jpg)

![Screenshot 2023-03-11 191812](https://user-images.githubusercontent.com/113555417/224941162-e04d3c93-c8d3-4219-aedd-200322a8ebe2.jpg)


![Screenshot 2023-03-11 191955](https://user-images.githubusercontent.com/113555417/224941182-3ac0cd6e-9a49-4373-9157-3e956722acc9.jpg)

![Screenshot 2023-03-11 192125](https://user-images.githubusercontent.com/113555417/224941192-d5b10d67-49ee-47a9-86e5-ee0a15de11fc.jpg)


## Step 3 : Containerize the Application using Docker

Write a Dockerfile as below

```bash
FROM node:19-alpine3.15

WORKDIR /reddit-clone

COPY . /reddit-clone

RUN npm install 

EXPOSE 3000

CMD ["npm","run","dev"]
```
Images:

![Screenshot 2023-03-11 192310](https://user-images.githubusercontent.com/113555417/224941381-2f0b697c-2d71-4256-9a17-01b04347a82c.jpg)


## Step 4 : Building Docker-Image

Build a Docker Image

```bash
docker build -t <image-name> .
```

## Step 5 : Push the image to DockerHub

For pushing the image to DockerHub you will need to login to the DockerHub account first for that use below command

```bash
docker login
```

You will be prompted to enter your docker account username and password.

If you don't have the DockerHub account then [click here](https://hub.docker.com/)

Once logged in to your DockerHub account
Use bellow command to push your build image to the DockerHub repository

```bash
docker tag <image-id> <docker-username>/<image-name:tag>

docker push <docker-username>/<image-name:tag>
```

Images :


![Screenshot 2023-03-11 192955](https://user-images.githubusercontent.com/113555417/224941766-0e3351b3-7283-42e8-b784-383c57612bcc.jpg)

![Screenshot 2023-03-11 193010](https://user-images.githubusercontent.com/113555417/224941806-47c56cd3-735f-436c-9f19-d5f2961f43d1.jpg)

![Screenshot 2023-03-11 193606](https://user-images.githubusercontent.com/113555417/224941814-4a8214cd-9957-4566-b4c6-4e429a7b2c16.jpg)

![Screenshot 2023-03-11 193949](https://user-images.githubusercontent.com/113555417/224941827-272d8916-0317-46c5-a1ca-e9b8df3a7290.jpg)



## Step 6 : Writing a kubernetes Manifest File

Now, You might be wondering what this manifest file in Kubernetes is. Don't worry, I'll tell you in brief.

When you are going to deploy to Kubernetes or create Kubernetes resources like a pod, replica-set, config map, secret, deployment, etc, you need to write a file called manifest that describes that object and its attributes either in yaml or json. Just like you do in the ansible playbook.

Of course, you can create those objects by using just the command line, but the recommended way is to write a file so that you can version control it and use it in a repeatable way.


Images:

![Screenshot 2023-03-11 194243](https://user-images.githubusercontent.com/113555417/224942319-224fec4c-1f2b-426e-9490-78b27123a343.jpg)

![Screenshot 2023-03-11 194605](https://user-images.githubusercontent.com/113555417/224942325-3b43db81-e30c-405d-9372-ddc0336b7c79.jpg)

![Screenshot 2023-03-11 194658](https://user-images.githubusercontent.com/113555417/224942333-5e623bea-9473-4ec9-90cb-82e52cc90167.jpg)

![Screenshot 2023-03-11 194949](https://user-images.githubusercontent.com/113555417/224942346-073037b0-40cf-4075-a559-47d48891184a.jpg)

![Screenshot 2023-03-11 195045](https://user-images.githubusercontent.com/113555417/224942362-f0a37792-aebc-4251-b33f-feb1092ad772.jpg)

![Screenshot 2023-03-11 195346](https://user-images.githubusercontent.com/113555417/224942394-e8b32326-a57b-404a-8aac-bb969d23681e.jpg)

- ## Writing a Deployment.yml file

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reddit-clone-deployment
  labels:
    app: reddit-clone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: reddit-clone
  template:
    metadata:
      labels:
        app: reddit-clone
    spec:
      containers:
      - name: reddit-clone
        image: snaket2628/reddit-clone
        ports:
        - containerPort: 3000
```
Images:


![Screenshot 2023-03-11 195438](https://user-images.githubusercontent.com/113555417/224942497-54782d89-c4be-4b2b-975d-80c93dd2ce0f.jpg)

![Screenshot 2023-03-11 195815](https://user-images.githubusercontent.com/113555417/224942608-aba08b7c-6a96-4026-939f-3adf19b1a3f2.jpg)

![Screenshot 2023-03-11 195848](https://user-images.githubusercontent.com/113555417/224942619-a1296120-a0e7-41b0-89f1-bdefd43c3922.jpg)

- ## Writing a Service.yml file

```bash
apiVersion: v1
kind: Service
metadata:
  name: reddit-clone-service
  labels:
    app: reddit-clone
spec:
  type: NodePort
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 31000
  selector:
    app: reddit-clone
```

Images:



![Screenshot 2023-03-11 200751](https://user-images.githubusercontent.com/113555417/224942771-0ba4244c-6aac-4ff5-8466-87f451a24781.jpg)

## Step 7 : Deploy the app to the kubernetes & Create a Service for it.

Now, we have a deployment file for our app and we have a running Kubernetes cluster, we can deploy the app to Kubernetes. To deploy the app you need to run following the command:

```bash
kubectl apply -f deployment.yaml -n <name-space>
```

Same for the service file

```bash
kubectl apply -f service.yaml -n <name-space>
```

## Configuring the Ingress

- Writing ingress.yml

```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-reddit-app
spec:
  rules:
  - host: "domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
  - host: "*.domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
```

apply the ingress file with following command:

```bash
kubectl apply -f ingress.yml -n <name-space>
```
Images:

![Screenshot 2023-03-11 200751](https://user-images.githubusercontent.com/113555417/224943269-d444dc67-9a7c-409e-a530-70e56afe16a3.jpg)

![Screenshot 2023-03-11 201005](https://user-images.githubusercontent.com/113555417/224943282-a499eabf-4b3c-4391-95dd-091358166ebd.jpg)



![Screenshot 2023-03-11 201301](https://user-images.githubusercontent.com/113555417/224943290-25d07919-f7ba-43ec-a873-9bd66be8e346.jpg)


![Screenshot 2023-03-11 203346](https://user-images.githubusercontent.com/113555417/224943298-ea7e9e0d-baf7-4ea4-87c9-23723c1f3318.jpg)


![Screenshot 2023-03-11 203937](https://user-images.githubusercontent.com/113555417/224943305-6a174f95-8f84-47ac-b5ff-716e43f56550.jpg)




## Step 8 : Expose the application

1. First We need to expose our deployment so use kubectl expose deployment reddit-clone-deployment --type=NodePort command.

2. You can test your deployment using curl -L http://{minikubeip}:31000. Port 31000 is defined in Service.yml

3. Then We have to expose our app service kubectl port-forward svc/reddit-clone-service 3000:3000 --address 0.0.0.0 &

### Test Ingress

Now It's time to test your ingress so use the curl -L domain/test command in the terminal.

Images:

![Screenshot 2023-03-11 204650](https://user-images.githubusercontent.com/113555417/224943558-dcc59d58-1a00-4be9-b389-dd064e2243ef.jpg)

## Final step 

Navigate to your chrome browser and enter http://{public-ip}:3000

Images:

![Screenshot 2023-03-11 205530](https://user-images.githubusercontent.com/113555417/224943754-935b0652-227e-4c26-836e-fde392bae631.jpg)

![Screenshot 2023-03-11 204008](https://user-images.githubusercontent.com/113555417/224943421-efa686de-a563-4f98-be4f-7625319b4eec.jpg)



## Overall, this project demonstrates how DevOps practices can be used to enhance the user experience and ensure the continuous availability of web applications. By automating the deployment process and using tools like Docker, DockerHub, and Kubernetes, developers can focus on improving the application itself while leaving the deployment process to the machines.

Thank you!
