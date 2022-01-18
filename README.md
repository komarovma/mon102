<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="____1002___0"></a>Домашнее задание к занятию “10.02. Системы мониторинга”</h1>
<h1 class="code-line" data-line-start=1 data-line-end=2 ><a id="__1"></a>Обязательные задания</h1>
<pre><code>Работаю на Windows 10 PRO +Docker Dekstop с итеграцией в WSL 2.0 (Ubuntu)
Все скрины лежат в репозитории
10_2_1.PNG Скриншот веб-интерфейса ПО chronograf (http://localhost:8888).
10_2_2.PNG Скриншот с отображением метрик утилизации места на диске (disk-&gt;host-&gt;telegraf_container_id) из веб-интерфейса.
10_2_3.PNG Скриншот с отображением метрик Docker.
10_2_4.PNG Скриншот Dashboards.
</code></pre>
<ol>
<li class="has-line-data" data-line-start="11" data-line-end="12">Опишите основные плюсы и минусы pull и push систем мониторинга.</li>
</ol>
<pre><code>Прежде всего, Push and Pull описывают метод передачи данных, который не влияет на содержание передачи. Другими словами, до тех пор, пока push может нести информацию, pull, безусловно, может нести ту же информацию, например данные мониторинга, такие как «загрузка ЦП 30%», независимо от того, является ли это pull или push, содержание передачи остается тем же, и оно не изменится из-за режима передачи. В результате объем сообщения чрезвычайно велик, поэтому пропускная способность сети, потребляемая двумя методами, не будет сильно отличаться.

Push-модель – когда сервер мониторинга ожидает подключений от агентов для получения метрик;
Классика, SCOM,Tivoly и прочие корпоративные системы.Агент может пушить на несколько серверов для отказоустойчивости.
В общем как правило реализуеться в корпоративной среде.

Pull-модель – когда сервер мониторинга сам подключается к агентам мониторинга и забирает данные;
Популярна в последнее время особенно для динамических хостов (Docker) и облачных решений.

В общем, выбор системы зависит от ситуации.
</code></pre>
<ol start="2">
<li class="has-line-data" data-line-start="25" data-line-end="33">
<p class="has-line-data" data-line-start="25" data-line-end="26">Какие из ниже перечисленных систем относятся к push модели, а какие к pull? А может есть гибридные?</p>
<pre><code> - Prometheus – PUSH/PULL. В основе лежит PULL-метод, но тот же push gateway использует PUSH, а Prometheus забирает инфу уже PULL запросами.
 - TICK - PUSH/PULL. Telegraf использует PUSH для передачи данных в хранилище, Kapacitor получает данные по PULL.
 - Zabbix - PUSH/PULL. Активные проверки используют метод PUSH, PULL – подключение к ресурсам, опрос агентов и тп.
 - VictoriaMetrics - PUSH/PULL. Основной сбор метрик по PULL-модели, но vmagent умеет получать метрики и по push-модели (от (Telegraf, statsd и т.п.)
 - Nagios – PULL, использует опрос по snmp
</code></pre>
</li>
</ol>
<p class="has-line-data" data-line-start="33" data-line-end="35">3.Склонируйте себе репозиторий и запустите TICK-стэк, используя технологии docker и docker-compose.<br>
В виде решения на это упражнение приведите выводы команд с вашего компьютера (виртуальной машины):</p>
<pre><code>- curl http://localhost:8086/ping
- curl http://localhost:8888
- curl http://localhost:9092/kapacitor/v1/ping
А также скриншот веб-интерфейса ПО chronograf (http://localhost:8888).

C:\Users\komar&gt;curl http://localhost:8086/ping -v
*   Trying 127.0.0.1:8086...
* Connected to localhost (127.0.0.1) port 8086 (#0)
&gt; GET /ping HTTP/1.1
&gt; Host: localhost:8086
&gt; User-Agent: curl/7.79.1
&gt; Accept: */*
&gt;
* Mark bundle as not supporting multiuse
&lt; HTTP/1.1 204 No Content
&lt; X-Influxdb-Build: OSS
&lt; X-Influxdb-Build: oss2
&lt; X-Influxdb-Version: 2.1.1
&lt; X-Influxdb-Version: 2.1.1
&lt; Date: Tue, 18 Jan 2022 17:04:13 GMT
&lt;
* Connection #0 to host localhost left intact


C:\Users\komar&gt;curl http://localhost:8888 -v
*   Trying 127.0.0.1:8888...
* Connected to localhost (127.0.0.1) port 8888 (#0)
&gt; GET / HTTP/1.1
&gt; Host: localhost:8888
&gt; User-Agent: curl/7.79.1
&gt; Accept: */*
&gt;
* Mark bundle as not supporting multiuse
&lt; HTTP/1.1 200 OK
&lt; Accept-Ranges: bytes
&lt; Cache-Control: public, max-age=3600
&lt; Content-Length: 336
&lt; Content-Security-Policy: script-src 'self'; object-src 'self'
&lt; Content-Type: text/html; charset=utf-8
&lt; Etag: &quot;336820331&quot;
&lt; Last-Modified: Fri, 08 Oct 2021 20:33:01 GMT
&lt; Vary: Accept-Encoding
&lt; X-Chronograf-Version: 1.9.1
&lt; X-Content-Type-Options: nosniff
&lt; X-Frame-Options: SAMEORIGIN
&lt; X-Xss-Protection: 1; mode=block
&lt; Date: Tue, 18 Jan 2022 17:04:58 GMT
&lt;
&lt;!DOCTYPE html&gt;&lt;html&gt;&lt;head&gt;&lt;meta http-equiv=&quot;Content-type&quot; content=&quot;text/html; charset=utf-8&quot;&gt;&lt;title&gt;Chronograf&lt;/title&gt;&lt;link rel=&quot;icon shortcut&quot; href=&quot;/favicon.fa749080.ico&quot;&gt;&lt;link rel=&quot;stylesheet&quot; href=&quot;/src.3dbae016.css&quot;&gt;&lt;/head&gt;&lt;body&gt; &lt;div id=&quot;react-root&quot; data-basepath=&quot;&quot;&gt;&lt;/div&gt; &lt;script src=&quot;/src.fab22342.js&quot;&gt;&lt;/script&gt; &lt;/body&gt;&lt;/html&gt;* Connection #0 to host localhost left intact


C:\Users\komar&gt;curl http://localhost:9092/kapacitor/v1/ping -v
*   Trying 127.0.0.1:9092...
* Connected to localhost (127.0.0.1) port 9092 (#0)
&gt; GET /kapacitor/v1/ping HTTP/1.1
&gt; Host: localhost:9092
&gt; User-Agent: curl/7.79.1
&gt; Accept: */*
&gt;
* Mark bundle as not supporting multiuse
&lt; HTTP/1.1 204 No Content
&lt; Content-Type: application/json; charset=utf-8
&lt; Request-Id: 92b79657-7886-11ec-8009-000000000000
&lt; X-Kapacitor-Version: 1.6.2
&lt; Date: Tue, 18 Jan 2022 17:46:32 GMT
&lt;
* Connection #0 to host localhost left intact</code></pre>