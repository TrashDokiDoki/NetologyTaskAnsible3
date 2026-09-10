# NetologyTaskAnsible3

# Домашнее задание к занятию 3 «Использование Ansible»

# Подготовка к выполнению

1. Подготовьте в Yandex Cloud три хоста: для `clickhouse`, для `vector` и для `lighthouse`.
2. Репозиторий LightHouse находится [по ссылке](https://github.com/VKCOM/lighthouse).

# Основная часть

1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает LightHouse.
2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
3. Tasks должны: скачать статику LightHouse, установить Nginx или любой другой веб-сервер, настроить его конфиг для открытия LightHouse, запустить веб-сервер.
4. Подготовьте свой inventory-файл `prod.yml`.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-03-yandex` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

# Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---

# Решение

1. Для начала создал ещё одну ВМ

<img width="2446" height="378" alt="image" src="https://github.com/user-attachments/assets/ac3f211c-9f15-40c6-b49b-64f9f12c9322" />

2. Далее я дописал playbook(playbook отдельно приложил)

3. Далее был подготовлен inventory-файл

<img width="490" height="353" alt="image" src="https://github.com/user-attachments/assets/b425e78f-cfbd-4a32-aebc-6958b4fc3ef3" />

4. Следующим шагом был запущен `ansible-lint site.yml`, который показал что ошибок нет

5. После я запустил playbook с флагом `--diff` дважды. Первый раз проверка работоспособности playbook, а второй для проверки на идемпотентность

  * Вывод с первой попытки

<img width="1028" height="388" alt="image" src="https://github.com/user-attachments/assets/cad7a5b5-81a7-41e2-bc53-e6d561dbff6b" />

  * Вывод со второй попытки

<img width="927" height="389" alt="image" src="https://github.com/user-attachments/assets/a1f22af9-9cec-4d9f-b4ad-92a7920de344" />

6. Так же я проверил, что `lighthouse` работает

<img width="1851" height="1010" alt="image" src="https://github.com/user-attachments/assets/5c828fa2-6085-4328-b4e0-6c3494bb99bc" />

---

# Что делает playbook

**ClickHouse**
 
 * скачивает и устанавливает ClickHouse;
 * запускает сервис;
 * создает базу logs.

**Vector**

 * скачивает и распаковывает Vector;
 * создает конфигурацию через Jinja2;
 * создает systemd service;
 * запускает Vector.

**LightHouse**

 * устанавливает Nginx;
 * скачивает LightHouse;
 * настраивает Nginx через Jinja2;
 * запускает веб-сервер.

**Переменные**

Переменные для сервисов находятся в каталогах:
```
group_vars/clickhouse/
group_vars/vector/
group_vars/lighthouse/
```
Там задаются версии программ, пути установки и другие параметры.

**Проверка**

Проверка playbook:

`ansible-lint site.yml`

Проверка без внесения изменений:

`ansible-playbook -i inventory/prod.yml site.yml --check`

Запуск с отображением изменений:

`ansible-playbook -i inventory/prod.yml site.yml --diff`

При повторном запуске:

```
changed=0
failed=0
```

Это подтверждает идемпотентность playbook.

**Теги**

Отдельные Ansible tags в playbook не использовались.

Для запуска отдельных групп можно использовать:

```
ansible-playbook -i inventory/prod.yml site.yml --limit clickhouse
ansible-playbook -i inventory/prod.yml site.yml --limit vector
ansible-playbook -i inventory/prod.yml site.yml --limit lighthouse
```
