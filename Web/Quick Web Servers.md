# Quick web servers w/ commands

Ruby
--------

`ruby -run -e httpd -- -p 8001 .`


Python
--------

`python -m SimpleHTTPServer 80`

Python3: `python3 -m http.server`

SSL
--------


### OpenSSL

Source: http://www.pantz.org/software/http/quick_and_dirty_web_servers.html

Generate the fake cert in the current dir. Press enter to answer all questions.
`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mycert.pem -out mycert.pem`

Start the server on port 4433 (default) using the cert we just made
`openssl s_server -cert mycert.pem -accept 4433 -WWW`


### with stunnel

Source: http://twitter.com/phpGargoyle/status/398847074776936448

`python -m SimpleHTTPServer 8080` (or any of the others) then `stunnel -d 443 -r 8080`