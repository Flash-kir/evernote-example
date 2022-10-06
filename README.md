# evernote-example

Программа предназначена для работы с evernote API.
Позволяет просматривать созданные записи, блокноты и добавлять запись по шаблону.

## Установка

Клонируйте реппозиторий:

    git clone git@github.com:Flash-kir/evernote-example.git
  
Выполните команду для установки требуемых библиотек в окружение:

    pip install -r requirements.txt

Установите  `evernote3` по [инструкции](https://github.com/evernote/evernote-sdk-python3).

## Настройка

1. Выполните регистрацию на тестовом сервере [sandbox.evernote](https://sandbox.evernote.com/Registration.action).
При регистрации вам выдадут `evernote consumer key` и `evernote consumer secret`.
1. Для запуска кода потребуется получить [ключ разработчика](https://sandbox.evernote.com/api/DeveloperToken.action).
1. Для создания файла с приватными настройками выполните команду:

    cp example.env .env
    
1. В файле `.env` заполните переменные:

- EVERNOTE_CONSUMER_KEY - имя пользователя, полученное в пункте 1.
- EVERNOTE_CONSUMER_SECRET - секретный ключ, полученный в пункте 1.
- EVERNOTE_PERSONAL_TOKEN - ключ разработчика, полученный в пункте 2 вида 'S=s1:U=96c33:E=18afbbd6118:C=183a40c3518:P=1cd:A=en-devtoken:V=2:H=92...'
- JOURNAL_TEMPLATE_NOTE_GUID - идентификационный номер заметки, с которой будет сниматься копия, вида: '801a6f37-d537-4060-a338-1b145bbcd98f', может быть получен при выполнении `dump_inbox.py`
- JOURNAL_NOTEBOOK_GUID - идентификационный номер блокнота, получаемый при выполнении `list_notebooks.py` '5363d8a6-e2b7-482d-a2ca-a771b61a581d'
- INBOX_NOTEBOOK_GUID=''
- IS_SANDBOX - если работа ведется с тестовым сервером `sandbox.evernote.com` присваиваем `True`, если на продуктовом сервере `evernote.com` присваиваем `False`.

Для запуска на продуктовом сервере потребуется выполнить одно из действий:
- активировать ключ разработчика через [форму](https://dev.evernote.com/support/faq.php#). Ссылка на [документацию](https://dev.evernote.com/support/faq.php#activatekey) по копированию ключа с тестового на продуктовый сервер.
- либо (зарегистрироваться)[https://www.evernote.com/Registration.action?analyticsLoginOrigin=login_action] на продуктовом сервере и получить ключ разработчика (там)[https://www.evernote.com/api/DeveloperToken.action]

## config.py

Программа описывает и настраивает файл конфигурации.

## dump_inbox.py

Программа получает список записей пользователя и выводит их в консоль. 
При запуске можно указать ключ `number` - число выводимых записей, по умолчанию 10 штук.

    usage: dump_inbox.py [-h] [number]

    Dumps notes from Evernote inbox to console

    positional arguments:
      number      number of records to dump

    optional arguments:
      -h, --help  show this help message and exit

## list_notebooks.py

Программа получает список блокнотов пользователя и выводит в консоль их `guid` и имена.

## add_note2journal.py

Программа копирует шаблон, `guid` которого указан в настройках в параметре `JOURNAL_TEMPLATE_NOTE_GUID`, и сохраняет запись в новую заметку в блокноте, `guid` которого указан в настройках в параметре `JOURNAL_NOTEBOOK_GUID`.
При разработке можно указать `guid` любой созданной заметки и создастся ее копия.

    Traceback (most recent call last):
      File "add_note2journal.py", line 58, in <module>
        utitle_without_comment = new_note.title.decode('utf8').split('#', 1)[0]
    AttributeError: 'str' object has no attribute 'decode'

При запуске можно указать ключ `date` в формате `YYYY-MM-DD` - дата создаваемой заметки, по умолчанию берется текущий день.

    usage: add_note2journal.py [-h] [date]

    Adds note to notebook "Дневник", uses template note

    positional arguments:
      date        date in format "YYYY-MM-DD"

    optional arguments:
      -h, --help  show this help message and exit
