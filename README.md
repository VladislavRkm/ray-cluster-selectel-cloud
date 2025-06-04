# ray-cluster-selectel-cloud
## Инфраструктура для развёртывания Ray-кластера в облаке Selectel с использованием инструментов Terraform и Ansible
## 1. Клонирование репозитория:
В первую очередь нужно клонировать к себе локально этот репозиторий:

```git clone https://github.com/VladislavRkm/ray-cluster-selectel-cloud.git```

Далее на своём локальном компьютере переходим в его папку: 

```cd путь/до/ray-cluster-selectel-cloud/```

## 2. Terraform:

После перехода нам нужно инициализировать terraform:
```cd terraform```
```terraform init```

Если Terraform успешно инициализирован, появится сообщение с подобным текстом: 

![изображение](https://github.com/user-attachments/assets/f6085ed6-0463-4e90-ae53-88c91195c8fd)

Далее нам нужно выполнить ```terraform plan``` :
#### Важно!:

На этом шаге Вам будет предложено ввести свои креды облака Selectel. Копируем и вставляем:
![изображение](https://github.com/user-attachments/assets/38aa56ea-b333-4140-8d41-b675fddc7a51)

Заключительный шаг с Terraform: 

```terraform apply```

На этом шаге нужно дополнительно подтвердить создание ресурсов, прописав "yes", на определённом шаге.

Если всё выполнено успешно, в облачной панели Selectel, должна быть такая инфраструктура:

![изображение](https://github.com/user-attachments/assets/64f5977e-3924-4f7d-9523-ef4cde2fce3a)
![изображение](https://github.com/user-attachments/assets/e8c70173-fa45-419f-8011-e98af89909e5)
![изображение](https://github.com/user-attachments/assets/5510e627-cee8-4e37-a37d-c8a6c31f9af4)

## Ansible:

Переходим к Ansible, поочерёдно выполняя команды:

```cd .. && cd ansible/```

```ansible all -i inventory.ini -m ping```
На этом шаге должен быть такой ответ:

![изображение](https://github.com/user-attachments/assets/2118b135-21ff-4ca6-ad6b-e21836b71479)

```ansible-playbook -i inventory.ini playbook.yml```

Результат выполнения плейбука:

![изображение](https://github.com/user-attachments/assets/f49440e7-c614-47a0-bab2-8d724c9a72ea)

Далее мы проверяем статус Ray на всех виртуальных машинах:

```ansible all -i inventory.ini -m command -a "systemctl status ray" -b```

![изображение](https://github.com/user-attachments/assets/26803eb4-2bf2-4224-88cd-f2ec5e0c2f69)

Ну и далее запускаем Ray Dashboard:

```ssh -L 8265:localhost:8265 root@178.130.50.85```

![изображение](https://github.com/user-attachments/assets/bf9fd9d8-4e9d-4c83-b059-3c45f03efd74)

![изображение](https://github.com/user-attachments/assets/353821d8-9c2f-46fb-9093-12a5021e1540)

После этого пробрасываем порт, чтобы можно было подключиться к Jupyter:

```jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root```

И подключаемся с нашего локального компьютера:

```ssh -L 8888:localhost:8888 root@178.130.50.85```

Открываем в бразуере: http://localhost:8265 и можно выполнять Ray-код.










