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


# 2. Создать репозиторий GitHub Pages

Зайти на https://pages.github.com/ и выполнить инструкции интерактивного помошника.

При установке нужно оставить такие настройки:
![[20250518091037.png]]


# 3. Синхронизировал заметки Obsidian с репозиторием


````bash
git clone --filter=blob:none --no-checkout https://github.com/SMAnatoly/sag-quartz.git
git sparse-checkout set content
git checkout origin/v4
git pull origin v4


git checkout v4
git branch -u origin/v4 v4

git status

>create file

git add content/test.md
git commit -m "Add new files to content folder"

git push origin v4
````







