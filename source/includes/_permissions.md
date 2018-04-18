# Permissions

>If a user is missing permissions for an endpoint, they will be seeing a message similar to the following:

```json
{
    "status": 403,
    "message": "missing scope korra-production:generate_simple"
}
```

Weeb.sh apis use an internal permission model to make sure that users are only allowed to use the endpoints they are whitelisted for.
Those permissions are stored using permission keys, which consist of the api-codename, the environment and optionally a key to specify the endpoint/endpoints that may be used:
`apicodename-environment:key`

Here are some examples:

`toph-production:image_data` -> grants a user `image_data` on `toph` (images) in the production environment

`toph-staging` -> grants **all** permissions on `toph` (images) in the staging environment

