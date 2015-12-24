# firewall-relay

Start a service within a network, point it at the public server, and have access to all HTTP goodness available internally.

## Setup

### 1. Deploy a relay server

Here is a lovely one-click installer for you! :heart:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

> **NOTE** The following is not yet implemented.

For security, run `heroku config:set SHARED_SECRET=ABC123` (with a secret you create of course). This will require that secret to be in the headers of each request. The `firewall-relay-client` can be used to act as a lightweight proxy that stores the secret and sets the header for you. It will also need to be set in your `.firewall-relay.json` file.

### 2. Configure & start a producer

On a computer you plan to keep within the network you can't access externally, run `npm install -g firewall-relay` to install. You can now start the producer with `firewall-relay relay=RELAY_URL`.

> **NOTE** The following is not yet implemented.

To whitelist the domains the producer should share, you can create a file in your home directory called `.firewall-relay.json`.

```json
{
  "allowedDomains": [
    "api.example1.org",
    "api.example2.org"
  ],
  "disallowedDomains": [
    "db.example1.org"
  ],
  "defaultRelay": "http://my-relay.herokuapp.com/",
  "secret": "ABC123"
}
```

* `allowedDomains` - whitelist method, only allow access to these and no others
* `disallowedDomains` - blacklist method, disallow access to these domains, but allow access to all others
* `defaultRelay` - saves you from having to pass it to the `firewall-relay` command
* `secret` - must be set if your relay has a `SHARED_SECRET` environment variable set

### 3. Start a client (optional)

> **NOTE** The following is not yet implemented.

If you're using a shared secret, it's suggested to use a local client server on your external machine rather than connect directly to the relay.

Start a client by running `firewall-relay-client relay=RELAY_URL secret=ABC123`.

The `firewall-relay-client` will also use the `.firewall-relay.json` file, but will ignore things like `allowedDomains` and `disallowedDomains`.

## Important things to consider!

* Is doing this against company policy?
* Does your company require traffic be encrypted via SSL?
* Should this be locked down so only select people can access it? (If so, set up a shared secret)
