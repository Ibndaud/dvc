# 📦 Версионирование данных с DVC

Практика из курса **karpov.courses — «Управление моделями»**, модуль «Версионирование данных (DVC)».

## Зачем это нужно

Git отлично версионирует код, но плохо подходит для больших файлов данных. **DVC (Data Version Control)** решает это: в git попадают только лёгкие `.dvc`-файлы с контрольными суммами (md5), а сами данные хранятся в удалённом хранилище и скачиваются командой `dvc pull`.

```
git repo                    remote storage
├── data/
│   ├── mydata.txt.dvc   ──►  mydata.txt     (сам файл данных)
│   └── mydata_2.txt.dvc ──►  mydata_2.txt
└── .dvc/config             (описание remote-хранилища)
```

## Что сделано

- инициализирован DVC-репозиторий поверх git;
- два файла данных (`mydata.txt`, `mydata_2.txt`) поставлены под версионирование через `dvc add`;
- настроен remote — **Google Drive** (в истории коммитов видно, как remote переводился с S3 на GDrive);
- данные отправлены в remote через `dvc push`.

## Основные команды

```bash
dvc init                        # инициализация DVC в git-репозитории
dvc add data/mydata.txt         # поставить файл под версионирование
dvc remote add -d myremote gdrive://<folder_id>/   # настроить remote
dvc push                        # отправить данные в remote
dvc pull                        # скачать данные версии из текущего коммита
dvc checkout                    # переключиться на версию данных из git-checkout
dvc status                      # проверить, соответствуют ли данные .dvc-файлам
```

## Как воспроизвести

```bash
git clone https://github.com/Ibndaud/dvc.git
cd dvc
dvc pull    # скачает данные из remote (нужен доступ к Google Drive)
```

> ⚠️ Remote в этом учебном репозитории — личная папка Google Drive: `dvc pull` сработает после авторизации в своём GDrive. Для своих экспериментов настройте свой remote (`dvc remote add`).

## Структура

```
├── data/
│   ├── mydata.txt.dvc      # метаданные: md5, размер, путь
│   └── mydata_2.txt.dvc
├── .dvc/
│   ├── config              # настройки remote (Google Drive)
│   └── .gitignore          # кэш DVC не попадает в git
├── .dvcignore              # исключения для DVC (аналог .gitignore)
└── LICENSE
```

## Лицензия

[MIT](LICENSE)
