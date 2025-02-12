# Ошибка сохранения артефакта

## Лог ошибки и описание
```
uploading artifacts as "archive" to coordinator 500 Internal Server Error id=69026 responceStatus=500 Internal Server Error status 500 context=artifacts-uploader error=invalid argument.

```

## Способы решения проблемы

1. Проверить права доступа к файлу
Запустите перед artifacts: команду:

```
yaml
Копировать
Редактировать
before_script:
  - ls -lh build/
  - chmod 644 build/*.jar
```

Иногда Runner может не иметь прав на чтение файла.

2. Попробовать явно указать файл
Если wildcard *.jar не работает, попробуйте указать полный путь:


```yaml
Копировать
Редактировать
artifacts:
  paths:
    - build/myfile.jar
```

(Замените myfile.jar на фактическое имя файла)

3. Попробовать find с artifacts:
Иногда .gitlab-ci.yml не обрабатывает *.jar корректно. Используйте find:

```
```yaml
before_script:
  - mv $(find build/ -name "*.jar") build/output.jar
artifacts:
  paths:
    - build/output.jar
```
Это переименует .jar в гарантированно существующий файл.

4. Проверить размер файла
Возможно, .jar больше, чем GitLab позволяет загружать:


```yaml
before_script:
  - ls -lh build/*.jar
```
Если .jar слишком большой, попробуйте уменьшить его, используя zip или tar:

```yaml
before_script:
  - tar -czf build/artifact.tar.gz build/*.jar
artifacts:
  paths:
    - build/artifact.tar.gz
```
5. Попробовать другой Runner (если есть)
Если у вас несколько Runner'ов, попробуйте запустить этот job на другом Runner'е. Возможно, проблема связана с конкретным окружением.

6. Проверить переменные окружения
Добавьте в before_script вывод всех переменных:

```yaml
before_script:
  - env | sort
```
Иногда переменные среды (CI_PROJECT_DIR, CI_JOB_ID и др.) могут влиять на пути артефактов.

***Вывод***
Если wildcard *.jar не работает, попробуйте явно указать путь или переименовать файл.
Если файл слишком большой, попробуйте заархивировать его перед загрузкой.
Если Runner не имеет доступа к файлу, проверьте права (chmod 644).
Если проблема остаётся только на одном Runner'е, попробуйте запустить job на другом Runner'е.
