## HA Proxy Docker Compose

This repo has HA Proxy configuration with `docker-compose`.


## Features

| Scenarios                   | Status  |
|-----------------------------|---------|
| Blacklist                   | ✅      |
| Rate Limit Global           | ✅      |
| Rate Limit on URLs          | ✅      |
| Rate Limit on Specific URLs | ✅      |


## HA Proxy Setup

1. Clone the repo

```bash
git@github.com:shardulsrivastava/haproxy-docker-compose.git
```

2. Start `haproxy` and `nginx` containers:

```bash
docker-compose up -d
```

3. Check the logs of `haproxy` container

```bash
docker-compose logs -f haproxy
```

4. Verify if `haproxy` is up and able to proxy requests to nginx container

```bash
curl -v http://localhost
```

## Change HA Proxy configuration

To test and play around with different configurations, update [haproxy.cfg](haproxy/haproxy.cfg) file. After adding changes, to reload `haproxy` configuration run:

```bash
docker-compose kill -s HUP haproxy
```

## Setup Ngrok to Test IP whitelisting

1. Install [ngrok](https://ngrok.com/download) from here

```bash
brew install ngrok/ngrok/ngrok
```

2. Once installed, sign up for `Ngrok` (it's free of cost) [here](https://dashboard.ngrok.com/).


3. Configure Ngrok token

```bash
ngrok config add-authtoken <token>
```

4. Start `ngrok`:

```bash
ngrok http 80
```

Copy the **Ngrok URL** from here and access it in the browser.

## Test IP blacklisting

1. Get your IPv6

```bash
curl ifconfig.io
```

2. Get your public IP

```bash
curl ifconfig.me
```

3. Add both IPs to the file [blacklist.acl](haproxy/blacklist.acl)

```
2a02:8109:a180:2e20:2c94:75c4:6ac6:8e7c
95.91.241.25
```

4. Restart the `haproxy` config:

```bash
docker-compose kill -s HUP haproxy
```

5. Access the application from browser:

```
curl <NGROK URL>
```

It should return `403 Forbidden` now.


