# qiita_trend_slack_notifier

`qiita_trend`gemを使用したSlack通知用のスクリプトです

※
[こちらの記事](https://qiita.com/dodonki1223/items/955819806297ee554b31)より引用

![SlackNotifySample](image/slack_notify_sample.png)

## 使用方法

### Slackアプリの登録

まずは通知させるための[Slack App](https://api.slack.com/apps)を作成します。  
[Slack App](https://api.slack.com/apps)リンクをクリックしてページに遷移して下さい。

![SlackAppページ](image/slack_api_app_page.png)

`Create New App`をクリックします。  

![SlackAppCreate](image/slack_api_app_create.png)

`App Name`,`Development Slack Warkspace`を記述して`Create App`をクリックします。

![SlackWebhookBotton](image/slack_webhook_button.png)

`Incoming Webhooks`をクリックします。  

![SlackWebhookSetting](image/slack_webhook_setting.png)

`Off→On`に変更し、`Add New Webhook to Workspace`をクリックします。

![SlackWebhookChannelSetting](image/slack_webhook_channel_setting.png)

`投稿先`を設定してインストールします。

![SlackWebhookUrl](image/slack_webhook_url.png)

`copy`をクリックすることでWebhook用のURLをコピーできます。  

### WebHookURLとQiitaにログインするためのユーザーとパスワードの設定を行う

`config.rb`を書き換えます
WeeklyとMonthlyを使用しない場合はユーザー名とパスワードの設定は必要ありません

```ruby
# frozen_string_literal: true

require 'qiita_trend'
require './qiita_notifier/configuration'

QiitaNotifier.configure do |config|
  # Qiitaでログイン出来るユーザー名とパスワードをセットしてください
  config.user_name = 'user_name' 　　　←　Qiitaにログインするユーザー名
  config.password = 'password' 　　　　←　Qiitaにログインするパスワード
  # Slackで作成したアプリのWebHookURLを設定してください
  config.web_hook_url = 'web_hook_url' ←　設定したWebHookURL
  # config.channel = '' ⇐ channelを任意選択したい場合に設定してください
end

QiitaTrend.configure do |config|
  config.user_name = QiitaNotifier.configuration.user_name
  config.password = QiitaNotifier.configuration.password
end
```

### 実行コマンド

```shell
# dailyのトレンドをSlackに通知
$ ruby notify_trend.rb

# dailyのトレンドでNEWのものだけをSlackに通知
$ ruby notify_trend.rb --new

# weeklyのトレンドをSlackに通知
$ ruby notify_trend.rb -t weekly

# weeklyのトレンドでNEWのものだけをSlackに通知
$ ruby notify_trend.rb -t weekly --new

# monthlyのトレンドをSlackに通知
$ ruby notify_trend.rb -t monthly

# monthlyのトレンドでNEWのものだけをSlackに通知
$ ruby notify_trend.rb -t monthly --new
```

## cronで定期実行

```
contrab -e
```

```
# 分 時 日 月 曜日 ユーザー名 コマンド
0  5  *  *  * root cd ~/projects/qiita_trend_slack_notifier && ruby notify_trend.rb --new
```
