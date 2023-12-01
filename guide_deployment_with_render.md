# Guide: Deployment with Render.com

## Sign up for a free Render account

- Visit https://render.com/ 
- Sign up using your GitHub account
- (Note: You can use the service for free without a credit card with [limitations](https://render.com/docs/free))


## Deploy a PostgreSQL database

- Create a new PostgreSQL database: https://dashboard.render.com/new/database
  - **Name**: `<your_app_name>`
  - **Database**: `<your_app_name>`
  - **User**: `<your_app_name>`
- (Wait a few minutes for the database to be created)
- Keep the page open, you'll need to copy the **Internal Database URL** for the next step

## Deploy a Rails backend API

- In your terminal, navigate to your Rails app
  - Run `rails test` to make sure your app is working locally
  - Run `rails server` to make sure your server is running locally
  - Run `bundle lock --add-platform x86_64-linux` to add linux as a platform in the production environment
  - Use git to add, commit, and push your code
- On render.com, create a new web service: https://dashboard.render.com/web/new
  - **Connect a repository** _(choose one of your public GitHub repositories to deploy)_
  - **Name**: `<your_app_name>`
  - **Build command**: `bundle install; bundle exec rake db:migrate; bundle exec rake db:seed;`
  - Click the "Advanced" button, then click the "Add Environment Variable" button
    - **DATABASE_URL**: _(paste the **Internal Database URL** from the previous step)_
    - **RAILS_MASTER_KEY**: _(paste the contents from your `config/master.key` file)_
- (Wait a few minutes for the web service to be created)
- Click on your app url to see it work in the browser!
  - Be sure to visit a url that you defined in your routes, something like `https://___.onrender.com/photos.json`
- Once your app is deployed, remove `bundle exec rake db:seed;` from the Build command
  - Dashboard -> your backend app -> Settings -> Build Command -> Edit
  - Change the build command to `bundle install; bundle exec rake db:migrate;`

  Every time you push new changes to the GitHub main branch, your app will automatically redeploy!

## Deploy a React frontend

- In your terminal, navigate to your React app
  - Run `npm run dev` to make sure your app is working locally
  - Run `git status` to make sure all changes have been committed and pushed to GitHub
- On render.com, create a new static site: https://dashboard.render.com/static/new
  - **Connect a repository** _(choose one of your public GitHub repositories to deploy)_
  - **Name**: `<your_app_name>`
  - **Build command**: `npm run build`
  - **Publish directory**: `dist`
- (Wait a few minutes for the web service to be created)
  - _The build can crash if some npm libraries are not installed_
  - _For example, `npm install bootstrap` may work fine locally but crash in the production environment (missing @popperjs/core dependency_
  - _If the build fails, read the error messages carefully, install any missing packages locally (like `npm install @popperjs/core`), then push the changes to GitHub_
- Click on your app url to see it work in the browser!
  - Note that backend web requests won't work yet until you connect the backend and frontend (see next section)
- Configure routing requests to index.html so they can be handled by your routing library
  - Dashboard -> your frontend app -> Redirect/Rewrites -> Add Rule
  - **Source**: `/*` **Destination**: `/index.html` **Action**: `rewrite`
  - Click "Save Changes"

  Every time you push new changes to the GitHub main branch, your app will automatically redeploy!

## Connect the backend and frontend
- In the backend code, [configure cors](https://gist.github.com/peterxjang/77d6243cf85103b027a56b401b62b289) to allow web requests from the deployed frontend url
  - Example `cors.rb` file:
    ```ruby
    Rails.application.config.middleware.insert_before 0, Rack::Cors do
      allow do
        origins "example.com", "localhost:5173", "<your-frontend-domain>"
        resource "*", headers: :any, methods: [:get, :post, :patch, :put, :delete]
      end
    end
    ```
   - Once you configure cors, push the changes to GitHub to redeploy the backend

- In the frontend code, configure all production web requests to use the deployed backend url
  - Example `axios` configuration:
    ```javascript
    axios.defaults.baseURL = process.env.NODE_ENV === "development" ? "http://localhost:3000" : "<your-backend-url>";
    ```
   - Once you configure the web request urls, push the changes to GitHub to redeploy the frontend
