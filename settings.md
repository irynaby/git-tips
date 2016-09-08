# Настройка GIT
 
 * Файл `/etc/gitconfig` содержит значения, действующие для всех пользователей системы и всех их репозиториев. 
 Указав параметр `--system` при запуске git config, вы добьетесь чтения и записи для этого конкретного файла.
 
 * Файл `~/.gitconfig` или `~/.config/git/config` связан с конкретным пользо- вателем. 
 Чтение и запись для этого файла инициируются передачей параметра `--global`. 
 
 * Параметры конфигурационного файла в папке Git (то есть .git/config) репозитория, с которым вы работаете в данный момент, действуют только на конкретный репозиторий.
 
 Настройки каждого следующего уровня переопределяют настройки предыдущего, то есть конфигурация из .git/config перекроет конфигурацию из /etc/gitconfig.
 
 ## Идентификатор
 ```sh
 git config --global user.name "John Doe"
 git config --global user.email johndoe@example.com
  ```
 ## Проверка настроек
 ```sh
 git config --list
 ```
 Результат:
  >user.name=John Doe
  >user.email=johndoe@example.com
  >color.status=auto
  >color.branch=auto
  >color.interactive=auto
  >color.diff=auto
  >...
 
 Можно проверить значение конкретного ключа: git config <ключ>
 ```sh
 git config user.name
 ```
  John Doe
 
 ## Получение справочной информации
  ```sh
  git help <команда>
  git <команда> --help
  man git-<команда>
  ```
  
  К примеру, справка по команде config открывается так:  git help config