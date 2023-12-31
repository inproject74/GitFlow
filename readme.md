## Основные команды в консоли.

**pwd** - узнать в какой мы папке

**cd** *имя папки* - перейти в нужную папку
**сd ~** - перейти в домашнюю папку
**cd /** - перейти в корневую папку

**ls**, **ls -lh** (полная версия в читаемом формате)
**ls -a** (просмотр скрытых файлов) - просмотр списка файлов и папок

**touch** *имя файла* - создать файл
**touch** *file1* *file2* - создать несколько файлов

**mkdir** *имя папки* - создать папку

**rm** *file* - удалить файл

**rmdir** *dir* - удалить пустую папку
**rm -r** *dir* - удалить директорию с файлами

**cp** *file.txt dir/* - копировать файл в папку dir
**mv** *file.txt dir/* - перенести файл в папку dir


Автоматическое дополнение по клавише **tab**

Начните вводить команду → **tab** → автоматически появится полное имя команды (то же при наборе имени файла, папки)

Команды можно вводить одной строкой, соеденив при помощи **&&** (логического **и**)


## Git

Git - система контроля версий для отслеживания изменений проектов

**git init** - создать репозиторий в текущей папке

**git status** - просмотр статуса репозитория

**rm -rf .git** - удалить репозиторий (разгитить)


**git add** - добавить в репозитарий

**git add --all** - добавить все файлы

**git add** *file* - добавить файл

**git add .** - добавить всю текущую папку


**git commit** - выполнить коммит

**git commit -m "Комментарий"** - коммит с коментарием

**git log** - вывести все коммиты


## GitHub

GitHub - платформа для хранения IT-проектов и совместной работы над ними с использованием Git.
Есть и другие платформы для работы с Git: GitLab (можно развернуть на сервере в локальной сети), Bitbucket - платформа совместной работы от создателя Jira (можно легко интегрировать в неё).

### Регистрируемся на GitHub

На главной странице **github.com** → **Sign up** → ввести **e-mail** → придумать пароль → ввести имя пользователя.
Пройдите проверку (капчу) → **Create account** → введите код, отправленный на **e-mail**.

### Создаём GitHub-репозиторий

На странице https://github.com/username (username - имя пользователя при регистрации) → перейти в раздел **Repositories** → **New** → ввести название (чтобы не путаться, лучше, чтобы совпадало с именем рапозитория на ПК) → Create repository.

### Создаём SSH-ключи для подключения к GitHub-репозиторию

Необходимо создать 2 ключа: приватный и публичный (**приватный** хранится только у владельца ключей, **публичный** доступен всем и используется для шифрования данных → **приватный** расшифровывает).

Ключи генерируются в домашней папке (обычно в папке .ssh).

**cd ~** - переходим в домашнюю папку
**ls -la .ssh/** - вывод списка ключей (если есть)

Если ключей нет, то всё в порядке.
Если есть, то нужно их удалить.

**ssh-keygen -t ed25519 -C "e-mail, который был указан при регистрации на GitHub"**
если система не получается, то
**ssh-keygen -t rsa -b 4096 -C "e-mail, который был указан при регистрации на GitHub"**
если система не поддерживает

Если ошибок нет, появится сообщение:
> Generating public/privat rsa key pair.

На дальнейшие вопросы жмём **Enter**. Проверяем наличие созданных ключей:

**ls -a ~/.ssh** - в списке файлов должны быть 2 файла: один с расширением *.pub*, другой без.

### Привязываем ключ к репозиторию на GitHub

**clip < ~/.ssh/id_rsa.pub** - копируем ключ в буфер

Ключ также можно вывести на экран
**cat ~/.ssh/id_rsa.pub**
и скопировать.

Затем переходим в GitHub → Setting → SSH and GPG keys → New SSH key.

В поле **Title** вводим название ключа (например, **Personal key**).
В поле **Key type** → **Authentication Key**.
В поле **Key** - вставляем скопированный ключ

**ssh -T git@github.com** - проверяем работу ключа

Если всё в порядке, появится сообщение:
**The authenticity of host 'github.com (140.82.121.4)' can't be established. ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. This key is not known by any other names. Are you sure you want to continue connecting (yes/no/[fingerprint])?**

Вводим **yes**, чтобы подтвердить ключ. Проверить ключ DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU предоставленный платформой можно [на GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints).

### Добавляем локальный репозиторий к репозиторию на GitHub

**cd ~/your-project** - переходим в папку проекта
**git remote add origin** `git@github.com:%ИМЯ_АККАУНТА%/your-project.git` - origin (псевдоним имени удалённого репозитория), ссылку `git@github.com:%ИМЯ_АККАУНТА%/your-project.git` можно получить на странице проекта (кнопка **Code** → SSH)

### Проверить, что репозитории связаны

**git remote -v**

Ожидаемое сообщение
`origin git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push)`

### Отправляем изменения на GitHub

**git push -u origin master** - если появляется ошибка, попробовать `git push -u origin main`
**git push** - команда обновления в дальнейшем


### Токен

GitHub → Настройки → Developer settings → Personal Access tokens → Tokens (classic) → Generate new token → Generate new token (classic)

`git remote set-url origin https://<token>@github.com/<username>/<repo>`
