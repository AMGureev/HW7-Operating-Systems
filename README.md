# HW7-Operating-Systems
## Задание выполнено на 10 баллов. В работе представлена 1 программа, в которой покрываются все условия для получения высокой оценки.
### Задание на 10 баллов: ***Разработать программы клиента и сервера, взаимодействующих через разделяемую память с использованием функций POSIX. Клиент генерирует случайные числа в том же диапазоне, что и ранее рассмотренный пример. Сервер осуществляет их вывод. Предполагается (но специально не контролируется), что запускаются только один клиент и один сервер. Необходимо обеспечить корректное завершение работы для одного клиента и одного сервера, при котором удаляется сегмент разделяемой памяти. Предложить и реализовать свой вариант корректного завершения. Описать этот вариант в отчете.***
Реализовано клиент-серверное приложение, демонстрирующее обмен данными через shared memory. Клиент и сервер доступен в `client.c` и `server.c` соответственно, некоторый общий код доступен в `shared.h`. Для сборки:
```
make client server
```
Ожидается, что перед запуском клиента сервер уже будет запущен. Пример работы:
server:
```
Server is waiting for client
Server got messege: 8841949
Server got messege: 13674892
Server got messege: 12461467
Server got messege: 718101111
Server got messege: 1098765556
Server recived shutdown message
```
client:
```
Client is waiting for server
Sending message 8841949 from server
Sending message 13674892 from server
Sending message 12461467 from server
Sending message 718101111 from server
Sending message 1098765556 from server
^C - exit work
```

Основная логика приложения такова:

Выделяется область общей памяти, содержащая в себе структуру данных для обмена информацией:
```c
struct shared {
    int shutdown;
    int message;
};
```
Выделяется блок из двух семафоров, каждый из которых принимает значение от 0 до 1. Они работают как мьютексы, блокируя то клиент, то сервер.

Попеременно клиент и сервер разблокируют друг друга, после чего блокируют себя (клиент при этом записывает число в общую память, сервер его печатает)

При Ctrl+C как на клиенте, так и на сервере в общую структуру данных записывается флаг отключения, сначала отключается клиент, потом отключается сервер. Ресурсы очищаются.
## Работу выполнил студент 2 курса БПИ227 Гуреев Александр Михайлович
