# Domains
Find available domains with Linux + Whois + Ruby

```rb
# control.rb

require 'daemons'
Daemons.run 'script.rb'
```

```rb
# script.rb

name_size = 5
tld = '.com'

chars = ('0'..'9').to_a + ('a'..'z').to_a
first = chars.size ** (name_size - 1)
last = (chars.size ** name_size) - 1

current = first
while current <= last
  sum = current

  mods = []
  5.times do
    mods << sum % chars.size
    sum = sum / chars.size
  end

  name = mods.reverse!.map do |mod|
    chars[mod]
  end.join

  whois = p `whois -H #{name}#{tld}`
  if whois =~ /No match for/
    open('domains.txt', 'a') { |f|
      f.puts name
    }
  end

  current += 1
end
```

```sh
ruby control.rb start
```

