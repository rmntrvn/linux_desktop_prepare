# Подготовка Linux Desktop для DevOps-инженера

Проект автоматизирует установку рабочего окружения на Ubuntu 26.04 и Fedora 45.
Playbook запускается локально на подготовляемом компьютере.

## Установка

1. Установить Ansible:
```
sudo apt install ansible -y       # Ubuntu
# Для Fedora: sudo dnf install ansible -y
```
2. Установить роли и коллекции:
```
ansible-galaxy install -r requirements.yml
```
3. Проверить inventory и, при необходимости, задать переменные в `-e` или inventory.
4. Запустить playbook для Ubuntu или Fedora:
```
ansible-playbook -i inventory.yml playbook.yml --ask-become-pass
# Fedora допускает тот же playbook или явный Fedora-вход:
ansible-playbook -i inventory.yml playbook-fedora.yml --ask-become-pass
```

Для запуска отдельной части используйте tags, например:
```
ansible-playbook -i inventory.yml playbook.yml --tags dnf-optimization --ask-become-pass
```

На Fedora 45 playbook отключает Cisco OpenH264, удаляет его плагины и
устанавливает свободный мультимедийный стек через RPM Fusion Free.

По умолчанию WireProxy устанавливается как бинарный файл, но его systemd-служба
включается только при переданном непустом `wireproxy_config_file` и
`wireproxy_custom_config: true`.

Роль `opencode` устанавливает OpenCode и codebase-memory-mcp для пользователя,
запустившего playbook через `sudo`. MCP-сервер автоматически добавляется в
глобальную конфигурацию OpenCode. Запустить только эту роль можно так:
```
ansible-playbook -i inventory.yml playbook.yml --tags opencode --ask-become-pass
```

## TODO:

- [x] Роль установки zsh с плагином oh-my-zsh.

- [ ] Автоматизация установки бинарей (возможно лучше использовать binenv):
  - kubie / kubecm
  - minikube
  - terragrunt
  - yq

- [ ] Вынести в отдельную роль установку docker. Выполнить автоматизацию по перемонтированию `/var/lib/docker` на отдельное блочное устройство. Выполнить настройку `.docker/config.json`.

- [ ] Вынести в отдельную роль установку vscode в установкой плагинов и других настроек.

- [ ] Вынести переменные в отдельный файл.
