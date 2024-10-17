# Kiwi IRC

***A versatile web based messenger using IRC***


- 100% static files. Host with your favourite web server or a CDN
- Works out of the box with UnrealIRCd
- Single or multiple IRC network connections
- Multiple layouts for small areas or full page layouts
- Light and dark modes
- Desktop notifications
- Extremely versatile via a [single JSON config file at runtime](https://github.com/kiwiirc/kiwiirc/wiki/Configuration)
- Themable and rich [plugin support](https://github.com/kiwiirc/kiwiirc/wiki/Plugins) such as [file uploading](https://github.com/kiwiirc/plugin-fileuploader/) and [video calling](https://github.com/kiwiirc/plugin-conference)
- Team mode for workplaces

Connection modes:

- Directly to a websocket IRC server
- Connect via the [webircgateway](https://github.com/kiwiirc/webircgateway) websocket proxy for normal IRC servers
- Stay connected with [KiwiBNC](https://github.com/kiwiirc/kiwibnc)

## Installing Kiwi IRC
If you just wanted to embed an IRC client on your website, you usedto be able to generate a custom client hosted by kiwiirc.com using the simple client builder, https://kiwiirc.com/clientbuilder/
This has been giving an error "Gateway time-out" when I have tried it, however.

To install Kiwi IRC on your own server, pre-built and ready to use installers can be found at the downloads page, https://kiwiirc.com/downloads/
However if you download one of these "prebuilt" binaries you will need to create a config.conf by copying config.conf.example and editting it. Then you you also need
to edit www/static/config.json
in that file under "startupOptions" : { you will need to add lines like the following:
```
                "server": "your.default.com",
                "port": 6667,
                "tls": false,
```
or if the IRC server you have chosen as your default supports tls,
```
                "server": "your.default.com",
                "port": 6697,
                "tls": true,
```
Make sure you configure a certificate. Then you should be able to point web browser at https://your.host.com:port###/ and run the client.

## Setting up Kiwi IRC to use with UnrealIRCd
First you need to add the following lines to your unrealircd.conf file:
```
loadmodule "websocket";
loadmodule "webserver";
listen {
    ip *;
    port 8000;
    options {
        tls;
        websocket { type text; } // type must be either 'text' or 'binary'
    };
    tls-options {
        certificate "server.cert.pem";
        key "server.key.pem";
        options {
            no-client-certificate;
        };
    };
};
```
Then in your Kiwi IRC web folder, modify static/config.json and in that file under "startupOptions" : { you will need to add lines like the following:
```
        "server": "yourirc.server.com",
        "port": 8000,
        "tls": true,
        "direct": true,
```
You will only be able to connect to IRC servers (like your that you just configured) that support the websocket protocol.

## Building from source
#### Dependencies
Before you can build or start to develop on Kiwi IRC, make sure to have the following installed on your system:
* [Nodejs](https://nodejs.org/)
* [yarn](https://yarnpkg.com/)

#### Building for production

``` bash
# Install dependencies
$ yarn install

# Build Kiwi IRC into the dist/ folder
$ yarn run build
```

***Note:*** *Be sure to copy the files from the `dist/` folder to your webserver! This folder will be overwritten each time it is built.*

#### Development environment
Kiwi IRC is built using Vuejs, webpack and babel.

``` bash
# Install dependencies
yarn install

# Optionally link git pre-commit linting hooks
ln -s $PWD/scripts/pre-commit .git/hooks/

# A development web server with hot reloading at http://localhost:8080/
yarn run dev
```

***Note:*** *Do not use this development environment on your live website. It is slow, very large, and unsecure.*

#### Configuration

By default, the client will load the /static/config.json file on startup which
contains the runtime configuration. When running in the development environment this can be found at [static/config.json](static/config.json)


## Browser support

Kiwi IRC is tested on all modern browsers and IE11. Other browsers are not actively tested and may have trouble running Kiwi IRC.
* Chrome
* Chrome Mobile (Android)
* Firefox
* IE11
* Safari 9+

