Практическое задание "Kubernetes. Причины появления. Команда kubectl"


# Задание 1

1. Установка microk8s по инструкции на виртуальную машину
```
sudo apt update
sudo apt install snapd
sudo snap install microk8s --classic
sudo usermod -a -G microk8s admin
sudo chown -f -R admin ~/.kube
newgr microk8s
```

<img width="1087" height="828" alt="image" src="https://github.com/user-attachments/assets/f8774ddf-15f6-4365-bbad-81c53e8523ab" />

2. Проверим статус установки
```
microk8s status --wait-ready
```

<img width="1118" height="826" alt="image" src="https://github.com/user-attachments/assets/7bfe7697-67d3-4382-942b-c8159b1baba9" />

3. Устанавливаем dashboard и проверяем статус. В addons/enabled появился dashboard.
```
microk8s enable dashboard
microk8s status
```

<img width="882" height="841" alt="image" src="https://github.com/user-attachments/assets/41763c21-134d-4eba-bbf3-99ace923946d" />

<img width="1116" height="827" alt="image" src="https://github.com/user-attachments/assets/fdc14df1-4b74-40c9-9311-d5544abd93a9" />

4. Обновляем /var/snap/microk8s/current/certs/csr.conf.template. Добавляем внешний ip адрес сервера в блок alt_names
```
nano /var/snap/microk8s/current/certs/csr.conf.template
```

<img width="1342" height="402" alt="image" src="https://github.com/user-attachments/assets/e6470434-1678-49e6-89d4-479b59c00b2d" />

5. Генерируем сертификат для подключения к внешнему ip адресу
```
sudo microk8s refresh-certs --cert front-proxy-client.crt
```

<img width="1307" height="385" alt="image" src="https://github.com/user-attachments/assets/264854e7-1fbb-471b-ba25-c7122752baf8" />

# Задание 2

1. Устанавливаю kubectl на локальную машину
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
<image src="image/task-2-1.png">

2. На сервере генерирую config файл для доступа к microk8s с локальной машины
```
microk8s config
```
<image src="image/task-2-2.png">

3. Копирую полученный файл на локальную машину в ~/.kube/config . Появляется доступ с локальной машины к серверу
```
kubectl version
kubectl get nodes
```
4. Подключаюсь к dashboard при помощи port-forward
```
kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443
```
<image src="image/task-2-3.png">

5. Для подключения необходим токен. На сервере генерирую токен
```
microk8s kubectl create token default
```
<image src="image/task-2-4.png">
<image src="image/task-2-5.png">

6. Через браузер подключаюсь к dashboard 
```
https://localhost:10443
```
<image src="image/task-2-6.png">
