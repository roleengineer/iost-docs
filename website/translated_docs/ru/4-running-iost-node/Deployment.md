---
id: Deployment
title: Присоединяйтесь к тестовой сети IOST
sidebar_label: Присоединяйтесь к тестовой сети IOST
---

Документация описывает, как настроить работающий сервер, подключающийся к тестовой сети IOST, если вы хотите просто настроить локальный одиночный сервер сети блокчейна для отладки/тестирования, вам лучше обратиться к [Запуску локального сервера](4-running-iost-node/LocalServer.md)   

Мы используем Докер для развертывания узла IOST.

## Требования к машине

Если вы хотите запустить соединение полного узла с сетью IOST, ваша физическая машина должна соответствовать следующим требованиям:

- CPU: 4-ех ядерный и выше (Рекомендуется 8 ядер)
- Memory: 8GB и выше (Рекомендуется 16GB)
- Disk: 1TB и выше (Рекомендуется 5TB жесткого диска)
- Network: доступ к Internet через открытый порт tcp:30000-30002

## Необходимо

- Curl (любая версия, которая вам нравится)
- Python (любая версия, которая вам нравится)
- [Docker 1.13/Docker CE 17.03 или новее](https://docs.docker.com/install)
- (Опционально) [Docker Compose](https://docs.docker.com/compose/install)

## Старт узла

По умолчанию, `/data/iserver` монтируется как том данных, вы можете изменить путь в соответствии с вашими потребностями.
Мы будем вдальнейшем ссылаться на `PREFIX` в качестве пути.

### Используя *boot*(загрузочный) скрипт:

```
curl https://developers.iost.io/docs/assets/boot.sh | bash
```

Вы можете установить исполняемый файл python, используя переменную окружения.
Например `curl ... | PYTHON=python3 bash` для Ubuntu без установленного python.

Этот скрипт очищает `$PREFIX` и запускает новый свежий полный узел, подключающийся к тестовой сети IOST.
Он также генерирует пару ключей *producer* ("производителя блоков") для генерации блоков.
Если вы хотите стать **Узлом Servi**, следуйте следующим шагам [здесь](4-running-iost-node/Become-Servi-Node.md).

Для запуска, остановки и перезапуска узла измените директорию на `$PREFIX` и выполните команду: `docker-compose (start|stop|restart)`

### Вручную:

#### Удаление данных

Если вы уже запускали предыдущие версии iServer, убедитесь, что старые данные были удалены:

```
rm -rf $PREFIX/storage
```

#### Конфигурация

Получите последнюю конфигурацию:

```
# get genesis
curl -fsSL "https://developers.iost.io/docs/assets/testnet/latest/genesis.tgz" | tar zxC $PREFIX

# get iserver config
curl -fsSL "https://developers.iost.io/docs/assets/testnet/latest/iserver.yml" -o $PREFIX/iserver.yml
```

## Старт

Выполните команду для запуска узла:

```
docker pull iostio/iost-node
docker run -d -v /data/iserver:/var/lib/iserver -p 30000-30003:30000-30003 iostio/iost-node
```

## Проверка узла

Файл логов расположен по пути `$PREFIX/logs/iost.log`. Однако, он отключен по умолчанию.
Вы можете включить его, но не забывайте удалять старые файлы логов.

Вы можете получить логи с помощью команды `(docker|docker-compose) logs iserver`.
Растущие значения `confirmed`, как к примеру ниже, означают синхронизацию данных блоков:

```
...
Info 2019-01-19 08:36:34.249 pob.go:456 Rec block - @4 id:Dy3X54QSkZ..., num:1130, t:1547886994201273330, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:34.550 pob.go:456 Rec block - @5 id:Dy3X54QSkZ..., num:1131, t:1547886994501284335, txs:1, confirmed:1095, et:49ms
Info 2019-01-19 08:36:34.850 pob.go:456 Rec block - @6 id:Dy3X54QSkZ..., num:1132, t:1547886994801292955, txs:1, confirmed:1095, et:49ms
Info 2019-01-19 08:36:35.150 pob.go:456 Rec block - @7 id:Dy3X54QSkZ..., num:1133, t:1547886995101291970, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:35.450 pob.go:456 Rec block - @8 id:Dy3X54QSkZ..., num:1134, t:1547886995401281644, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:35.750 pob.go:456 Rec block - @9 id:Dy3X54QSkZ..., num:1135, t:1547886995701294638, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:36.022 pob.go:456 Rec block - @0 id:EkRgHNoeeP..., num:1136, t:1547886996001223210, txs:1, confirmed:1105, et:21ms
Info 2019-01-19 08:36:36.324 pob.go:456 Rec block - @1 id:EkRgHNoeeP..., num:1137, t:1547886996301308669, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:36.624 pob.go:456 Rec block - @2 id:EkRgHNoeeP..., num:1138, t:1547886996601304333, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:36.921 pob.go:456 Rec block - @3 id:EkRgHNoeeP..., num:1139, t:1547886996901318752, txs:1, confirmed:1105, et:20ms
Info 2019-01-19 08:36:37.224 pob.go:456 Rec block - @4 id:EkRgHNoeeP..., num:1140, t:1547886997201327191, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:37.521 pob.go:456 Rec block - @5 id:EkRgHNoeeP..., num:1141, t:1547886997501297659, txs:1, confirmed:1105, et:20ms
...
```

Вы также можете проверить состояние узла с помощью инструмента `iwallet`.
См. также [iWallet](4-running-iost-node/iWallet.md).

```
docker exec -it iserver iwallet state
```

Последняя информация о блокчейне также показана в [blockchain explorer](https://explorer.iost.io).
