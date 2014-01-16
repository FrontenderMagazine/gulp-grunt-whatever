# Gulp, Grunt, Whatever

[Gulp][01] — это недавно появившаяся система сборки, которая внушает большие
надежды. Он дает возможность фактически за 10 минут написать [действительно лаконичный код][02].
That's a lot to say when pitching it against Grunt, which is [quite larger][03] than that. 

Помните Grunt? Я [писал о нем достаточно много][04] в прошлом, и я [даже написал книгу][05],
в которой я предлагаю использовать Grint как инстромент для сборки. Это
потрясающий инструмент, вы должны как-нибудь обязательно его попробовать.

[Некоторые люди][06] считают, что Gulp намного лучше Grunt, и за Gulp последует
много людей. Я считаю, что это [возмутительно вопиюще нелепо][07]. Это не так
уж и очевидно, но у Grunt есть много плюсов по отношению к Gulp, как и у Gulp
по отношению к Grunt.

В этой статье я намерен **объяснить что такое Gulp**, так как это достаточно
новый инструмент, который появился около 6-ти месяцев назад. Затем, _я хочу
сравнить его с Grunt_, и показать, какой инструмент что делает лучше и почему.

[![gulp.png][08]][01]

[01]: http://gulpjs.com/ "Gulp Streaming Build System" 
[02]: https://github.com/gulpjs/gulp/ "gulpjs/gulp on GitHub"
[03]: https://github.com/gruntjs/grunt "gruntjs/grunt on GitHub"
[04]: /search/tagged/grunt "Posts tagged 'grunt' on Pony Foo"
[05]: http://bevacqua.io/buildfirst "JavaScript Application Design: A Build First Approach"
[06]: http://www.100percentjs.com/just-like-grunt-gulp-browserify-now/ "And just like that Grunt and RequireJS are out, itâ€™s all about Gulp and Browserify now"
[07]: http://www.youtube.com/watch?v=kRhW_zMlf0Y "Jackie Chiles on Seinfeld"
[08]: http://i.imgur.com/yFeBvMO.png "gulp.png"

# Введение в [Gulp][1]

Возможно, вы захотите пропустить эту часть, [прочитав README][2], хотя, я бы
порекомендовал вам посмотреть в исходный код Gulp, там не много сюрпризов, скорее
просто хорошо продуманный и краткий код.

Как вы могли заметить в документации, установка Gulp очень напоминает, то
как это происходит с Grunt.

- Создать `package.json`
- `npm install -g gulp`
- `npm install --save-dev gulp`
- Создать `gulpfile.js`
- `gulp`

At this point, the main difference with Grunt is that they didn't take their 
**CLI** out of core (???) _(at least not yet)_, which might confuse you if you don't 
[understand global installs][3] in `npm`.

Перед тем, как мы перейдем к простому gulpfile., давайте посмотрим, как выглядит
API Gulp. Здесь есть 5 методов, и, вобщем-то, это все, что вам понадобиться. 
Это потрясающий лаконичный API, который действительно помогает Gulp фокусироваться
на главном — обработке задач сборки.

- `.src(globs[, options])` принимает [glob][4] и возвращает входной поток
- `.dest(path)` принимает путь к файлу, и возвращает исходящий поток
- `.task(name[, deps], fn)` определяет задачу
- `.run(tasks...[, cb])` запускает задачу
- `.watch(glob [, opts], cb)` следит за изменениями в файловой системе

Вы можете сказать, этот API просто великолепен! Посмотрим на простую задачу из
документации Gulp — компилирование Jade-шаблонов. Здесь `jade()` и `minify()` —
плагины Gulp — мы вернемся к этому через минуту. 

    js gulp.src('./client/templates/*.jade')
      .pipe(jade()) .pipe(minify())
      .pipe(gulp.dest('./build/minified_templates'));

Здесь не так уж и много шаблонного кода, потому что мы здесь просто пишем код, 
а не собирает объект-конфигурацию. 

В Grunt вам понадобиться немного шаблонного кода для того, чтобы выполнить
то же самое. К примеру, загрузка `npm` модулей с помощью `grunt.loadNpmTasks`,
создание ссылки на задачу, которая содержит список требуемых тасков, и
конфигурация каждой задачи так, как нам нужно. Одна из проблем с Grunt,
которую решает Gulp — это единый монолитный конфигурационный объект, заставляющий
вас прыгать через обручи, чтобы добиться желаемых результатов.

Если ваш процесс сборки заключается в копировании файл и минифицирует что-то еще,
плюс копирует еще один файл не относящийся ни к чему файл, то задача `copy` будет
задача `copy` будет иметь две абсолютно несвязанных операции копирования, хотя
и с различными целями (???)

