**Create A New App Extension**  
`npm run deploy:reset` - select extension - `Yes, create is as a new app` - enter an App name  
go to `partners` dashboard - click on `Apps` navigation - `All apps` - `App name` from Apps list

copy `Client ID` and `Client secret` - paste it in HEROKU Settings `Config Vars` - `SHOPIFY_API_KEY` and `SHOPIFY_API_SECRET`  
in `heroku.yaml` update `SHOPIFY_API_KEY` with same value as it is in `Config Vars`  
create `heroku.yml` if not exists
```
build:
docker:
    web: Dockerfile
config:
    SHOPIFY_API_KEY: 
```  
go to `App settings` - update `App URL` and `Allowed redirection URL` with heroku `urls` - `/api/auth` and `/api/auth/callback`  
go to `Extensions` and create `major version` for every extension - `publish` them  
got to `Distribution` - `Choose single merchant installation` - enter valid shop url

**View existing Apps**  
can be found on `https://partners.shopify.com/` - Apps - All apps

**View existing App URLs**  
click on `Apps` navigation - `All apps` - `App name` from Apps list - `App setup` - URLs  
`development app urls` are automatically updated when command `npm run dev` is executed  
`production app urls` needs to be manually entered - check heroku for base url!  
pay attention to `Allowed redirection URLs` - `/api/auth` and `/api/auth/callback`

**View existing App Extensions**  
`app extensions` are visible on shopify partner page - click on `App name` from Apps list - `Extensions`  
to `activate/deactivate` extension - click on extension `title` from list - locate `Development store preview` - click `ENABLE/DISABLE`  
to `publish extension` - create new version - click on `publish` link in `Version history` list

**Start development**  
change directory to `checkout-extension`  
`npm run deploy-reset` - select extension - `No, connect it to an existing app` - select existing app  
`npm run dev` - start local dev, `URLs` in app setup will be updated with new ones

**Deploying production APP - First attempt**  
run `npm run deploy-reset` - select existing production app  
run `npm run deploy` to deploy changes to Shopify  
go to the `heroku.com` - select/create extensions server - update Settings `Config Vars` - `SHOPIFY_API_KEY` and `SHOPIFY_API_SECRET`  
config vars can be found in `.env` file or in `partner dashboard` clicking on selected App name  
in `heroku.yaml` update `SHOPIFY_API_KEY` with same value as it is in `Config Vars`
run `git push heroku master` to deploy changes on Heroku - server will be restarted with new changes applied
go to `shopify partners` and create `major version` for a extensions

**Create Heroku App and Deploy Production -- Fast Approach**
- create Shopify app: run `npm run deploy-reset` - select extension - `Yes, create is as a new app` - enter an App name  
- go to Shopify Partners dashboard and find your App - copy: `Client ID` and `Client secret` 
    - let it open in a tab!
- execute `heroku create -a <heroku-app-name> -s container`  
- open new tab and go to heroku dashboard, find your App - go to Settings -> Reveal Config Vars and add following variables:  
 HOST: ${Heroku HOST URL}  
 SHOPIFY_API_KEY: ${Client ID}  
 SHOPIFY_API_SECRET: ${Client Secret}  
 SCOPES:  ${App scopes separated with comma}
- come back to the Shopify App and update App setup URLs with the following urls:
 App URL: ${Heroku HOST URL}  
 Redirection URLs: ${Heroku HOST URL}/api/auth and ${Heroku HOST URL}/api/auth/callback  
- go to Distribution tab, click on `Choose single-merchant install link` and generate install link, do not visit it jet!
- push changes to heroku: `git push heroku master`
- visit generated installation link once git push is done
- fin

**Production Extension Update**  
after every production extension update - create new version and publish it

**Create App**
https://shopify.dev/docs/apps/getting-started/create
**Scaffolding**  
https://shopify.dev/docs/api/checkout-ui-extensions#scaffolding-extension  
**Generate Extension**  
https://shopify.dev/docs/apps/tools/cli/commands#generate-extension  
**Documentation**  
https://shopify.dev/docs/apps/checkout  
**Create Heroku Server**  
https://shopify.dev/docs/apps/deployment/web#set-up-hosting-with-heroku
