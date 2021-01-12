# B2Broker-task

## Task

Есть сервис (service), общение с которым происходит через rabbitmq. В rabbit есть 2 очереди.
Из первой service вычитывает все сообщения, а во вторую пишет ответ на сообщение.
Необходимо реализовать еще один сервис (proxy), который будет проксировать все запросы,
приходящие к нему по http на service таким образом, чтобы не отдавать ответ по http, пока
мы не получим ответ от service
Шаги, выполненные в proxy должны быть следующими
Получение http request
Отправка сообщения в очередь
Получение ответа из очереди
Отправка http response пользователю
Особые моменты
Так как необходимо написать только proxy то service можно реализовать на свое усмотрение.
Он может ничего не делать с сообщением, а просто пересылать запрос в очередь ответа
С учетом пункта 1, маппинг запросов и ответов к service можно делать на основе полного
совпадения запросов и ответов
Сами сообщения должны быть разными, чтобы избежать неправильного маппинга (можно в
сообщение просто генерировать uuid)
Url запроса не важен, как и его метод
Можно использовать любые библиотеки, кроме тех, которые уже реализуют функционал
проксирования http в mq

# Start 

Start RabbitMQ
```
  docker run --rm -d -p 15672:15672 -p 5672:5672 --name b2bbroker-task-mq rabbitmq:3-management
```
Go on `localhost:15672` and create queues: 
- `to-service`
- `to-proxy`

Start proxy
```
  go run cmd/proxy/main.go
```

Start service
```
  go run cmd/service/main.go
```
# Proxy API

[API](cmd/proxy/API.md)

# Todo 

- [X] Debug
- [X] Tests
  - [X] MQ
- [X] Build images
- [ ] Create queues at start RabbitMQ container 
- [ ] Start with docker
- [X] Describe proxy API
- [ ] Review