Gulp делает хорошую работу, показывая как код вместо конфигурации может помочь
предотвратить такой сценарий, в котором конфигурация начинает сбивать с толку.

## Каков `gulp` на вкус

Рассматривая этот короткий пример `gulpfile.js`, адаптированный из документации
к Gulp.

    var gulp = require('gulp'); 
    var uglify = require('gulp-uglify'); 

    gulp.task('scripts', function() { 
      // Minify and copy all JavaScript (except vendor scripts) 
      gulp.src(['client/js/**/*.js', '!client/js/vendor/**']) 
        .pipe(uglify()) 
        .pipe(gulp.dest('build/js'));

        // Copy vendor files 
        gulp.src('client/js/vendor/**') 
          .pipe(gulp.dest('build/js/vendor')); 

    }); 

    // The default task (called when you run `gulp`) 
    gulp.task('default', function() { 
      gulp.run('scripts'); 

      // Watch files and run tasks if they change 
      gulp.watch('client/js/**', function(event) { 
        gulp.run('scripts'); 
      }); 
    });

Это выглядит чертовски читаемо, даже если вы не знакомы с потоковой системой
передачи данных в NodeJS, не так ли? Я уверен, что это гораздо читаемее, чем
`Gruntfile.js`, который бы делал те же самые вещи, потому что в этом случае
мы можем просто пробежаться глазами по коду и понять, что здесь происходит. 
Это гораздо легче, чем в случае с GruntJS. Вы можете избавиться от комментариев,
которые вполне очевидны, и вы получите лаконичный `gulpfile.js`

Тот факт, что Gulp обеспечивает реализацию метода `.watch` как часть своего
основного API также обнадёживает, потому как он является ключевой частью
функциональности, которая в процессе разработки имеет много значения в системе
сборки.  Поддержка [разработки асинхронных задач][5] кажется гораздо лучше
интеграрованной в Gulp чем в Grunt, где цели(targets) действительно усложняют
работу при передаче данных в задачи. 

Говоря в общем, то API, которое есть у Gulp обеспечивает более понятное и
простое использование, чем Grunt. Это более-менее аргумент для Gulp. **Простой,
лаконичный, потрясающий** API. Простые плагины, которые выполняют только одну
простую задачу, и не делают все, что им кажется нужным, как это принято
в большинстве плагинов Grunt. Но у Gulp тоже есть недостатки 
(Not everything is pink roses for Gulp, though, and there are a few downsides to it as well.)

## Я выбираю `/Gr?u(nt|lp)/`

Давайте посмотрим, где сравнение между этими таск-менеджерами ломается. Gulp is
streams all the way down, almost as if you were shell scripting. That is, if
you "get" [Node streams][6]. Otherwise, you're going to have a bad time.

[![streams.jpg][7]][8]

Если вы node-персона. Трудно игнорировать смелость, с которой Gulp строит механизм
сборки вокруг кода, вместо конфигурации, как это делает Grunt. Однако, это и
неоспоримый недостаток Gulp. Некоторые люди просто не понимают потоки NodeJS.
Это могут быть PHP-разработчики, или какие-то другие сервеные вуду, типа Ruby или
Python. Они могут быть не знакомы с потоками или буферами NodeJS. Они может быть и
знают CommonJS, но едва ли они зашли в этом дальше своей зоны комфорта. Такие
люди никогда откажутся от Grunt в пользу Gulp.

> В то время, как Gulp легче для чтения, Grunt легче для написания, и ингогда
> это имеет большее значение.


Gulp ориентирован на сборку, в особенности _на работу с файлами_. На этом,
в сущности, и основан API Gulp: «In, Out, Watch». Это здорово, потому что
позволяет сконцентрироваться на задачах. Сборке. В этом и есть идея — вы
получаете что-то на вход: а затем что-то возвращаете(output), используя некоторые
преобразования, которые придают форму результату. Но в этом есть минус, потому
что те вещи, которые не относятся к процессу сборки _с таким API становятся труднее_. 
К примеру, [отправка уведомления об окончании сборки][9] или запуск 
[Amazon EC2-серверов][10] противоречит тому, для чего был разработан Gulp.

Gulp очень новый, и мы можем видеть, как развивается его эко-система, но я
не жду, что разработка задач будет более популярна, чем у GruntJS (???). Я думаю,
что это хорошо, я е верю что Gulp сможет побить Grunt в непрерывной интеграционнии
и в области деплоя, тогда как я полагаю, что Gulp полностью возьмет на себя более
простые воркфлоу которые не заходят дальше, чем сборка клиетской части, и 
публикация на Heroku.

