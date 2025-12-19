# Интструкция по созданию и настройке проекта rss-puzzle

## 1. Создание проекта с помощью Vite
   1. Перейти в корень репозитория, ветка main
   2. Создать от ветки main новую ветку chore/RSS-PZ-35_CreateProjectAndInstallPackages
      ```
      git checkout -b chore/RSS-PZ-35_CreateProjectAndInstallPackages
      ```
   3. Запустить создание проекта rss-puzzle  с помощью команды:
      ```
      npm create vite@latest rss-puzzle
      ```
   4. Выбрать при создании следующие пункты:
      - Vanilla
      - TypeScript
      - No
      - Yes
   5. Перейти в созданную папку rss-puzzle:
      ```
      cd rss-puzzle
      ```
   6. Заменить содержимое файла tsconfig.json на следующий тексе:
      ```json
      {
        "compilerOptions": {
          "types": ["vite/client"],
          "allowImportingTsExtensions": true,
          "noEmit": true,
          "strictNullChecks": true,
          "strict": true,
          "noImplicitAny": true
        }
      }
      ```
   7. Закоммитить изменения
      ```
        git add -A
        git commit -m "chore: create project"
      ```
