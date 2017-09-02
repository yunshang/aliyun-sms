# Aliyun::Sms 阿里云短信服务 Ruby Gem aliyun-sms

## Installation 安装

### Rails 应用安装方法

```

应用的根目录下运行:

```ruby
gem 'aliyun-sms', git: 'https://github.com/yunshang/aliyun-sms.git'
```

## Usage 使用

#### 第一步：

在 Rails 应用目录 config/initializers/ 下创建脚本文件 aliyun-sms.rb，在文件中加入以下内容：

config/initializers/aliyun-sms.rb

```ruby
Aliyun::Sms.configure do |config|
  config.access_key_secret = ENV['SMS_ACCESS_KEY_SECRET'] # 阿里云接入密钥，在阿里云控制台申请
  config.access_key_id = ENV['SMS_ACCESS_KEY_ID']          # 阿里云接入 ID, 在阿里云控制台申请
  config.action = 'SendSms'              # 默认设置，如果没有特殊需要，可以不改
  config.format = 'JSON'                       # 短信推送返回信息格式，可以填写 'JSON'或者'XML'
  config.region_id = 'cn-hangzhou'             # 默认设置，如果没有特殊需要，可以不改
  config.sign_name = ENV['SMS_SIGN_NAME']                 # 短信签名，在阿里云申请开通短信服务时申请获取
  config.signature_method = 'HMAC-SHA1'        # 加密算法，默认设置，不用修改
  config.signature_version = '1.0'             # 签名版本，默认设置，不用修改
  config.sms_version = '2017-05-25'            # 服务版本，默认设置，不用修改
  config.domain = 'dysmsapi.aliyuncs.com'
end
```

#### 第二步：

在 Rails 应用中调用短信发送代码：

```ruby
Aliyun::Sms.send(phone_number, template_code, param_string)
```    

参数说明：

1. phone_number: 接收短信的手机号，必须为字符型，例如 '1234567890'；
2. template\_code: 短信模版代码，必须为字符型，申请开通短信服务后，由阿里云提供，例如 'SMS_12345678'；
3. para_string: 请求字符串，向短信模版提供参数，必须为字符型的json格式，例如 '{"customer": "username"}'。

特别说明：

在程序中可以先用 HASH 组织 param\_string 内容，再使用 to_json 方法转换为 json 格式字符串，例如：

```ruby
phone_number = '1234567890'
template_code = 'SMS_12345678'
param_string = {'customer' => 'username'}.to_json
Aliyun::Sms.send(phone_number, template_code, param_string)
```    
