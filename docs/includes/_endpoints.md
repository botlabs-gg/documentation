# Endpoints

Weeb.sh provides APIs on multiple base endpoints(environments), generally users are unlocked for the staging and production endpoint of an api, but in special cases users may only be unlocked for a single environment/endpoint.

## API endpoints

| Environment | URL                             | Purpose                 |
|-------------|---------------------------------|-------------------------|
| development | http://localhost:API_PORT       | local development       |
| staging     | https://staging.weeb.sh/images/ | testing of new features | 
| production  | https://api.weeb.sh/images/     | production usage        |

## CDN endpoints
To serve our assets (images, data, etc.) we also operate the following cdn endpoints:

| Environment | URL                             | Purpose                                   |
|-------------|---------------------------------|-------------------------------------------|
| all         | https://cdn.weeb.sh             | serve assets created by services and users|
