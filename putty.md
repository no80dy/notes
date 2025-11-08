
## Инструкция по работе с PuTTY
Чтобы подключиться к **Vagrant**-машине через **PuTTY**, нужно выполнить несколько шагов, так как Vagrant по умолчанию использует **SSH-ключи** для аутентификации, а PuTTY работает со своим форматом ключей (`.ppk`).
### Убедитесь, что машина запущена
Перейдите в каталог с вашим `Vagrantfile` и выполните команду:
```bash
vagrant up
```
Если машина уже запущена, Vagrant просто подтвердит это. Затем проверьте параметры подключения:
```bash
vagrant ssh-config
```
Вы увидите примерно следующее:
```bash
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  IdentityFile C:/Users/Имя_пользователя/.vagrant.d/insecure_private_key
```
Эти данные понадобятся для настройки PuTTY:
- **HostName** — адрес (обычно `127.0.0.1`)
- **Port** — порт (по умолчанию `2222`)
- **User** — имя пользователя (`vagrant`)
- **IdentityFile** — путь к приватному ключу
### Конвертируйте ключ в формат PuTTY (.ppk)
PuTTY не понимает формат OpenSSH, который использует Vagrant. Для конвертации используйте **PuTTYgen** (устанавливается вместе с PuTTY).
- Запустите **PuTTYgen**.
- Нажмите **Load** - выберите файл `insecure_private_key`, указанный в `IdentityFile`.
    - Чтобы увидеть файл, установите фильтр на **All Files (_._)**.
- После загрузки нажмите **Save private key**.
    - Можно сохранить без пароля (для Vagrant это нормально).
- Сохраните файл как, например, `vagrant.ppk`.
### Настройте PuTTY для подключения
- Откройте **PuTTY**.
- В разделе **Session** укажите:
    -   **Host Name (or IP address)**: `127.0.0.1`
    -   **Port**: `2222`
    -   **Connection type**: `SSH`
- В левой панели перейдите в **Connection** - **Data** и в поле **Auto-login username** введите **vagrant**
- Затем перейдите в **Connection** - **SSH** - **Auth** - **Credentials** и в поле **Private key file for authentication** укажите ваш `vagrant.ppk`.
- Вернитесь в раздел **Session**, дайте имя (например, `VagrantBox`) и нажмите **Save**, чтобы сохранить настройки.
- Нажмите **Open**, чтобы подключиться.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkxMjU2ODM5M119
-->