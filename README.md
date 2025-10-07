# ngx_http_substitutions_filter_module (Enhanced Fork)

HTTP‑модуль для Nginx, выполняющий подстановки в теле ответа: как фиксированные строки, так и регулярные выражения.
Форк основан на проекте [yaoweibin/ngx_http_substitutions_filter_module](https://github.com/yaoweibin/ngx_http_substitutions_filter_module) и добавляет ряд улучшений, включая сохранение заголовка `Last-Modified`.

## Описание

Модуль `ngx_http_substitutions_filter_module` позволяет заменять строки или совпадения по регулярным выражениям в теле HTTP-ответа.

Фильтр работает на этапе **body filter** и сканирует ответ построчно, обеспечивая замену текста до отдачи клиенту.
При обработке сжатых ответов (`gzip`, `deflate`) фильтр не выполняет замену и пропускает тело без изменений.

## Изменения в этом форке

* Добавлена директива **`subs_filter_last_modified`**, позволяющая сохранить исходный заголовок `Last-Modified`, если тело ответа модифицируется фильтром.

## Директивы

### `subs_filter`

**Синтаксис:**

```nginx
subs_filter <regex_or_string> <replacement> [options];
```

**По умолчанию:** нет

**Контекст:** `http`, `server`, `location`

Описание:

* `<regex_or_string>` — шаблон поиска (строка или регулярное выражение);
* `<replacement>` — текст замены;
* `[options]` — модификаторы:

  * `g` — глобальный режим (все совпадения);
  * `o` — только первое совпадение;
  * `i` — без учёта регистра;
  * `r` — шаблон рассматривается как регулярное выражение.

### `subs_filter_types`

**Синтаксис:**

```nginx
subs_filter_types <mime/type> [mime/type ...];
```

**По умолчанию:** `text/html`

**Контекст:** `http`, `server`, `location`

Используется для указания типов контента, которые должны проверяться фильтром, помимо `text/html`.
По умолчанию фильтр применяется только к `text/html`.

### `subs_filter_bypass`

**Синтаксис:**

```nginx
subs_filter_bypass <variable>;
```

**По умолчанию:** нет

**Контекст:** `http`, `server`, `location`

Если значение указанной переменной не пустое или не равно `0`, фильтр не выполняет замену в ответе.

### `subs_filter_last_modified` *(новая директива)*

**Синтаксис:**

```nginx
subs_filter_last_modified on | off;
```

**По умолчанию:** `off`

**Контекст:** `http`, `server`, `location`

Определяет, нужно ли сохранять заголовок `Last-Modified`, если тело ответа модифицируется.
При `on` заголовок сохраняется.

## Установка

1. Клонировать репозиторий форка:

   ```bash
   git clone https://github.com/yourname/ngx_http_substitutions_filter_module.git
   ```
2. Собрать вместе с Nginx:

   ```bash
   ./configure --add-module=/path/to/ngx_http_substitutions_filter_module
   make && make install
   ```
3. Или собрать как динамический модуль:

   ```bash
   ./configure --add-dynamic-module=/path/to/ngx_http_substitutions_filter_module
   make modules
   ```
4. Подключить модуль:

   ```nginx
   load_module modules/ngx_http_subs_filter_module.so;
   ```

## Лицензия

BSD 2-Clause License.

Copyright (c) 2014, Weibin Yao

Copyright (c) 2025, Alexander Popov / Relines
