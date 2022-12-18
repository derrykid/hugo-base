---
title: "Curl cheat sheet, for web developement"
date: 2022-12-18T20:42:21Z
tags: ["curl", "web", "command", "HTTP"]
categories: ["technology and development"]
---

## HTTP Request

### Specify Method by `-X`

Form:
```bash
curl -X <METHOD> <URI>
```

By default, it uses `GET` method
```bash
curl https://www.google.com/

# same as 
curl -X GET https://www.google.com/
```

Use Post simply by:
```bash
curl -X POST https://www.example.com/
```

### GET

Get entire document of the website in terminal
```bash
curl https://www.google.com/
```

### POST

```bash
curl -X POST https://example.com/
```

More complex, pass data, `-d` and header `-H` to server. Below shows POST with json body
```bash
curl -d '{"option": "value", "something": "anothervalue"}' \
-H "Content-Type: application/json" \ 
-X POST https://example.com/
```


## HTTP Headers

### Inspect all details of header by `--verbose`
```bash
curl -v https://www.google.com/

# same as 
curl -verbose https://www.google.com/
```

### Only see the header by `-I`

```bash
curl -I https://www.google.com/
```

### Get the body along with response header with `-i`

It's similar to `--verbose` but contains less.

Response header is usually omitted. Use `-i` to include it
```bash
# full name
curl -include https://www.google.com/
curl -i https://www.google.com/
```

### Set referer

> An HTTP request may include a 'referer' field (yes it is misspelled), which can be used to tell from which URL the client got to this particular resource. 

```bash
curl --referer http://I.come.here http://www.example.com
```

### Set User-agnet

```bash
curl --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" [URL]
```

## Store the response to a file, like `wget`

With its origin file name
```
curl -O https://derry.club/index.html
```

Give a new name
```
curl -o output.html https://derry.club/
```

