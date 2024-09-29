# Satisfactory server with a map

This project defines the necessary `docker-compose.yml` to start a Satisfactory server in a custom domain `mydomain.com` and visualize in [Satisfactory Calculator](https://satisfactory-calculator.com) the map of the Satisfactory server by going to `satisfactory.mydomain.com`.

This project uses the image from [wolveix/satisfactory-server](https://github.com/wolveix/satisfactory-server) to run the satisfactory server.

## Usage

To use this project you can follow this steps:

1. Clone the repository:

```
git clone https://github.com/jmigual/satisfactory-server-with-map
```
2. Enter the repository:
```
cd satisfactory-server-with-map
```
3. Copy `example.env` to `.env` and edit the `USER_DOMAIN` and `USER_EMAIL` variables with your own domain name and your e-mail (used for let's encrypt TLS certificate) respectively.
4. Pull and build the images:
```
docker-compose build --pull && docker-compose pull
```
5. Start the server
```
docker-compose up -d
```

## How does it work

Satisfactory calculator can load a custom map if you access it with the url `https://satisfactory-calculator?url=<my_url>`. Where your browser needs to be able to access `<my_url>` (i.e., download the file) and the CORS policy needs to be set properly.

What we do is we create an nginx server that provides two urls:
- `mydomain.com/saves/*`: This one serves any savefile of the satisfactory server such that it can be downloaded.
- `mydomain.com`: This one redirects to `https://satisfactory-calculator?url=https://mydomain.com/saves/latest.sav`.

The save `latest.sav` is a symlink created by the `satisfactory-latest` docker image which runs a cronjob on the saves folder and updates the the symlink to point to the last modified save. See `build-scripts/satisfactory-latest` for the details of how the symlink is created. 

