Контейнеризация.

 Урок 3. Введение в Docker

 1. Устанавливаем Docker

 sudo apt update - обновляем списки пакетов

sudo apt upgrade

sudo apt install apt-transport-https ca-certificates curl software-properties-common
устанавливаем пакеты и ключ
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Добавляем репозиторий Docker
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
Обновляем список пакетов, чтобы включить информацию о пакетах Docker из добавленного репозитория:
sudo apt update

![Скриншот](/Seminar_3/image1.jpg)
![Скриншот](/Seminar_2/image2.jpg)

Установливаем Docker:
__
sudo apt install docker-ce
![Скриншот](/Seminar_2/image3.jpg)

Добавляем пользователя в группу docker

sudo usermod -aG docker $USER

Запускаем следующую команду, чтобы применить изменения в текущем сеансе:

newgrp docker

Можем проверить его работу, выполнив команду:

docker --version

![Скриншот](/Seminar_3/image5.jpg)

2.Тестируем.

Запускаем контейнер с использованием образа "cowsay". 
docker run docker/whalesay cowsay Hello, Docker!

![Скриншот](/Seminar_3/image4.jpg)

3.Тестируем команды

Создание и запуск контейнеров:
docker run: Запускает контейнер из образа.
docker start: Запускает остановленный контейнер.
docker stop: Останавливает работающий контейнер.
docker restart: Перезапускает контейнер.
docker exec: Выполняет команду внутри запущенного контейнера.
Управление контейнерами:
docker rm $(docker ps -aq): удалит все остановленные контейнеры
__
docker ps: Просмотр списка запущенных контейнеров.
docker ps -a: Просмотр списка всех контейнеров (включая остановленные).
docker rm: Удаляет контейнер.
docker logs: Просмотр логов контейнера.
Работа с образами:
__
docker images: Просмотр списка образов.
docker pull: Загрузка образа с Docker Hub.
docker build: Сборка образа из Dockerfile.
docker rmi: Удаляет образ.

![Скриншот](/Seminar_3/image6.jpg)

![Скриншот](/Seminar_3/image7.jpg)

![Скриншот](/Seminar_3/image8.jpg)

4. Хранение данных в контейнерах Docker

запустим контейнер из образа Ubuntu и войдем в него:

docker run -it -h GB --name gb-test ubuntu:22.10

Посмотрим содержимое корневой директории:

ls -l 
![Скриншот](/Seminar_3/image9.jpg)

Создадим новую директорию в корне:

mkdir /example

Создадим файл "passwords.txt" и добавим в него какие-либо данные (представим, что это данные сайта или базы данных). Так как нет редактора:

touch /example/passwords.txt
echo "123test" >> /example/passwords.txt
![Скриншот](/Seminar_3/image10.jpg)

Остановим контейнер и запустим его снова
docker stop gb-test

docker start gb-test

docker exec -it gb-test bash

cat /example/passwords.txt
![Скриншот](/Seminar_3/image11.jpg)
Наши данные сохранятся, так как мы не пересоздавали контейнер.

Удалим контейнер и создадим его заново, используя те же команды:

docker stop gb-test

docker rm gb-test

docker run -it -h GB --name gb-test ubuntu:22.10
![Скриншот](/Seminar_3/image11.jpg)
В этот раз наши данные будут утеряны, так как контейнер был удален

Использование внешнего хранилища

Создадим директорию и подмонтируем ее к контейнеру:

mkdir /test/folder

docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

Мы создали директорию и подмонтировали ее в контейнер, что позволило нам сохранить данные.

Добавим данные в подмонтированную директорию:

echo "$HOSTNAME" >> /otherway/test.txt

Проверим доступность данных с локальной системы:

cat /test/folder/test.txt

Удалим контейнер и создадим его снова, подмонтировав директорию:
__
docker rm gb-test
docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

![Скриншот](/Seminar_3/image12.jpg)

Хранение данных в контейнерах Docker

mkdir ~/docker-mount-example
В этой папке создаем файл test.txt и наполним его данными:

echo "This is the host test.txt file" > ~/docker-mount-example/test.txt

В домашней директории создаем файл test.txt, который также понадобится для монтирования в контейнер, но с другим содержимым:

echo "This is the root test.txt file" > ~/test.txt

Создаем контейнер из образа ubuntu:22.10 и задаем ему имя и hostname:

docker run -it -h GB --name gb-test ubuntu:22.10

Смонтируем ранее созданную папку с хоста в контейнер:

docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount ubuntu:22.10

Смонтируем созданный ранее текстовый файл из домашней директории внутрь смонтированной папки в контейнере:

docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10

Посмотрим содержимое текстового файла в контейнере:

cat /container-mount/test.txt

![Скриншот](/Seminar_3/image13.jpg)

Мы видим данные из файла домашней директории.