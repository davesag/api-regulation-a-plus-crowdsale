# api-regulation-a-plus-crowdsale

[![Greenkeeper badge](https://badges.greenkeeper.io/C4Coin/api-regulation-a-plus-crowdsale.svg)](https://greenkeeper.io/)

The API that interfaces between C4Coin's Regulation A+ Crowdsale website and smart contracts

* `develop` — [![CircleCI](https://circleci.com/gh/C4Coin/api-regulation-a-plus-crowdsale/tree/develop.svg?style=svg)](https://circleci.com/gh/C4Coin/api-regulation-a-plus-crowdsale/tree/develop) [![codecov](https://codecov.io/gh/C4Coin/api-regulation-a-plus-crowdsale/branch/develop/graph/badge.svg)](https://codecov.io/gh/C4Coin/api-regulation-a-plus-crowdsale)
* `master` — [![CircleCI](https://circleci.com/gh/C4Coin/api-regulation-a-plus-crowdsale/tree/master.svg?style=svg)](https://circleci.com/gh/C4Coin/api-regulation-a-plus-crowdsale/tree/master) [![codecov](https://codecov.io/gh/C4Coin/api-regulation-a-plus-crowdsale/branch/master/graph/badge.svg)](https://codecov.io/gh/C4Coin/api-regulation-a-plus-crowdsale)

## Functional Requirements

The API Server supports the C4Coin Regulation A+ Crowdsale website.

* Investor Registration
* Password Reset
* Two Factor Auth support (RSA token or similar)
* Investor login

  - Investor KYC data upload/update (how to verify the person's details?  send a postcard?)
  - Investor Ethereum Address Registration
  - Investor Ethereum Address Update
  - Buy Tokens
  - Logout

* Admin login

  - Verify Potential Investor's Ethereum Address / KYC data
  - Unverify Investor Address
  - set USDConversionRate
  - finaliseCrowdsale

* Get Public Crowdsale Data

  - startDate (UTC)
  - endDate (UTC)
  - tokens sold (integer)
  - isGoalReached (boolean)
  - isCapReached (boolean)
  - investor count
  - amountRaised (USD)

### Security

* The `login` feature will return a time-limited JOSE token containing encrypted user credentials.
* all subsequent activities requiring user authentication will extract this token from the `authorization` header.
* if the token expires a new one can be generated to replace it.

### API Routes _incomplete_

#### `GET /ping`

Returns a heartbeat response.

    200 Okay

    {
      "response": "okay",
      "uptime": secondsSinceServerLaunch
    }

#### `GET /`

Returns a list of API versions.

    200 Okay

    [
      {
        version: 1,
        path: '/api/v1'
      }
    ]

#### `POST /api/v1/login` (_not implemented_)

Logs a user in via simple credentials (can be enhanced later to support 2fa)

Body params

    {
      username: 'string',
      password: 'string'
    }

Returns

    200 Okay

    {
      token: 'some-jwt-that-must-go-in-the-header-to-remain-logged-in'
    }

Error Response

    401 Unauthorised

#### `POST /api/v1/logout` (_not implemented_)

Logs a user out

Returns

    200 Okay

## Development

### Prerequisites

* [NodeJS](htps://nodejs.org), version 9+ (I use [`nvm`](https://github.com/creationix/nvm) to manage Node versions — `brew install nvm`.)
* [Docker](https://www.docker.com) (Use [Docker for Mac](https://docs.docker.com/docker-for-mac/), not the homebrew version)
* [Access to the C4Coin Jira](https://c4coin.atlassian.net)

### Initialisation

    npm install

### To Start the API server while working on API clients.

    docker-compose up -d

Runs the database and server within docker, exposing the API on port `3001`.

### To Start the server to work on the server itself

    npm install

Run `docker-compose up -d db` to only start Postgres,

Then run `npm start` to start the api server on port `3000`

### Seed some data

With the database running, run

    I_KNOW_WHAT_I_AM_DOING=true npm run seed

### Test it

run `docker-compose up db -d` to only start Postgres, then:

* `npm test` — runs the unit tests (quick)
* `npm run test:db` — runs the database tests (not so quick)
* `npm run test:server` — runs the API endpoint tests (not so quick)
* `npm run test:all` — runs all the tests (slowest of all)

### Lint it

    npm run lint

## Deployment

The site will be deployed automatically to [heroku](https://heroku.com) once CircleCI has cleared a merge to either `develop` (staging server) or `master` (production).

## Contributing

Please see the [contributing notes](CONTRIBUTING.md).
