# Project
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-8-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

## Video Overviews

High quality version on sharepoint [HERE](https://mnscorp-my.sharepoint.com/:v:/g/personal/christopher_mitchelmore_mnscorp_net/EZcJD2TF955Nk_EWVpsg9o0BvtRHrGGYJoq1CVaRxIFIWA?e=0ePYdU)

### GraphQL Code

![GraphQL Overview](https://user-images.githubusercontent.com/1902403/121559493-84206300-ca0e-11eb-92ca-0fbcb3e973e4.mp4)

### Infrastructure as code and pipelines

![Infrastructure as code and pipelines overview](https://user-images.githubusercontent.com/1902403/121559618-a74b1280-ca0e-11eb-8a7d-0d0994f77e16.mp4)

## Useful links

[Service Design and Support Model](docs/service-design.md)

## Requirements

- [Node.js 14.17.x](https://nodejs.org/en/download/)
- [Yarn](https://yarnpkg.com) `npm install --global yarn`
- [Azure Functions Core Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=macos%2Ccsharp%2Cbash#install-the-azure-functions-core-tools)

## Used packages

- [Apollo](https://www.apollographql.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [ESLint](https://eslint.org/)
- [GraphQL Code Generator](https://graphql-code-generator.com/docs/getting-started/)
- [stmux](https://github.com/rse/stmux)

## Installation

- Install Azure Functions Core Tools
  - `brew tap azure/functions`
  - `brew install azure-functions-core-tools@3`
- Clone the repo
  - `git clone git@github.com:DigitalInnovation/yggdrasil.git`
- `yarn install`

## Development

### Running the toolchain

To run locally using apollo express use

```bash
yarn dev
```

🔥 To quit, press `ctrl-a k`
🔄 To restart a process, select window and press `ctrl-a r`

### Linting using ESLint and prettier and fixing linting errors

To see linting errors on command line do the following: 

```bash
yarn lint
```

Some ESLint errors are fixable programmatically, to do so please use:

```bash
yarn lint-fix
```

NB: `lint-fix` is run before each commit.

## Pull Requests

When you create a pull request it is validated and tested first then we spin up an ephemeral function that runs you PR code so that you can do any e2e or integrated testing.

Changes to infrastructure logic or configuration are also validated in a PR.

See contributing.md for PR guidelines.

## Environment Variables

Add a ```.env``` file with the following:

```bash
API_BASE_URL="https://api.marksandspencer.com/"
```

Environment variables are configured as part of the infrastructure as code and require an infrastructure deployment to be applied. See ci/infra/terraform/vars and look for layer 3 in the environment you need to make the change on.

## Deployment

We use github actions to run our pipelines for both applicaiton and infrastructure changes. The function is deployed as code only and Azure function takes care of the rest. Deployment is continuous so merging a change to main will deploy to dev, test, preprod(slot) in sequence and then perform a slot swap to production for 0 downtime.

If you want to deploy your an environment from local see [Deploy your own environment from local](ci/infra/terraform#deploy-your-own-env-from-local)

## Run Check

To check that things are up and running you can open the GraphQL explorer interface on from `http://localhost:4000/` and paste the following query to check you're running.

```graphql
  query  {
  product(id: "P60458745") {
    id
    skus{
      skuId
      quantityAdvised
    }
    reviews {
      averageRating
    }
  }
}
```

N.B. The product id may no longer work but you can grab a new one from the URL of a product page and exclude the pl part. e.g.

`https://www.marksandspencer.com/corduroy-slim-fit-flare-trousers/p/clp60458745?color=KHAKI`
Would give you ```p60458745``` as a product ID for the query.

### Authentication

Please note!

**You will need to provide API keys to authenticate against the APIs**

Currently the project operates on a delegate authentication model. The client provides the API key in the form of an auth header e.g.

{ "Authorization": "MSAuth apikey=yourAPIkeyHERE"}

In GraphQL explorer you can add this using the "HTTP HEADERS" section in the bottom left. If you are making direct requests some other way, you will need to add the auth header there too.

When you make a GraphQL query your request will be resolved against some data source (Exclusively HttpDataSources at the moment). Your API key will need to be valid against all dependent APIs.

Discussions around how we will implement auth in the future are here https://github.com/DigitalInnovation/yggdrasil/discussions/549

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->


<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
