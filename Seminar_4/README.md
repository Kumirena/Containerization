Контейнеризация.

 Урок 4. Dockerfile и слои

 Необходимо создать Dockerfile, основанный на любом образе .
В него необходимо поместить приложение, написанное на любом известном вам языке программирования (Python, Java, C, С#, C++).
При запуске контейнера должно запускаться самостоятельно написанное приложение.

1. Создаем директорию и в ней создаем файл с приложением на языке Python

![Скриншот](/Seminar_4/image1.jpg)

2. Файл с приложением

![Скриншот](/Seminar_4/image2.jpg)

3. В этой же диретории создаем Dockerfile

![Скриншот](/Seminar_4/image3.jpg)

В нем прописываем, что контейнер будет запущен для последней версии ubuntu, 

обновляем пакеты, 

устанавливаем python,

копируем файл с приложением в директорию /app в контейнере,

для этого создаем эту директорию внутри контейнера,

запускаем приложение при старте контейнера (сначала python, затем наше приложение)

4. Командой 

docker build -t python_app .

даем команду собрать контейнер

На скриншоте видно, как команда по слоям собирает контейнер

![Скриншот](/Seminar_4/image1.jpg)

5. Командой 

docker run -it python_app

запускаем контейнер и видим, что команда отработала.

![Скриншот](/Seminar_4/image4.jpg)