## Gulp не победит Grunt

Вот несколько причин, почему я верю, что Gulp не бросит Grunt умирать от
обезвоживания в пустыне. Но, во всяком случае он привлечет больше внимания
к нему, расширяя границы того, что сейчас могут сделать JavaScript таск-менеджеры.
Во-первых, Gulp не собирается побеждать Grunt. По крайней мере, ни у кого нет
такой цели, и точно этой цели нет у лидеров индустрии.

<blockquote class="twitter-tweet" lang="en">
  <p>Grunt vs. Gulp: both serve different needs &amp; can coexist happily. 
     See: <a href="http://t.co/fQ5G8HDNwl">http://t.co/fQ5G8HDNwl</a> &amp;&amp; <a href="https://t.co/joUVeRqj8T">https://t.co/joUVeRqj8T</a>. 
     We&#39;ll support both
  </p>
  &mdash; Addy Osmani (@addyosmani) <a href="https://twitter.com/addyosmani/statuses/420870024379637760">January 8, 2014</a>
</blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>



Можно воспринимать это как новость, но это не должен быть сюрприз. Много
усилий было вложено в текущее состояние Grunt, и едва ли кто-то бросит их,
просто для того, чтобы портировать их все в новую горячую цыпочку в городке,
вы должны сделать выбор. Придерживаться того, что есть у вас, или пуститься
в погоню за популярной и таинственной блондинкой.

Во-вторых, как я уже говорил раньше, в Gulp есть определенный порог входа,
которого нет у Grunt, без опыта в nodejs у вас будут трудности со stream, pipes,
buffers, ассинхронным JavaScript в целом (промисы, колбеки, да с чем угодно!),
и я просто не вижу как gulp может просочиться в среду далеких от nodejs
людей, и применяться там как система для сборки фронт-енда. Учитывая все эти
факторы.

Кроме того, Gulp **не режает каждой новой проблемы**. Серьезно. API потрясающ
и целенаправлен, но он вызывает затруднения в задачах не касающихся сборки,
в этом побеждает Grunt. [Больше 2000 плагинов Grunt][11] зарегистрировано в `npm`,
против [~200][12] написанных для Gulp. Интересно видеть возможность как Gulp 
может запускать задачи Grunt, но я сомневаюсь, что это приживется. Я сомневаюсь,
что `/Gr?u(nt|lp)/` сделает вашу жизнь легче. Вряд ли. Если вам кажется, 
что вам нужны обе системы сборки, то _вероятно, вам стоит остановиться на Grunt_.

![regex.png][13] 

Здесь также рассматривался фактор скорости работы. Я оставлю вас обдумать это
самостоятельно. Важно понять, что здесь нет всеобъемлющего ответа. Gulp может
быть быстрее, возможности Grunt могут быть более обширными, но в конце концов,
вы предпочтете один другому. Не используйте оба в одном приложении, не будьте как
этот парень.

<blockquote class="twitter-tweet" lang="en">
  <p>just switched a project from <a href="https://twitter.com/search?q=%23gruntjs&amp;src=hash">#gruntjs</a> to <a href="https://twitter.com/search?q=%23gulpjs&amp;src=hash">#gulpjs</a> - simpler code, &amp; build time on save during watch from 3-5sec to 10-20ms. I kid you not.</p>
  &mdash; Andy Joslin (@andytjoslin) <a href="https://twitter.com/andytjoslin/statuses/416424985662468097">December 27, 2013</a>
</blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

И еще один фактор стоит учитывать. Grunt за все эти дни не подал никакого вида.
Это заставляет волноваться, и это нужно брать во внимание.

## The boar is becoming kind of stale 

Grunt might _drown on its own_. It sat on `0.4.1` for ages, before moving to an
unimpressive `0.4.2` release, and it doesn't seem to be going places now, either.
Activity on the [@gruntjs][14] Twitter account is kind of flat-lining these days,
and that's not a good sign, either.

I'm really hoping this is just transitional as planning for `0.5.0` is underway,
but I feel like the team moved on to other projects. While I wouldn't consider
it abandoned, it's a concern that I haven't seen raised yet. What I'd love to
see is an eventual `1.0.0` release with a re-imagined configuration structure
that deals with _the problems we've experimented thus far_. Easier plugin loading,
a watch mechanism in core similar to what Gulp did, simpler file description
semantics, and a reduced overhead for configuring tasks in general.

