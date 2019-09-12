# logger

## 概要

ログの出力先を変更や出力形式を変更する場合は、 Rogger の設定を変更する。  

## example

Rails の Logger を差し替えて出力先を標準出力にする場合

```ruby
config.logger = ActiveSupport::Logger.new(STDOUT)
```

Rails は ruby 標準の Rogger を ActiveSupport で拡張して使用する。  

- Ruby の Logger  
https://github.com/ruby/logger  
- ActiveSupport の Logger  
https://github.com/rails/rails/blob/master/activesupport/lib/active_support/logger.rb  

ログの出力形式を変更してファイルに出力する場合

```ruby
#
# Logger::Formatter を ActiveSupport で拡張したものを更に拡張し、
#
class CustomFormatter < ActiveSupport::Logger::SimpleFormatter
  #
  # エラーをカスタマイズして返す
  #
  # @param [String] severity エラーレベル
  # @param [Time] timestamp タイムスタンプ
  # @param [String] progname プログラム名
  # @param [String] msg メッセージ
  #
  # @return [String] エラーメッセージ
  #
  def call(severity, timestamp, progname, msg)
    {
      message: message,
      timestamp: timestamp.strftime('%Y-%m-%dT%H:%M:%S'),
      level: severity
    }.to_json
  end
end

logger = ActiveSupport::Logger.new(
  Rails.root.join('log', 'custom.log')
)
logger.formatter = CustomFormatter.new
config.logger = logger
```
