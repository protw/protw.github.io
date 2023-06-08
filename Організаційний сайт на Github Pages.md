# Організаційний сайт на Github Pages

29-4-2019, [Link](https://protw.github.io/tw5/#%D0%9E%D1%80%D0%B3%D0%B0%D0%BD%D1%96%D0%B7%D0%B0%D1%86%D1%96%D0%B9%D0%BD%D0%B8%D0%B9%20%D1%81%D0%B0%D0%B9%D1%82%20%D0%BD%D0%B0%20Github%20Pages)

`Хостинг Тідлівікі`

Після того, як ми навчились створювати [статичні сайти на Github Pages](https://protw.github.io/tw5/#Автопублікація Tiddlywiki на Github Pages), майже напевно виникне потреба створити так званий організаційний сайт або сайт-портфоліо, що буде об'єднувати інформацію і надавати посилання на всі окремі проекти у Github.

Розберемо це на конкретному прикладі. Так, на моєму Github https://github.com/protw є два проекти `tw5` і `rder-uk`, які мають автозгенеровані Github Pages https://protw.github.io/tw5/ і https://protw.github.io/rder-uk/, відповідно.

Доменом для організаційного сайту для обох зазначених проектів природно буде `https://protw.github.io`.

Згідно до [документації Github](https://help.github.com/articles/user-organization-and-project-pages/#user--organization-pages) для створення простору для організаційного сайту потрібно організувати проект за такою схемою:

```
github.com/<user_name>/<user_name>.github.io
```

У нашому випадку проект матиме таке посилання:

https://github.com/protw/protw.github.io

На жаль [автоматизоване створення статичних сайтів](https://protw.github.io/tw5/#Автопублікація Tiddlywiki на Github Pages) на гілці `gh-pages` у випадку організаційного сайту не працює: процедури побудування на `travis-ci` з тими ж скриптами `.travis.yml` і `publish.sh` проходять успішно. Але зрештою на сайті https://protw.github.io/ відображається вміст гілки `master`, а не гілки `gh-pages`. Це, як можна зрозуміти, особливість організаційного сайту.

Іншими словами, для відображення за адресою `https://protw.github.io` статичні файли `index.html`, `static.html` та інші мають бути розташовані безпосередньо в гілці `master`: https://github.com/protw/protw.github.io.

Для вирішення цього питання процедура побудови статичних файлів організаційного сайту була організована на моєму локальному компі, де розташований git репозитарій цього сайту.

Відповідний скрипт під Windows розташований у фолдері, де власне розміщені тідлери організаційного сайту (у мене цей скрипт називається `staticm.bat` і знаходиться у фолдері `D:\protw\5060-protw-protw`), і виглядає таким чином:

```
if exist static\ del /s/q static\ > nul
if exist index.html del /s/q index.html > nul
if exist static.html del /s/q static.html > nul
if exist \temp1029384756 del /s/q \temp1029384756 > nul
if exist \temp1029384756 rd \temp1029384756 > nul

call tiddlywiki ./wiki --build

md \temp1029384756
xcopy /s/y wiki\output\*.* \temp1029384756 > nul
xcopy /s/y \temp1029384756\*.*  > nul
del /s/q \temp1029384756  > nul
rd \temp1029384756 > nul
del /s/q wiki\output > nul
```

Після побудови з допомогою цього скрипта `index.html`, `static.html` та інших статичних файлів потрібно стандартно застосувати git процедури `commit` і `push` до локального репозиторія для перенесення на Github. Після цього, організаційний сайт одразу запрацює за адресою [https://protw.github.io](https://protw.github.io/).

Запропонована схема побудови статичних сторінок для організаційного сайту відрізняється від схеми, запропонованої для окремих проектів. Окрім того вона не дуже елегантна. Але вона працює.