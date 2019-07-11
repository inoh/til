# Mock have_received

## 概要
プライベートな処理がインフラに依存していてテストがしずらいときにモックするテクニック。

## example

```ruby
# ユーザメーラー
class UserMailer
  def confirm
    user = User.new

    send(
      user.email('example.com')
    )
  end

  def send(email)
    ...
  end
end

# ユーザ
class User
  def initialize
    json = JSON.parse(
      Net::HTTP.get('https://api.github.com/users/inoh')
    )

    @login = json['login']
  end

  def email(domain)
    "ino_h@#{domain}"
  end
end
```

```ruby
RSpec.describe UserMailer do
  subject { described_class.new.confirm }

  let(:user) { spy('user') } 

  before do
    allow(User).to receive(:new).and_return(user)

    subject
  end

  it 'email が呼ばれること' do
    expect(user).to have_received(:email).with('example.com')
  end
end
```
