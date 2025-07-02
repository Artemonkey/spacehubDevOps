<h1 align="center">🛰️Балансировщик для космических кораблей</h1>

> **Задача:**
> Есть программное обеспечение для космопортов. Его docker-образ находится по адресу: _hub.66bit.ru/shared/images/devops-practice:latest_. \
> На планете находится 4 космопорта. У каждого космопорта есть имя, которое задается через переменную окружения ASP_INSTANCE_NAME. \
> ПО космопорта, при обращении к нему, возвращает своё имя. Вам нужно написать docker-compose файл, в котором находятся несколько космопортов и диспетчерская служба в виде балансировщика нагрузки, к которой обращаются космические корабли при посадке на планету. Диспетчерская служба последовательно предлагает разные космопорты кораблям. В качестве балансировщика можно использовать nginx.

## ⚙️ Архитектура

![deployment-diagram.drawio](https://github.com/Artemonkey/spacehubDevOps/blob/main/doc/deployment-diagram.drawio.png)

- **spaceport_alpha** – космопорт с меткой `alpha`
- **spaceport_beta** – космопорт с меткой `beta`
- **spaceport_gamma** – космопорт с меткой `gamma`
- **spaceport_delta** – космопорт с меткой `delta`
- **dispatcher** – балансировщик на базе `nginx`, получающий запросы от кораблей через `http://spacehub.local`

## 🧪 Поведение системы

1. Корабль отправляет HTTP-запрос на `http://spacehub.local/`
2. Балансировщик `dispatcher` перенаправляет запрос к одному из четырёх космопортов по алгоритму **round-robin**
3. Программное обеспечение космопорта отвечает своим именем, заданным через переменную окружения `ASP_INSTANCE_NAME`

## 🖥️ Пример настройки hosts файла (на хост-машине)

```txt
# добавляем в /etc/hosts или C:\Windows\System32\drivers\etc\hosts с правами администратора!
127.0.0.1 spacehub.local
```

## ✅ Как запустить?

Введите в консоли:

```bash
docker compose up -d
```

Ожидайте примерно такой вывод в консоли:

```bash
[+] Running 5/5
 ✔ Container spacehubdevops-spaceport_gamma-1  Started   0.4s
 ✔ Container spacehubdevops-spaceport_beta-1   Started   0.4s
 ✔ Container spacehubdevops-spaceport_alpha-1  Started   0.3s
 ✔ Container spacehubdevops-spaceport_delta-1  Started   0.3s
 ✔ Container spacehubdevops-dispatcher-1       Started   0.7s
```

После этого открывайте в браузере:

```txt
http://spacehub.local/
```

## 🚀 Результат

- Космопорты работают изолированно и параллельно
- Диспетчер автоматически балансирует нагрузку
- Корабли получают равномерное распределение на все 4 порта

```txt
Пример ответа:
alpha
beta
gamma
delta
alpha
...
```
