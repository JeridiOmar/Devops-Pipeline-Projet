## DevOps pipeline project
In this project I used 
- github actions for the ci workflow
- azure container registry
- azure app service for continuous delivery
 - Deployment website link
 `https://sahti.azurewebsites.net/`
## Pipeline explained
- building project and running tests
```yaml
      - run: npm install
      - run: npm install -g dotenv-cli
      - run: npm run test
```
-building docker image and push to azure CR

```yaml
      - uses: azure/docker-login@v1
        with:
          login-server: sahti.azurecr.io
          username: ${{ secrets.REGISTRY_USERNAME }}
          password: ${{ secrets.REGISTRY_PASSWORD }}

      - run: |
          docker build . -t sahti.azurecr.io/shati:${{ github.sha }}
          docker image ls
          docker push sahti.azurecr.io/shati:${{ github.sha }}
```
- deploy the new version to azure webapp


```yaml
           - uses: azure/webapps-deploy@v2
             with:
               app-name: 'sahti'
               publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
               images: 'sahti.azurecr.io/sahti:${{ github.sha }}'
```

- finally we can see the response updated after the latest push
<img src="./readme_assets/deploy.PNG">

- azure app service config
<img src="./readme_assets/config.PNG">
- azure app service scale out and deployment slot config
<img src="./readme_assets/config2.PNG">

***azure web app name is sahti because the initial plan was about mixing the test & devops labs
but some problem occurred so I migrate the pipeline to this dummy project which does not affect it functionality***

## Actions Screenshots
<img src="./readme_assets/ci1.PNG">
<img src="./readme_assets/ci%202.PNG">


## Installation

```bash
$ npm install
```

## Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```


## License

Nest is [MIT licensed](LICENSE).
