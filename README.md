Технологии разработки программного обеспечения
==============================================
Лабораторная работа №2: создание кластера Kubernetes и деплой приложения
========================================================================
### Выполнила студентка группы МБД2131 Троицкая Т.А.

Целью лабораторной работы является знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер.


Ниже приведен текст deployment.yaml: 

        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: my-deployment
        spec:
          replicas: 10
          selector:
            matchLabels:
              app: my-app
          strategy:
            rollingUpdate:
              maxSurge: 1
              maxUnavailable: 1
            type: RollingUpdate
          template:
            metadata:
              labels:
                app: my-app
            spec:
              containers:
                - image: myapi:latest
                  # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
                  # указыаает на то, что образы нужно брать только из локального registry. В продакшене никогда не использовать
                  imagePullPolicy: Never 
                  name: myapi
                  ports:
                    - containerPort: 8080
              hostAliases:
              - ip: "192.168.49.1" # The IP of localhost from MiniKube
                hostnames:
                - postgres.local
            
            
Ниже приведен текст service.yaml:

        apiVersion: v1
        kind: Service
        metadata:
          name: my-service
        spec:
          type: NodePort
          ports:
            - nodePort: 31317
              port: 8080
              protocol: TCP
              targetPort: 8080
          selector:
            app: my-app
       
       
Ниже представлен скрин вывода команды kubectl get po:
![lab2_1](https://user-images.githubusercontent.com/97349226/148901819-08114f9e-0530-4b68-891c-44964b2b4b48.png)

Ниже представлен скрин графического интерфейса Kubernetes со статусом подов:
![Lab2_2](https://user-images.githubusercontent.com/97349226/148902477-80983090-afb5-473c-97b9-0253947aaa3a.png)

Видео с обзором кластера (47 сек):

https://user-images.githubusercontent.com/97349226/148902534-ec80bb2f-b5b0-4625-9871-7cfbad5c8c2b.mp4




