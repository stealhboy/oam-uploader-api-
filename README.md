<h1 align="center">OAM Uploader API
  <a href="https://travis-ci.org/hotosm/oam-uploader-api">
    <img src="https://travis-ci.org/hotosm/oam-uploader-api.svg" alt="Build Status"></img>
  </a></h1>

<div align="center">
  <h3>
  <a href="https://docs.openaerialmap.org/ecosystem/getting-started/">Ecosystem</a>
  <span> | </span>
  <a href="http://hotosm.github.io/oam-uploader-api/">API Docs</a>
  <span> | </span>
  <a href="https://github.com/hotosm/oam-uploader">Uploader Interface</a>
  <span> | </span>
  <a href="https://github.com/hotosm/oam-uploader-admin">Token Manager</a>
  </h3>
</div>

The Uploader API powers the [Uploader Interface](https://github.com/hotosm/oam-uploader) by issuing authentication tokens and receiving imagery uploads. Before proceeding, we suggest you read the ecosystem docs.
(上傳器 API通過頒發身份驗證令牌和接收圖像上傳來為[上傳器界面](https://github.com/hotosm/oam-uploader)提供支持。在繼續之前，我們建議您閱讀 ecosystem docs。)

## Installation and Usage(安裝與使用)

The steps below will walk you through setting up your own instance of the oam-uploader-api.
(下面的步驟將引導您設置您自己的 oam-uploader-api 實例。)

### Install Project Dependencies(安裝依賴項目)

- [MongoDB](https://www.mongodb.org/)(安裝mongodb在ubuntu22.04(https://wiki.crowncloud.net/?How_to_Install_Latest_MongoDB_on_Ubuntu_22_04))
- [Node.js](https://nodejs.org/) v0.12

###我的mongo大問題
在執行安裝mongo安裝的步驟，到核實安裝是否成功的步驟時跑出
![image](https://user-images.githubusercontent.com/117743957/205804991-b54f57c1-15b1-4313-bbb2-d12539be32f9.png)
但是以上的地方都沒問題
![image](https://user-images.githubusercontent.com/117743957/205805104-37e0b8df-6ae7-40a7-a8f9-f19ecea7ed60.png)
![image](https://user-images.githubusercontent.com/117743957/205805157-f29107a7-c8d4-4982-abf6-fc5e46056742.png)
於是我上網找mongo not found 的原因，這位老歌也跟我有一樣麻煩
![image](https://user-images.githubusercontent.com/117743957/205805691-3a865773-72cd-4c7f-896d-9c809c03dba9.png)
下面有人給出解決方法可是
![image](https://user-images.githubusercontent.com/117743957/205806035-d84b8c74-08ee-4960-ab08-47246086c38f.png)
還是老樣子
![image](https://user-images.githubusercontent.com/117743957/205806153-47d1f165-2d31-4c38-9691-c2b718c5e5e1.png)




Using [homebrew](http://brew.sh/), you can install MongoDB and other dependencies:
(使用 [homebrew](http://brew.sh/)，您可以安裝 MongoDB 和其他依賴項目：)

    $ brew bundle

### Install Application Dependencies

If you use [`nvm`](https://github.com/creationix/nvm), activate the desired Node version:

```
nvm install
```

Install Node modules:

```
npm install
```

### Usage(用法)
You need to set environment variables before starting the API. We suggest you copy `local.sample.env` to `local.env` and modify it. Before starting the API you can run `source local.env` to export the environment variables to the shell.
(您需要在啟動 API 之前設置環境變量。我們建議您複製local.sample.env並local.env修改它。在啟動 API 之前，您可以運行source local.env以將環境變量導出到 shell。)

#### Environment Variables

- `NODE_ENV` - Node environment. When in production should be set to `production`, otherwise can be ignored.
- `PORT` - The port to listen on (Default to 4000).
- `HOST` - The hostname or ip address (Default to 0.0.0.0).
- `COOKIE_PASSWORD` - Password used for cookie encoding. Should be at least 32 characters long. IMPORTANT to change the default one for production.
- `AWS_ACCESS_KEY_ID` - AWS secret key id for reading `OIN_BUCKET`.
- `AWS_SECRET_ACCESS_KEY` - AWS secret access key for reading `OIN_BUCKET`.
- `AWS_REGION` - AWS region of `OIN_BUCKET` (Default to us-west-2).
- `SENDGRID_API_KEY` - Sendgrid API key, for sending notification emails.
- `SENDGRID_FROM` - Email address from which to send notification emails (Default to info@hotosm.org).
- `OIN_BUCKET` - The OIN bucket that will receive the uploads (Default to oam-uploader).
- `DBURI` - MongoDB connection url (Default to mongodb://localhost/oam-uploader)
- `DBURI_TEST` - MongoDB connection url for tests. Will be loaded when `NODE_ENV` is `test`. It's not needed for production. (Default to mongodb://localhost/oam-uploader-test)
- `ADMIN_USERNAME` - Username to access the [Token Management](https://github.com/hotosm/oam-uploader-admin) panel.
- `ADMIN_PASSWORD` - Password to access the [Token Management](https://github.com/hotosm/oam-uploader-admin) panel.
- `GDRIVE_KEY` - Google Api key. Needed to use the upload from google drive functionality.
- `GDAL_TRANSLATE_BIN` - Full path to the gdal bin (Default to /usr/bin/gdal_translate)
- `MAX_WORKERS` - Max number of workers used to process the uploads (Default to 1)
- `NEW_RELIC_LICENSE_KEY` - New relic license key.
- `TILER_BASE_URL` - Base URL for dynamic TMS/WMTS endpoints. Defaults to `http://tiles.openaerialmap.org`.

For a quick local setup for development the following variables can be omitted: `SENDGRID_API_KEY`, `SENDGRID_FROM`, `GDRIVE_KEY`, `NEW_RELIC_LICENSE_KEY`. Be aware that although the system will work some functionalities will not be available and errors may be triggered.

#### Starting the database

```
mongod
```

![image](https://user-images.githubusercontent.com/117743957/204726066-4c0e1b31-73aa-4f03-a5cf-f495c8d6e3e5.png)
![image](https://user-images.githubusercontent.com/117743957/204726242-5c41cef9-fe5d-49fa-9eda-2c3dd593f6ec.png)
![image](https://user-images.githubusercontent.com/117743957/204726293-8374597d-c9e6-4fc3-b367-84d15b76c429.png)


#### Starting the API:

```
npm start
```
![image](https://user-images.githubusercontent.com/117743957/204726396-dc5a7e93-0d4b-4d3a-92d1-1c63ca82f245.png)
![image](https://user-images.githubusercontent.com/117743957/204726452-1b8b0422-018a-4e47-ad4f-13117c0b8bff.png)


The API exposes endpoints used to access information form the system via a RESTful interface.

### Tokens
The uploader endpoints are protected via a token authentication system.
Tokens are issued using the [oam-uploader-admin](https://github.com/hotosm/oam-uploader-admin), a friendly user interface for token management. Follow the [instructions](https://github.com/hotosm/oam-uploader-admin) to set it up. To understand better how all the components interact refer to the [Ecosystem Docs](https://docs.openaerialmap.org/ecosystem/getting-started/).

#### Populate the database

To get the API running quickly, you can skip running the admin interface and populate the database with a test token. 

```
mongo
use oam-uploader
db.tokens.insert(
  {
    "name" : "admin",
    "expiration" : false,
    "status" : "active",
    "token" : "MYTESTTOKEN",
    "created" : ISODate("2016-12-15T17:28:19.823Z"),
    "updated" : null
  }
)
```

### Documentation
The documentation for the different endpoints can be generated by running:
```
npm run docs
```
This will compile the documentation and place it inside `docs`. The docs site can then be run by any web server.

The documentation is also automatically built and deployed by [Travis CI](https://travis-ci.org/) whenever a Pull Request is merged to the production branch (in this case `master`). The deployment is done to `gh-pages`.

### Install via Docker

Alternatively, if you've got a mongo instance running elsewhere, install and
run on a fresh instance using docker as follows:

[Install Docker](https://docs.docker.com/installation/)

One-time setup:

```sh
# clone the repo
git clone https://github.com/hotosm/oam-uploader-api

# build the docker image
cd oam-uploader-api
docker build -t oam-uploader-api .

# set up environment vars:
cp local.sample.env local.env
nano local.env
```

To start up the API server after pulling from the repo:

```sh
# install node dependencies
.build_scripts/docker/run.sh /install.sh
# start server
.build_scripts/docker/run.sh /start.sh
```

# Deployment

To automate some of the above on a remote, you can use
[visionmedia/deploy](https://github.com/visionmedia/deploy) and upstart for
deployment and process management.

First, add a new section to
https://github.com/hotosm/oam-uploader-api/blob/develop/.build_scripts/deploy.conf.
(If you don't want to commit it to the repo, you can just make your own copy of the
config file wherever you want.) Make sure you have ssh creds to the server from
wherever you're running `deploy`. Then do the following, with `ENV` replaced with
whatever you called the section you added to `deploy.conf`.

```sh
deploy -c .build_scripts/deploy.conf ENV setup
```

Now ssh into the server with `deploy -c .build_scripts/deploy.conf ENV console`,
and set up the upstart script and start up the server

```sh
sudo cp .build_scripts/upstart.conf /etc/init/oam-uploader-api.conf
sudo nano /etc/init/oam-uplodaer-api.conf
sudo start oam-uploader-api
exit
```

Now you can use `deploy -c .build_scripts/deploy.conf ENV` at any time to
deploy your local branch.

## License
Oam Uploader Api is licensed under **BSD 3-Clause License**, see the [LICENSE](LICENSE) file for more details.


