# Установка ноды Gear

Gear — это передовая платформа смарт-контрактов на основе WASM, которую можно развернуть как парачейн Kusama и Polkadot, что позволяет разработчикам развертывать свои dApp менее чем за 5 минут самым простым и эффективным способом.

Сайт проекта  https://www.gear-tech.io/

Документация проекта https://wiki.gear-tech.io/

Discord https://discord.gg/vYkBDnnH

Минимальные требования
CPU 2, RAM 4 GB, SSD 64 GB

Используемые порты:
30333 / TCP


Аренда сервера под ноду https://pq.hosting/?from=36405

## Подготовка сервера
Для начала подготовим сервер и установим нужные пакеты:

```
sudo apt update
```

```
sudo apt install htop mc curl tar wget git make ncdu jq chrony net-tools iotop nload -y
```


## Установка ноды
Скачиваем и устанавливаем предварительно скомпилированный бинарный файл:

```
wget --no-check-certificate https://get.gear.rs/gear-nightly-linux-x86_64.tar.xz
```

```
tar xvf gear-nightly-linux-x86_64.tar.xz
```

```
rm gear-nightly-linux-x86_64.tar.xz
```

## Сделаем файл исполняемым и переместим к бинарным файлам:

```
sudo chmod +x gear && sudo mv gear /usr/bin
```

## Задайте имя ноды (Отредактировать строку на свое имя и выполнить в терминале)

```NODE_NAME=свое_имя_ноды```

## Создаем файл сервиса (копируем все целиком и выполняем)

```
sudo tee /etc/systemd/system/gear-node.service > /dev/null <<EOF
[Unit]
Description=Gear-node
After=network-online.target
[Service]
User=$USER
ExecStart=/usr/bin/gear --name '$NODE_NAME' --telemetry-url 'ws://telemetry-backend-shard.gear-tech.io:32001/submit 0'
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

## Запускаем ноду
```
sudo systemctl daemon-reload && \
sudo systemctl enable gear-node && \
sudo systemctl restart gear-node
```

## Смотрим логи ноды

```
sudo journalctl -u gear-node -f -o cat
```

## Если все верно то должен пойти процесс синхронизации, вот так это выглядит 

```2023-03-28 11:55:41 ⚙️  Syncing 276.4 bps, target=#438230 (40 peers), best: #78438 (0x487d…a957), finalized #78336 (0xf983…023f), ⬇ 418.4kiB/s ⬆ 411.0kiB/s```

```2023-03-28 11:55:46 ⚙️  Syncing 210.0 bps, target=#438230 (40 peers), best: #79488 (0x668b…e79c), finalized #79361 (0x66d2…e83d), ⬇ 535.7kiB/s ⬆ 405.3kiB/s```

```2023-03-28 11:55:51 ⚙️  Syncing 435.2 bps, target=#438230 (40 peers), best: #81664 (0x8b77…5729), finalized #81409 (0x5449…7f49), ⬇ 488.2kiB/s ⬆ 388.6kiB/s```


## Дожидаемся синхронизации, она выглядит вот так

```2023-03-28 12:06:21 💤 Idle (40 peers), best: #438232 (0x87c7…e6ba), finalized #437761 (0x72de…c812), ⬇ 254.2kiB/s ⬆ 252.3kiB/s```

```2023-03-28 12:06:26 💤 Idle (40 peers), best: #438232 (0x87c7…e6ba), finalized #437761 (0x72de…c812), ⬇ 249.9kiB/s ⬆ 247.4kiB/s```

## Проверяем свою ноду, найдите себя в телеметрии:
https://telemetry.gear-tech.io/

просто никуда не нажимая начните вводить имя вашей ноды

## Бэкап: (обязательно)
Создаем каталог для бэкапа и копируем приватный ключ:

```
mkdir -p $HOME/backup/gear
cp $HOME/.local/share/gear/chains/gear_staging_testnet_*/network/secret_ed25519 $HOME/backup/gear/
```

Далее загрузите файл на свой ПК из папки $HOME/backup/gear

## Обновление ноды
Остановите ноду:
```
sudo systemctl stop gear-node
```

Обновите бинарный файл ноды:
```
wget --no-check-certificate https://get.gear.rs/gear-nightly-linux-x86_64.tar.xz && \
tar xvf gear-nightly-linux-x86_64.tar.xz && \
rm gear-nightly-linux-x86_64.tar.xz && \
sudo chmod +x gear && sudo mv gear /usr/bin
```

Запустите ноду:
```
sudo systemctl start gear-node
```
## Удаление ноды
```
sudo systemctl stop gear-node
sudo systemctl disable gear-node
sudo rm -rf $HOME/.local/share/gear
sudo rm /etc/systemd/system/gear-node.service
sudo rm /usr/bin/gear
```

## Полезные команды
Остановить ноду:
```
sudo systemctl stop gear-node
```
Запустить ноду:
```
sudo systemctl start gear-node
```
Проверить логи ноды:
```
sudo journalctl -u gear-node -f -o cat
```


Вступить к нам в сообщество можно только через бота https://t.me/WingsNodeTeamBot


