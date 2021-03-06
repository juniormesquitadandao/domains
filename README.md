# Domains
Find available domains with Linux + Whois + Ruby
```rb
# script.rb

name_size = 5
tld = '.com'

chars = ['-'] + ('0'..'9').to_a + ('a'..'z').to_a
last = (chars.size ** name_size) - 1

current = 0
while current <= last
  sum = current

  mods = []
  name_size.times do
    mods << sum % chars.size
    sum = sum / chars.size
  end

  name = mods.reverse!.map do |mod|
    chars[mod]
  end.join

  whois = p `whois -H #{name}#{tld}`
  whois.encode! 'UTF-8', 'binary', invalid: :replace, undef: :replace, replace: ''
  if whois =~ /No match for/
    open('domains.txt', 'a') { |f|
      f.puts name
    }
  end

  current += 1
end
```
```rb
# control.rb

require 'daemons'
Daemons.run 'script.rb'
```
```sh
ruby control.rb start
```

