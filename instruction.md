# Интструкция по созданию и настройке проекта rss-puzzle

## 1. Создание проекта с помощью Vite
   1. Перейти в корень репозитория, ветка `main`
   2. Создать от ветки `main` новую ветку `rss-puzzle`
      ```
      git checkout -b rss-puzzle
      ```
   3. Создать от ветки `rss-puzzle` новую ветку `chore/RSS-PZ-34_CreateProjectAndInstallPackages`
      ```
      git checkout -b chore/RSS-PZ-34_CreateProjectAndInstallPackages
      ```
   4. Запустить создание проекта `rss-puzzle` с помощью команды:
      ```
      npm create vite@latest rss-puzzle
      ```
   5. Выбрать при создании следующие пункты:
      - Vanilla
      - TypeScript
      - No
      - Yes
   6. Перейти в созданную папку `rss-puzzle`:
      ```
      cd rss-puzzle
      ```
   7. Создать в папке `rss-puzzle` файл `vite.config.js` и поместить в него следующий текст:
      ```
      import { defineConfig } from 'vite';

      export default defineConfig({
        base: './',
        build: {
          minify: false,
        },
        css: {
          modules: {
            localsConvention: 'camelCaseOnly',
          },
        },
      });
      ```
   8. Удалить из `tsconfig.json` строчку
      ```
      "erasableSyntaxOnly": true,
      ```
   9. Файлы, сгенеророванные `vite`, лучше не удалять, чтобы на следующем шаге можно было понять заработали ли `lint`, `prettier` и `typescript`
   10. Закоммитить изменения
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
   5. Заменить в файле `eslint.config.mjs` текст
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
  6. В файле `eslint.config.mjs` добавить в массив `jsConfig` после `...configs.base.recommended,` следующий объект
     ```
       {
         linterOptions: {
           noInlineConfig: true,
         },
       },
     ```
  7. Добавить правила для Typescript:
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
  8. В папке `rss-puzzle` создать файл `.prettierrc.json` и скопировать в него
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
  9. Прописать скрипты:
     ```
     "format": "prettier src --write",
     "ci:format": "prettier src --check",
     "lint": "npx eslint src",
  10. Запустить форматтер:
      ```
      npm run format
      ```
  11. Удалить содержимое файла `main.ts` и файлы `counter.ts` и `typescript.svg`
  12. Закоммитить изменения:
      ```
      git add -A
      git commit -m "chore: add eslint and prettier"
      ```
## 3. Установка stylelint and commitlint (по желанию)
  1. Установить пакет `stylelint` следующей командой
     ```
     npm create stylelint@latest
     ```
  2. В файле `stylelint.config.mjs` в массив extends добавить `stylelint-config-clean-order`
  3. В файле `package.json` прописать следующие скрипты
     ```
     "stylelint": "npx stylelint src/**/*.css",
     "stylelint:fix": "npx stylelint src/**/*.css --fix",
     ```
  4. Установить пакет `commitlint` следующей командой
     ```
     npm install -D @commitlint/cli @commitlint/config-conventional
     ```
  5. Настроить `commitlint` следующей командой
     ```
     echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
     ```
  6. Закоммитить изменения
     ```
     git add -A
     git commit -m "chore: add stylelint and commitlint"
     ```
## 4. Установка validate-branch-name
  1. Установить `validate-branch-name
     ```
     npm i -D validate-branch-name
     ```
  2. В папке `rss-puzzle` создать файл `.validate-branch-namerc.json` и в него вставить следующий текст
     ```json
     {
       "pattern": "^(init|feat|fix|refactor|docs|style|chore)\/RSS-PZ-[0-9]{2}_[a-zA-Z]+$",
       "errorMsg": "Incorrect branch name. Example: feat/RSS-PZ-01_addNewFeature"
     }

     ```
  3. Закоммитить изменения
     ```
     git add -A
     git commit -m "chore: add validate-branch-name"
     ```
## 5. Установка lint-staged
  1. Установить пакет `lint-staged`
     ```
     npm install --save-dev lint-staged
     ```
  2. В папке `rss-puzzle` создать файл `.lintstagedrc.json` и в негоо вставить следующий текст
     ```json
     {
       "src/**/*.ts": ["npm run ci:format"],
       "src/**/*.css": ["npm run stylelint"],
       "src/**/*.html": ["npm run ci:format"]
     }
     ```
  3. Если пакет `stylelint` не устанавливался, то удалить строку `"src/**/*.css": ["npm run stylelint"],`
  4. Закоммитить изменения
     ```
     git add -A
     git commit -m "chore: add lint-staged"
     ```
## 6. Установка hasky
  1. Установить пакет `husky`
     ```
     npm install --save-dev husky
     ```
  2. Создать в файле `package.json` следующий скрипт 
     ```
     "prepare": "cd .. && husky rss-puzzle/.husky",
     ```
  3. Запустить скрипт `prepare`. В папке `rss-puzzle` должна появиться папка `.husky`
     ```
     npm run prepare
     ```
  4. В папке `.husky` создать файл `pre-commit` и в него вставить следующий текст
     ```
     cd rss-puzzle
     npx lint-staged
     ```
  5. В папке `.husky` создать файл `pre-push` и в него вставить следующий текст
     ```
     cd rss-puzzle
     npm run lint
     npx validate-branch-name
     ```
  6. Если был установлен пакет `commmitlint`, то в папке `.husky` создать файл `commit-msg` и в него вставить следующий текст
     ```
     cd rss-puzzle
     npx --no-install commitlint --edit "$1"
     ```
  7. Закоммитить изменения
     ```
     git add -A
     git commit -m "chore: add husky"
     ```
## Установка gh-pages
  1. Установить пакет `gh-pages`
     ```
     npm install -D gh-pages
     ```
  2. Создать в файле `package.json` следующий скрипт
     ```
     "deploy": "npm run build && gh-pages -d dist -e rss-puzzle"
     ```
  3. Закоммитить изменения
     ```
     git add -A
     git commit -m "chore: add gh-pages"
     ```
