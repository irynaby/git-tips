### Начало работы

Создание нового репозитория, первый коммит, привязка удалённого репозитория с gthub.com, отправка изменений в удалённый репозиторий.

``` bash
# указана последовательность действий:
git init # инициируем гит в этой папке
touch readme.md # создаем файл readme.md
git add readme.md # делаем этот файл отслеживаемым
git commit -m "Первый коммит" # создаем первый коммит с вменяемым названием
git remote add origin git@github.com:nicothin/test.git # добавляем предварительно созданный пустой удаленный репозиторий
git push origin master # отправляем данные из локального репозитория в удаленный (в ветку master)
```


### Обычный рабочий процесс

Создание нового репозитория на github.com, клонирование к себе, работа, периодическая «синхронизация с github.com».

``` bash
# указана последовательность действий:
# создаём на github.com репозиторий, копируем его URL клонирования (SSH)
# в консоли попадаем в свою папку для всех проектов
git clone АДРЕС_РЕПОЗИТОРИЯ ПАПКА_ПРОЕКТА # клонируем удалённый репозиторий к себе на компьютер (если не указать ПАПУ_ПРОЕКТА, будет создана папка, совпадающая по имени с названием репозитория)
cd ПАПКА_ПРОЕКТА # переходим в папку проекта (указана команда для git bash)
# редактируем файлы, добавляем файлы и/или папки (если удаляем файлы — см. секцию про удаление файлов)
git add . # делаем все новые файлы в этой папке отслеживаемыми и готовыми к коммиту
git commit -m "НАЗВАНИЕ_КОММИТА" # создаем коммит с вменяемым названием
git push origin master # отправляем данные из локального репозитория в удаленный (в ветку master)
# снова вносим какие-то изменения (если удаляем файлы — см. секцию про удаление файлов)
# возвращаемся к шагу с git add . и проходим цикл заново
```

### Внесение изменений в коммит

``` bash
# указана последовательность действий:
subl inc/header.html # редактируем и сохраняем разметку «шапки»
git add inc/header.html # индексируем измененный файл
git commit -m "Убрал телефон из шапки" # делаем коммит
# ВНИМАНИЕ: коммит пока не был отправлен в удалённый репозиторий
# сознаём, что нужно было еще что-то сделать в этом коммите
# вносим изменения
git add inc/header.html # индексируем измененный файл (можно git add .)
git commit --amend -m "«Шапка»: выполнена задача №34 (вставить-вынуть)" # заново делаем коммит
```

### Работа с ветками

``` bash
# указана последовательность действий:
git checkout -b new_page_header # создадим новую ветку для задачи изменения «шапки» и перейдём в неё
subl inc/header.html # редактируем и сохраняем разметку «шапки»
git commit -a -m "Новая шапка: смена логотипа" # делаем первый коммит (работа еще не завершена)
# тут выясняется, что есть баг с контактом в «подвале»
git checkout master # возвращаемся к ветке master
git checkout -b footer_hotfix # создаём ветку (основанную на master) для решения проблемы
subl inc/footer.html # устраняем баг и сохраняем разметку «подвала»
git commit -a -m "Исправление контакта в подвале" # делаем коммит
git checkout master # переключаемся в ветку master
git merge footer_hotfix # вливаем в master изменения из ветки footer_hotfix
git branch -d footer_hotfix # удаляем ветку footer_hotfix
git checkout new_page_header # переключаемся в ветку new_page_header для продолжения работ над «шапкой»
subl inc/header.html # редактируем и сохраняем разметку «шапки»
git commit -a -m "Новая шапка: смена навигации" # делаем коммит (работа над «шапкой» завершена)
git checkout master # переключаемся в ветку master
git merge new_page_header # вливаем в master изменения из ветки new_page_header
git branch -d new_page_header # удаляем ветку new_page_header
```


### Работа с ветками, конфликт слияния

Есть master (публичная версия сайта), в двух параллельных ветках (branch_1 и branch_2) было отредактировано одно и то же место одного и того же файла, первую ветку (branch_1) влили в master, попытка влить вторую вызывает конфликт.

``` bash
# указана последовательность действий:
git checkout master # переключаемся на ветку master
git checkout -b branch_1 # создаём ветку branch_1, основанную на ветке master
subl . # редактируем и сохраняем файлы
git commit -a -m "Правка 1" # коммитим (теперь имеем 1 коммит в ветке branch_1)
git checkout master # возвращаемся к ветке master
git checkout -b branch_2 # создаём ветку branch_2, основанную на ветке master
subl . # редактируем и сохраняем файлы
git commit -a -m "Правка 2" # коммитим (теперь имеем 1 коммит в ветке branch_2)
git checkout master # возвращаемся к ветке master
git merge branch_1 # вливаем изменения из ветки branch_1 в текущую ветку (master), удача (автослияние)
git merge branch_2 #  вливаем изменения из ветки branch_2 в текущую ветку (master), КОНФЛИКТ автослияния
# Automatic merge failed; fix conflicts and then commit the result.
subl . # выбираем в конфликтных файлах те участки, которые нужно оставить, сохраняем
git commit -a -m "Устранение конфликта" # коммитим результат устранения конфликта
```


### Синхронизация репозитория-форка с мастер-репозиторием

Есть некий репозиторий на github.com, он него нами был сделан форк, добавлены какие-то изменения. Оригинальный (мастер-) репозиторий был как-то обновлён. Задача: стянуть с мастер-репозитория изменения (которые там внесены уже после того, как мы его форкнули).

``` bash
# указана последовательность действий:
git remote add upstream git@github.com:address.git # добавляем удаленный репозиторий: сокр. имя — upstream, URL мастер-репозитория
git fetch upstream # качаем все ветки мастер-репозитория, но пока не сливаем со своими
git checkout master # переключаемся на ветку master своего репозитория
git merge upstream/master # вливаем ветку master удалённого репозитория upstream в свою ветку master
```


### Ошибка в работе: закоммитили в мастер, но поняли, что нужно было коммитить в новую ветку (ВАЖНО: это сработает только если коммит еще не отправлен в удалённый репозиторий)

``` bash
# указана последовательность действий:
# сделали изменения, проиндексировали их, закоммитили в master, но ЕЩЁ НЕ ОТПРАВИЛИ (не делали git push)
git checkout -b new-branch # создаём новую вертку из master
git checkout master # переключаемся на master
git reset HEAD~ --hard # жОско сбрасываем состояние master
git checkout new-branch # переключаемся обратно на новую ветку
```

### Нужно вернуть содержимое файла к состоянию, бывшему в каком-либо коммите (известна SHA коммита)

``` bash
# указана последовательность действий:
git checkout f26ed88 -- index.html # указана SHA коммита, к состоянию которого нужно вернуть файл и имя файла
git status # изменения внесены в файл, файл сразу проиндексирован
git diff --staged # показать изменения в файле
git commit -am "Navigation fixs" # коммит
```


### При любом действии с github (или другим удалённым сервисом) запрашивается логин/пароль

Речь именно о запросе пароля, а не ключевой фразы.

``` bash
# указана последовательность действий:
git remote -v # показать список удалённых репозиториев с адресами (у проблемного будет адрес по https), предположим, это origin
git remote add origin git@github.com:address.git # добавляем удаленный репозиторий, сокр. имя — origin
# если возникает ошибка добавления с сообщением о том, что origin «уже задан», то: 
git remote rm origin # удаляем привязанный удалённый репозиторий
git remote add origin git@github.com:address.git # добавляем удаленный репозиторий, сокр. имя — origin
```