---
title: git-sync
---
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