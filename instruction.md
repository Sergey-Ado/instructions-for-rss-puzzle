# Интструкция по созданию и настройке проекта rss-puzzle

## 1. Создание проекта с помощью Vite
   1. Перейти в корень репозитория, ветка `main`
   2. Создать от ветки `main` новую ветку `chore/RSS-PZ-35_CreateProjectAndInstallPackages`
      ```
      git checkout -b chore/RSS-PZ-35_CreateProjectAndInstallPackages
      ```
   3. Запустить создание проекта `rss-puzzle` с помощью команды:
      ```
      npm create vite@latest rss-puzzle
      ```
   4. Выбрать при создании следующие пункты:
      - Vanilla
      - TypeScript
      - No
      - Yes
   5. Перейти в созданную папку `rss-puzzle`:
      ```
      cd rss-puzzle
      ```
   6. Заменить содержимое файла `tsconfig.json` на следующий тексе:
      ```json
      {
        "compilerOptions": {
          "types": ["vite/client"],
          "strictNullChecks": true,
          "strict": true,
          "noImplicitAny": true
        }
      }
      ```
   7. Файлы, сгенеророванные `vite`, лучше не удалять, чтобы на следующем шаге можно было понять заработали ли `lint`, `prettier` и `typescript`
   8. Закоммитить изменения
      ```
        git add -A
        git commit -m "chore: create project"
      ```
## 2. Установка eslint, prettier и typescript
   1. Запустить
      ```
      npx create-airbnb-x-config
      ```
   2. При установке выбрать следующие пункты:
      - Extended
      - Yes
      - Yes
      - Node
      - Yes
      - Typescript
      - Yes
      - No
   3. Если нет необходимости в линтинге ноды, то удалить строки 32-37:
      ```
      const nodeConfig = [
        // Node Plugin
        plugins.node,
        // Airbnb Node Recommended Config
        ...configs.node.recommended,
      ];
      ```
      и строки 71-72:
      ```
        // Node Config
        ...nodeConfig,
      ```
   4. Установить предлагаемые пакеты с нодой
      ```
      npm install -D eslint @eslint/compat @eslint/js eslint-config-airbnb-extended eslint-import-resolver-typescript typescript-eslint prettier eslint-plugin-prettier eslint-config-prettier @stylistic/eslint-plugin@^3.1.0 eslint-plugin-import-x eslint-plugin-n
      ```
      или без ноды
      ```
      npm install -D eslint @eslint/compat @eslint/js eslint-config-airbnb-extended eslint-import-resolver-typescript typescript-eslint prettier eslint-plugin-prettier eslint-config-prettier @stylistic/eslint-plugin@^3.1.0 eslint-plugin-import-x
      ```
   5. Заменить в файле eslint.config.mjs тексе
      ```
        // Prettier Config
        {
          name: 'prettier/config',
          rules: {
            ...prettierConfigRules,
            'prettier/prettier': 'error',
          },
        },
      ```
      на текст
      ```
        // Prettier Config
        {
          name: 'prettier/config',
          rules: {
            ...prettierConfigRules,
            'prettier/prettier': [
              'error',
              {
                singleQuote: true,
                endOfLine: 'auto',
              },
            ],
          },
        },
      ```
  6. Добавить правила для Typescript:
     ```
        // Strict TypeScript Config
        rules.typescript.typescriptEslintStrict,
        {
          rules: {
           '@typescript-eslint/explicit-function-return-type': 'error',  // по желанию
           '@typescript-eslint/explicit-member-accessibility': 'error',  // по желанию
           'max-lines-per-function': ['error', 40],
          },
        },

     ```
  7. Создать файл .prettierrc.json и скопировать в него
     ```
        {
           "tabWidth": 2,
           "useTabs": false,
           "singleQuote": true,
           "semi": true,
           "bracketSpacing": true,
           "arrowParens": "avoid",
           "trailingComma": "es5",
           "bracketSameLine": true,
           "printWidth": 80,
           "endOfLine": "auto"
         }
     ```
  8. Прописать скрипты:
     ```
     "format": "prettier src --write",
     "ci:format": "prettier src --check",
     "lint": "npx eslint src",
  9. Запустить форматтер:
      ```
         npm run format;
      ```
  10. Удалить файлы по умолчанию
  11. Закончить изменения:
      ```
      git add -A
      git commit -m "chore: add eslint and prettier"
      ```