Of course, it's easy to _want_ those things, but it's hard to implement them
without breaking most of the existing 2000 plugins. Considering the plugin
ecosystem is one of Grunt's most valuable assets, it'll be hard to get right
a release plan that's both sensible and meaningful. We'll just have to sit and
wait, or you might want to [go ahead and propose something][17] to be
implemented in `0.5.0`. 

## The case for doing nothing 

Right wing UNIX extremists have [time][18] and [again][19] suggested doing nothing.
Forget about Gulp, Grunt, whatever. Just do nothing. I don't agree with this sort
of extremism, you might just be more comfortable writing everything in JavaScript.
It does, however, hold some merit in its premise. In the case of Gulp, I do
consider the `npm run` approach as a valid questioning of its purpose. 

Gulp is pretty close to doing "nothing", a la `npm run`, while at the same time it 
kind of does "something", like Grunt does. I think Gulp provides value in providing
Windows support, but it does introduce a certain amount of complexity, so it's
really a trade-off. You need to ask yourself what you're looking for. If it's
just the simplicity, you might be better off just using `npm run`!

The case for Windows support might not hold a lot of meaning within the Node
community itself, since most of us seem to be working on _*nix_, but it does
become a factor in other communities, which Grunt [seems to be penetrating][20].
I agree you should use some flavor of `bash` for Windows, it's still a pain doing
just about anything in the command-line, and there isn't really much to say in
favor of _not using_ Grunt on Windows. 

[![nothing.png][15]][16] 

So use Gulp, use Grunt, _whatever_. 

## Whatever, _But_ 

Grunt wins at teaching people how to do builds, and even then, it's pretty hard
to put it [in terms anyone can understand][21], but it fails at keeping it short.
Gulp wins at being terse and having a gorgeous API, but it fails at the entry level,
because of streams being hard to grasp at first. In the low-risk low-gain corner
we have `npm run`. It wins at not doing anything, resulting in no overhead, but
it fails at being cross-platform, if that's something that worries you.

Make a choice by yourself, don't just pick something _because XYZ said so_. Pick
the tool which works for you. The one you understand, are comfortable with. Above
all, the one **that fits your needs**. Don't go blindly chasing the latest fad
because someone else tells you to. Similarly, don't get stuck with monolithic
jQuery applications _(just to give out an example)_, try something else. Innovate.
Be the change you want to see in the world. 
> **Be the change you want to see in the world.** 

I need a drink. 

[1]: http://gulpjs.com/ "Gulp Streaming Build System" 
[2]: https://github.com/gulpjs/gulp/ "gulpjs/gulp on GitHub" 
[3]: http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation/ "npm 1.0: Global vs Local installation" 
[4]: https://github.com/isaacs/node-glob "isaacs/node-glob on GitHub" 
[5]: https://github.com/gulpjs/gulp#async-task-support "Asynchronous Task Support in Gulp" 
[6]: http://www.youtube.com/watch?v=lQAV3bPOYHo "Harnessing the awesome power of streams" 
[7]: http://i.imgur.com/pnEIqGO.jpg [8]: http://www.youtube.com/watch?v=lQAV3bPOYHo "Harnessing the awesome power of streams" 
[9]: https://npmjs.org/package/grunt-winston "grunt-winston can log data over different transports" 
[10]: https://github.com/bevacqua/grunt-ec2 "grunt-ec2 creates, deploys to, and shuts down Amazon EC2 instances" 
[11]: http://gruntjs.com/plugins "Grunt Plugin Listing" 
[12]: http://gratimax.github.io/search-gulp-plugins/ "Search for Gulp Plugins" 
[13]: http://i.imgur.com/Ih0Y1Zw.png "To generate #1 albums, 'jay --help' recommends the -z flag." 
[14]: https://twitter.com/gruntjs "@gruntjs on Twitter" 
[15]: http://img.youtube.com/vi/EQnaRtNMGMI/0.jpg 
[16]: http://www.youtube.com/watch?v=EQnaRtNMGMI "The Nothing Pitch" 
[17]: https://github.com/gruntjs/grunt/issues?milestone=7 "Grunt issues for the 0.5.0 milestone" 
[18]: http://substack.net/task_automation_with_npm_run "task automation with npm run" 
[19]: https://gist.github.com/substack/8313379 "introducing ./task.js, THE new javascript task runner automation framework" 
[20]: https://npmjs.org/search?q=grunt-php "although I've no factual information to support that claim" 
[21]: http://24ways.org/2013/grunt-is-not-weird-and-hard/ "Grunt for People Who Think Things Like Grunt are Weird and Hard""
