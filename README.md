This repository contains basic config files and settings for deploying a mastodon instance via docker and caddy.

- docker is directory somewhere on your server
- then take a look into the configs
	- [docker/docker-compose.yml](docker/docker-compose.yml)
	- [docker/caddy/Caddyfile](docker/caddy/Caddyfile)
	- [docker/mastodon/mastodon/.env.production](docker/mastodon/mastodon/.env.production)
	
## To initialize the setup with secrets etc.
`bash
SECRET_KEY_BASE=$(docker-compose run --rm mastodon-web bundle exec rake secret)
sed -i -e "s/SECRET_KEY_BASE=/&${SECRET_KEY_BASE}/" .env.production
OTP_SECRET=$(docker-compose run --rm web bundle exec rake secret)
$ sed -i -e "s/OTP_SECRET=/&${OTP_SECRET}/" .env.production
docker-compose run --rm mastodon-web bundle exec rake mastodon:webpush:generate_vapid_key
sudo nano ./.env.production
# do your changes
sudo chown -R 991:991 public/system
`