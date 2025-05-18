---
title: Установка Quartz на github
draft: false
description: Мой субъективный опыт.
permalink: 
date: 2025-05-17
tags:
  - Quartz
  - Github
---

Давно думал над тем, что было бы неплохо заполучить подобие [Obsidian](https://obsidian.md/) в web. И вот внезапно такое приложение обнаружилось, называется [Quartz](https://quartz.jzhao.xyz/)


Для установки первым делом я попробовал воспользоваться инструкцией от [nicolevanderhoeven](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages)
Но позже немного ее модифицировал.

# 0. Prerequisits

- [NodeJS](https://notes.nicolevanderhoeven.com/NodeJS) последгюю версию (проверить версию `node -v`)
- [NPM](https://notes.nicolevanderhoeven.com/Node+Package+Manager) последгюю версию  (проверить версию `npm -v`)
- [Git](https://notes.nicolevanderhoeven.com/Git) (проверить версию `git --version`)
# 1. Установка Quartz на Ubuntu

Просто выполните эти команды в той директории, в которую хотите установить приложение.
````bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
````

Команда скачивает оригинальный репозиторий в выбранную локальную папку.
````bash
git clone https://github.com/jackyzha0/quartz.git 
````

Перейдите в папку, которая создалась после скачивания.
````bash
cd quartz
````

Команда скачивает все зависимости нужные для Quartz.
````bash
npm i
````

Команда запустит процесс конфигурирования приложения.
````bash
npx quartz create
````
# 2. Создание репозитория GitHub Pages

Зайти на https://pages.github.com/ и выполнить инструкции интерактивного помошника.

При установке нужно оставить такие настройки:
![[20250518091037.png]]
Для автоматичесмкого деплоя необходимо создать файл `quartz/.github/workflows/deploy.yml`

И скопировать в него следующее содержимое:
````yml
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
````

После этого необходимо настроить синхронизацию со своим репозиторием
````bash
# list all the repositories that are tracked
git remote -v
 
# if the origin doesn't match your own repository, set your repository as the origin
git remote set-url origin REMOTE-URL
 
# if you don't have upstream as a remote, add it so updates work
git remote add upstream https://github.com/jackyzha0/quartz.git
````

После этого отправить свою установленную версию в GitHub Pages
````bash
npx quartz sync --no-pull
````
# 3. Синхронизация заметок Obsidian с репозиторием

Если хочется синхронизировать только папку с публикуемым контентом, можно проделать следующие шаги на локальном компьютере

Настроить синхронизацию одной папки
````bash
git clone --filter=blob:none --no-checkout https://github.com/<your_name>/<your_name>.git
git sparse-checkout set content
git checkout origin/v4
git pull origin v4
git checkout v4
git branch -u origin/v4 v4
````

После этого изменения в этой папке можно отправлять следующим образом

Если локально создать новый файл test.md
````bash
git add content/test.md
git commit -m "Add new files to content folder"

git push origin v4
````

Наличие изменений для синхронизации можно проверить так:
````bash
git status
````



