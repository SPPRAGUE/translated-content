---
title: Безопасность веб-приложения Django
slug: Learn_web_development/Extensions/Server-side/Django/web_application_security
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/Server-side/Django/Deployment", "Learn/Server-side/Django/django_assessment_blog", "Learn/Server-side/Django")}}

Защита пользовательских данных - важная часть проектирования любого веб-сайта.Ранее мы рассматривали некоторые наиболее распространённые угрозы безопасности в теме [Веб безопасность](/ru/docs/Web/Security). В данной статье будет представлена практическая демонстрация того, как встроенные механизмы защиты Django's обрабатывают подобные угрозы.

| Требования: | Прочитайте тему [Веб безопасность](/ru/docs/Web/Security). Завершите изучение предыдущих частей руководства до [Руководство часть 9: Работа с формами](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Forms) включительно. |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Цель:       | Понять, что нужно делать (или наоборот не делать), для обеспечения безопасности вашего веб-приложения.                                                                                                                                   |

## Обзор

Тема [Веб безопасность](/ru/docs/Web/Security) рассматривает значение безопасности веб-приложения для проектирования серверного приложения и некоторые из наиболее распространённых угроз, от которых вам может потребоваться защита. Одна из ключевых идей этой темы состоит в том, что практически все атаки будут успешны, если веб-приложение доверяет пользовательским данным (например данным из браузера).

> **Предупреждение:** **Важно:** Наиболее важный урок, который вы должны усвоить, состоит в том - что никогда не стоит доверять переданным пользователем данным. Они включают в себя GET параметры в URL, тело POST запроса, HTTP заголовки, cookies, загруженные пользователем данные и т.д. Всегда проверяйте и обрабатывайте все входные данные. Всегда готовьтесь к худшему.

