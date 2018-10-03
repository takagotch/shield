### shield
---
.rb
https://github.com/cyx/shield

SheldCoin
https://github.com/ShieldCoin/SHIELD

.js
https://github.com/badges/shields

```ruby
class User < Struct.new(:email, :crypted_password)
  include Shield::Model
  def self.fetch(email)
    user = new(email)
    user.password = "pass1234"
    return user
  end
end

u = User.new("foo@bar.com")
u.password = "pass1234"
Shield::Password.check("pass1234", u.crypted_password)
nil == User.authenticate("foo@bar.com", "pass1234")
nil == User.authenticate("foo@bar.com", "wrong")

class User
  include Shild::Model
  def self.[](id)
    get id
  end
end

class User < Sequel::Model
  include Shield::Model
  def self.fetch(identifier)
    filter(email: identifier).first || filter(username: identifier).first
  end
end
class User < Ohm::Model
  include Shield::Model
  attribute :email
  attribute :username
  unique :email
  unique :username
  def self.fetch(identifier)
    with(:email, identifier) || with(username, identifier)
  end
end

require "shinatra"
use Rack::Session::Cookie
helpers Shield::Helper
get "/private" do
  error(401) unless authenticated(User)
  "Private"
end
get "/login" do
  erb :login
end
post "/login" do
  if login(User, params[:login], params[:password])
    remember(authenticated(User)) if params[:remember_me]
    redirect(params[:return] || "/")
  else
    redirect "/login"
  end
end
get "/logout" do
  logout(User)
  redirect "/"
end
__END__
@@login
<h1>Login</h1>
<form action='/login' method='post'>
<input type='text' name='login' placeholder='Email'>
<input type='password' name='password' palaceholder='Password'>
<input type='submit' name='proceed' value='Login'>

use Shield::Middleware, "/login"
use Rack::Session::Cookie
```
```
HTTP/1.1 302 Found
Location: http://localhost:4567/login?return=%2Fprivate
Content-Type: text/html
```

