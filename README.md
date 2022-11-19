This repository contains basic config files and settings for deploying a mastodon instance via docker and caddy.

- docker is directory somewhere on your server
- then take a look into the configs
	- [docker/docker-compose.yml](docker/docker-compose.yml)
	- [docker/caddy/Caddyfile](docker/caddy/Caddyfile)
	- [docker/mastodon/mastodon/.env.production](docker/mastodon/mastodon/.env.production)
	
## To initialize the setup with secrets etc.
```bash
cd docker
SECRET_KEY_BASE=$(docker-compose run --rm mastodon-web bundle exec rake secret)
sed -i -e "s/SECRET_KEY_BASE=/&${SECRET_KEY_BASE}/" ./mastodon/mastodon/.env.production
OTP_SECRET=$(docker-compose run --rm web bundle exec rake secret)
$ sed -i -e "s/OTP_SECRET=/&${OTP_SECRET}/" ./mastodon/mastodon/.env.production
docker-compose run --rm mastodon-web bundle exec rake mastodon:webpush:generate_vapid_key
sudo nano ./mastodon/mastodon/.env.production
# do your changes
sudo chown -R 991:991 mastodon/mastodon/public/system
```

## start everything
```bash
cd docker
docker-compose up -d
```


### stop everything
```bash
cd docker
docker-compose down