Хорошей новостью для всех разработчиков, использующих Django, является то, что большинство известных атак обрабатывается фреймворком! Статья [Безопасность в Django](https://docs.djangoproject.com/en/2.0/topics/security/) (Django docs) описывает методы обеспечения безопасности Django и стратегии защиты веб-приложения разработанного на данном фреймворке.

## Распространённые угрозы/методы защиты

Мы не будем дублировать документацию Django и в данной статье продемонстрируем некоторые основные методы обеспечения безопасности в контексте разрабатываемого в данном руководстве приложения [LocalLibrary](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

### Межсайтовый скриптинг (XSS)

XSS это термин, применяющийся для описания класса атак, позволяющего атакующему, через веб-сайт внедрить скрипты, которые будут выполнены на устройстве зашедшего на страницу пользователя. Часто это происходит через сохранение вредоносного кода в базе данных, откуда данный код будет возвращён и выполнен для запросившего некие данные пользователя (типичный пример - сохранение тега \<script> с вредоносным кодом в комментарии, который может увидеть другой пользователь). Другой вектор атаки - в том чтобы сгенерировать определённую ссылку, при клике на которую пользователь запустит выполнение некоего замаскированного кода JavaScript в своём браузере.

Система шаблонов Django защищает от большинства XSS-атак, [экранируя определённые символы](https://docs.djangoproject.com/en/2.0/ref/templates/language/#automatic-html-escaping), считающиеся "опасными" в HTML. Мы можем продемонстрировать это, попытавшись внедрить произвольный JavaScript-код в наше приложение LocalLibrary через форму добавления автора, созданную в [Руководство часть 9: Работа с формами](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Forms).

1. Запустите веб-сайт, используя сервер разработки (`python3 manage.py runserver`).
2. Откройте сайт в вашем браузере и войдите под аккаунтом супер-пользователя.
3. Перейдите на страницу добавления автора (она должна быть доступна по URL: [`http://127.0.0.1:8000/catalog/author/create/`](http://127.0.0.1:8000/catalog/author/create/)).
4. Введите данные об имени и фамилии, датах рождения и смерти автора. Затем добавьте следующую строку в поле фамилии:
   `<script>alert('Test alert');</script>`.
   ![Author Form XSS test](author_create_form_alert_xss.png)

   > [!NOTE]
   > Это безобидный скрипт, который, если будет выполнен, отобразит окно с сообщением "Test alert" в вашем браузере. Если данное окно отображается при открытии страницы с созданной подобным образом записью - значит сайт уязвим перед атаками XSS.

5. Нажмите **Submit** для сохранения записи.
6. После сохранения автора - он должен быть отображён, как показано ниже. Так как сработала защита от XSS - команда `alert()` не будет запущена. Вместо этого скрипт будет отображаться как обычный текст.![Author detail view XSS test](author_detail_alert_xss.png)

Если вы посмотрите исходный HTML код, вы увидите, что "опасные" символы - например такие как скобки тегов - были заменены на их безопасные эквивалентные html сущности (к примеру `>` на `&gt;`)

```html
<h1>
  Author: Boon&lt;script&gt;alert(&#39;Test alert&#39;);&lt;/script&gt;, David
  (Boonie)
</h1>
```

Использование шаблонов Django защищает вас от большинства XSS-атак. Однако существует возможность отключения данной защиты, при котором экранирование не будет автоматически применятся ко всем полям, которые не должны будут заполнятся пользователем(к примеру, поле `help_text` обычно заполняется не пользователем, поэтому Django не будет экранировать его значение).

Так же XSS-атаки могут быть осуществлены через другие ненадёжные источники данных, такие как cookies, сторонние сервисы или загруженные файлы (и прочие источники, данные которых не были специально обработаны перед отображением на странице). Если вы отображаете данные из этих источников, вы должны добавить ваш собственный обработчик для фильтрации данных.

### Межсайтовая подделка запроса (CSRF)

CSRF атаки позволяют атакующему выполнять действия от имени другого пользователя без его согласия. Например, предположим что есть хакер, который хочет добавить авторов в наше приложение LocalLibrary.

> [!NOTE]
> Очевидно, что наш хакер делает это не ради денег! Более амбициозные хакеры могут использовать описываемый подход для выполнения более опасных задач (например, переводы денег пользователей на их личные счета и т.д).

Для того, чтобы сделать это, хакер может создать HTML файл, подобный продемонстрированному ниже, который будет содержать форму создания автора (похожую на ту, что мы разрабатывали в предыдущих частях руководства), которая будет отправлена как только данный файл будет загружен в браузер. Хакер отправит данный файл всем Библиотекарям и будет ждать пока кто-либо из них откроет файл (он содержит только безобидную информацию, честно!). Если файл будет открыт любым залогиненным пользователем, с правами Библиотекаря - тогда форма будет отправлена от его имени и создаст нового пользователя.

```html
<html>
  <body onload="document.EvilForm.submit()">
    <form
      action="http://127.0.0.1:8000/catalog/author/create/"
      method="post"
      name="EvilForm">
      <table>
        <tr>
          <th><label for="id_first_name">First name:</label></th>
          <td>
            <input
              id="id_first_name"
              maxlength="100"
              name="first_name"
              type="text"
              value="Mad"
              required />
          </td>
        </tr>
        <tr>
          <th><label for="id_last_name">Last name:</label></th>
          <td>
            <input
              id="id_last_name"
              maxlength="100"
              name="last_name"
              type="text"
              value="Man"
              required />
          </td>
        </tr>
        <tr>
          <th><label for="id_date_of_birth">Date of birth:</label></th>
          <td>
            <input id="id_date_of_birth" name="date_of_birth" type="text" />
          </td>
        </tr>
        <tr>
          <th><label for="id_date_of_death">Died:</label></th>
          <td>
            <input
              id="id_date_of_death"
              name="date_of_death"
              type="text"
              value="12/10/2016" />
          </td>
        </tr>
      </table>
      <input type="submit" value="Submit" />
    </form>
  </body>
</html>
```

Запустите веб-сервер разработки и войдите в аккаунт супер-пользователя. Скопируйте приведённый выше текст в файл и затем откройте его в браузере. Вы должны получить CSRF ошибку, потому что у Django есть защита от атак данного вида!

Механизм защиты заключается в том, что вы добавляете тег шаблона `{% csrf_token %}` в вашу форму. Этот токен будет отображён в вашем HTML как показано ниже, со значением, уникальным для каждого запрашивающего форму пользователя.

```html
<input
  type="hidden"
  name="csrfmiddlewaretoken"
  value="0QRWHnYVg776y2l66mcvZqp8alrv4lb8S8lZ4ZJUWGZFA5VHrVfL2mpH29YZ39PW" />
```

Django генерирует уникальный для пользователя/браузера токен и отклоняет все формы, которые не содержат его или содержат его неверное значение.

Для продолжения использования данного вида атак, хакер теперь должен найти и добавить верный CSRF токен для каждого выбранного целью пользователя. Это означает, что хакер теперь не может использовать массовые рассылки одного вредоносного файла всем Библиотекарям, так как для каждого из них CSRF токен будет уникальным.

Защита Django от CSRF атак по умолчанию включена. Вам всегда следует использовать тег`{% csrf_token %}` в ваших формах и использовать `POST` для запросов, которые могут изменить или добавить данные в вашу базу данных.

### Другие атаки

Django так же предоставляет защиту от других видов атак ( демонстрация большинства из которых была бы сложна новичкам для понимания и не слишком полезна ):

- Защита от SQL инъекции
  - : Уязвимость SQL инъекции позволяет атакующему выполнить произвольный SQL код в базе данных и получить доступ к данным (прочитать, отредактировать и изменить) независимо от текущих прав доступа пользователя. В большинстве случаев вы будете получать доступ к данным базы данных, используя сущности queryset/model Django. Используя их для генерации SQL запросов, вы получите корректно сформированный и экранированный запрос для выбранной базы данных. Если вам необходимо писать "сырые" запросы, вам так же нужно будет продумать защиту от инъекций.
- Защита от Кликджекинга
  - : В данном виде атак атакующий перехватывает ввод на видимом слое страницы и перенаправляет их на скрытый слой под ним. Этот метод может быть использован к примеру для отображения официального сайта банка, с перехватом данных для входа в невидимом [`<iframe>`](/ru/docs/Web/HTML/Element/iframe), который контролирует атакующий. Django содержит [защиту от кликджекинга](https://docs.djangoproject.com/en/2.0/ref/clickjacking/#clickjacking-prevention) в виде `промежуточного програмного обеспечения (middleware) X-Frame-Options,` который поддерживается браузерами и может запретить отображение страницы внутри [`<iframe>`](/ru/docs/Web/HTML/Element/iframe).
- SSL/HTTPS
  - : SSL/HTTPS может быть использован на веб-сервере для шифрования всего трафика между сервером и пользователем, включая данные входа, которые иначе будут отправляться как обычный текст (который сможет прочитать любой перехвативший запрос человек). Использование HTTPS высоко рекомендовано. Если используется HTTPS, Django позволяет использовать следующие методы защиты:

<!---->

- [`SECURE_PROXY_SSL_HEADER`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECURE_PROXY_SSL_HEADER) может быть использовано для проверки что всегда используется безопасное подключение, даже если данные поступают из прокси, использующего протокол отличный от HTTP.
- [`SECURE_SSL_REDIRECT`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECURE_SSL_REDIRECT) используется для перенаправления всех запросов с HTTP на HTTPS.
- Используйте [HTTP Strict Transport Security](https://docs.djangoproject.com/en/2.0/ref/middleware/#http-strict-transport-security) (HSTS). Этот HTTP заголовок информирует браузер о том, что все последующие запросы должны всегда использовать HTTPS. Совместно с перенаправлением HTTP запросов на HTTPS, эта опция позволяет обеспечить использование HTTPS в каждом запросе. HSTS может так же быть настроен опциями [`SECURE_HSTS_SECONDS`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECURE_HSTS_SECONDS) и [`SECURE_HSTS_INCLUDE_SUBDOMAINS`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECURE_HSTS_INCLUDE_SUBDOMAINS) или на веб-сервере.
- Используйте 'безопасные' cookies выставив [`SESSION_COOKIE_SECURE`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SESSION_COOKIE_SECURE) и [`CSRF_COOKIE_SECURE`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-CSRF_COOKIE_SECURE) в `True`. Это позволит обеспечить пересылку данных cookies только через протокол HTTPS.

<!---->

- Валидация заголовка Host
  - : Используйте [`ALLOWED_HOSTS`](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-ALLOWED_HOSTS) чтобы принимать только запросы от доверенных хостов.

Так же существует множество других техник защиты и указаний по их использованию. Мы надеемся, что данная статья дала вам понимание, какие техники Django предлагает для обеспечения безопасности. Мы надеемся, что вы продолжите изучение этого вопроса по документации Django.

## Подводим итоги

Django имеет методы обеспечения защиты от распространённых видов атак, включая XSS и CSRF атаки. В данной статье мы продемонстрировали, как различные виды атак обрабатываются Django на примере нашего приложения _LocalLibrary_. Мы так же кратко рассмотрели другие виды уязвимостей и методы защиты от них.

Это было очень краткое погружение в вопрос веб-безопасности. Мы крайне рекомендуем вам прочитать [Безопасность в Django](https://docs.djangoproject.com/en/2.0/topics/security/) для более глубокого понимания.

Следующим и последним шагом в данном руководстве будет выполнение [самостоятельной работы](/ru/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog).

## Смотрите также

- [Безопасность в Django](https://docs.djangoproject.com/en/2.0/topics/security/) (Django docs)
- [Веб безопасность](/ru/docs/Web/Security) (MDN)
- [Безопасность вашего сайта](/ru/docs/Web/Security/Practical_implementation_guides) (MDN)

{{PreviousMenuNext("Learn/Server-side/Django/Deployment", "Learn/Server-side/Django/django_assessment_blog", "Learn/Server-side/Django")}}

## In this module

- [Django introduction](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)
- [Setting up a Django development environment](/ru/docs/Learn_web_development/Extensions/Server-side/Django/development_environment)
- [Django Tutorial: The Local Library website](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website)
- [Django Tutorial Part 2: Creating a skeleton website](/ru/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)
- [Django Tutorial Part 3: Using models](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Models)
- [Django Tutorial Part 4: Django admin site](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)
- [Django Tutorial Part 5: Creating our home page](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Home_page)
- [Django Tutorial Part 6: Generic list and detail views](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views)
- [Django Tutorial Part 7: Sessions framework](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Sessions)
- [Django Tutorial Part 8: User authentication and permissions](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Authentication)
- [Django Tutorial Part 9: Working with forms](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Forms)
- [Django Tutorial Part 10: Testing a Django web application](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Testing)
- [Django Tutorial Part 11: Deploying Django to production](/ru/docs/Learn_web_development/Extensions/Server-side/Django/Deployment)
- [Django web application security](/ru/docs/Learn_web_development/Extensions/Server-side/Django/web_application_security)
- [DIY Django mini blog](/ru/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog)
