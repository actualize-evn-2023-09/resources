# Guide: Rails API CRUD practice

## Create a new Rails app
1.  Start with a clean slate

    - Close all your text editor windows, quit your text editor.
    - Find and kill any terminal Rails servers using Ctrl C.
    - Open Terminal and navigate to your Actualize folder.

2.  In your terminal, create a new rails project:

    <pre><code>rails new <ins><strong>name-of-your-app</strong></ins></code></pre>

3.  In the terminal, go to your new directory

    <pre><code>cd <ins><strong>name-of-your-app</strong></ins></code></pre>

4.  In the terminal, create the database by entering:

    ```
    rails db:create
    ```

5. In the terminal, start your server by entering:

    ```
    rails server
    ```

6. In your browser, visit http://localhost:3000 to make sure your app is working

> In your terminal, open a new tab by entering the shortcut Command T, then use git to save a version of your code.
> ```bash
> git init
> git add --all
> git commit -m "Initial commit"
> ```

## Create the data layer
1.  In the terminal, create a new model by entering:

    <pre><code>rails generate model <ins><strong>Photo</strong></ins> <ins><strong>name</strong></ins>:string <ins><strong>width</strong></ins>:integer <ins><strong>height</strong></ins>:integer</code></pre>

2.  In the terminal, enter the command to open your text editor:

    ```
    code .
    ```

3.  In your text editor, double check the generated migration code in the `db/migrate` folder

    - If you have any typos, you can fix them now

4.  In the terminal, run the migration code by entering:

    ```
    rails db:migrate
    ```

5. In your text editor, use the `rails console` or the `db/seeds.rb` file to create new items:

    <pre><code>
    <ins><strong>Photo</strong></ins>.create(<ins><strong>name: "Winter", width: 200, height: 150</strong></ins>)
    <ins><strong>Photo</strong></ins>.create(<ins><strong>name: "Family", width: 1024, height: 768</strong></ins>)</code></pre>

6. In the terminal, run the code in the seeds file by entering:

    ```
    rails db:seed
    ```

7. In the terminal, open the irb rails console by entering:

    ```
    rails console
    ```

8. In the terminal (irb rails console), you can double check your entries by entering:

    <pre><code><ins><strong>Photo</strong></ins>.all</code></pre>

9. In your terminal (irb rails console), exit the rails console by entering:

    ```
    exit
    ```
    
> In your terminal, use git to save a version of your code.
> ```bash
> git add --all
> git commit -m "Add model with seeds for initial data"
> ```

## Create the web layer

1. In the terminal, create JSON views by entering:

    <pre><code>rails generate jbuilder <ins><strong>Photo</strong></ins></code></pre>

2. Define the JSON output 
    - <details><summary>In <code>app/views/photos/<ins><strong>_photos</strong></ins>.json.jbuilder</code>, replace the code with your model's attributes</summary>
  
      ```ruby
        json.id photo.id
        json.name photo.name
        json.width photo.width
        json.height photo.height
        json.created_at photo.created_at
        json.updated_at photo.updated_at
      ```
      </details>
      
3.  In the terminal, create a new controller by entering:

    <pre><code>rails generate controller <ins><strong>Photos</strong></ins></code></pre>

4. Make the **INDEX** action
    - In <code>test/controllers/<ins><strong>photos</strong></ins>_controller_test.rb</code>, add a test for the index action
  
      <pre><code>  test "index" do
          get "/<ins><strong>photos</strong></ins>.json"
          assert_response 200

          data = JSON.parse(response.body)
          assert_equal <ins><strong>Photo</strong></ins>.count, data.length
        end</code></pre>
      
    - <details><summary>In <code>config/routes.rb</code>, add a route</summary>
  
      ```ruby
        get "/photos" => "photos#index"
      ```
  
      </details>

    - <details><summary>In <code>app/controllers/<ins><strong>photos</strong></ins>_controller.rb</code>, add an index action</summary>
  
      ```ruby
        def index
          @photos = Photo.all
          render :index
        end
      ```
  
      </details>
    - In the terminal, run `rails test` to make sure the code is working
      > You can also use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a GET request to <code>localhost:3000/<ins><strong>photos</strong></ins>.json</code> and see the response
      > 
      > You can also visit <code>localhost:3000/<ins><strong>photos</strong></ins>.json</code> in the browser to see the output
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add index action"
      > ```

5. Make the **CREATE** action
    - In <code>test/controllers/<ins><strong>photos</strong></ins>_controller_test.rb</code>, add a test for the create action
  
      <pre><code>  test "create" do
          assert_difference "<ins><strong>Photo</strong></ins>.count", 1 do
            post "/<ins><strong>photos</strong></ins>.json", params: { <ins><strong>name: "lake", width: 800, height: 600</strong></ins> }
            assert_response 200
          end
        end</code></pre>
      
    - <details><summary>In <code>config/routes.rb</code>, add a route</summary>
  
      ```ruby
        post "/photos" => "photos#create"
      ```
  
      </details>

    - <details><summary>In <code>app/controllers/<ins><strong>photos</strong></ins>_controller.rb</code>, add a create action</summary>
  
      ```ruby
        def create
          @photo = Photo.create(
            name: params[:name],
            width: params[:width],
            height: params[:height],
          )
          render :show
        end
      ```
  
      </details>
    - In `app/controllers/application_controller.rb`, add the following line to disable [Rails CSRF protection](https://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf) for JSON requests:
      ```diff
        class ApplicationController < ActionController::Base
      +   protect_from_forgery with: :exception, unless: -> { request.format.json? }
        end
      ```
    - In the terminal, run `rails test` to make sure the code is working
      > You can also use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a POST request to <code>localhost:3000/<ins><strong>photos</strong></ins>.json</code> with the appropriate parameters and see the response
      > 
      > You CANNOT test this in the browser address bar (since the browser defaults to a GET request)
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add create action"
      > ```

6. Make the **SHOW** action
    - In <code>test/controllers/<ins><strong>photos</strong></ins>_controller_test.rb</code>, add a test for the show action
  
      <pre><code>  test "show" do
          get "/<ins><strong>photos</strong></ins>/#{<ins><strong>Photo</strong></ins>.first.id}.json"
          assert_response 200

          data = JSON.parse(response.body)
          assert_equal ["id", <ins><strong>"name", "width", "height",</strong></ins> "created_at", "updated_at"], data.keys
        end</code></pre>
      
    - <details><summary>In <code>config/routes.rb</code>, add a route</summary>
  
      ```ruby
        get "/photos/:id" => "photos#show"
      ```
  
      </details>

    - <details><summary>In <code>app/controllers/<ins><strong>photos</strong></ins>_controller.rb</code>, add a show action</summary>
  
      ```ruby
        def show
          @photo = Photo.find_by(id: params[:id])
          render :show
        end
      ```
  
      </details>
    - In the terminal, run `rails test` to make sure the code is working
      > You can also use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a GET request to <code>localhost:3000/<ins><strong>photos</strong></ins>/<ins><strong>1</strong></ins>.json</code> and see the response
      > 
      > You can also visit <code>localhost:3000/<ins><strong>photos</strong></ins>/<ins><strong>1</strong></ins>.json</code> in the browser to see the output
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add show action"
      > ```

7. Make the **UPDATE** action
    - In <code>test/controllers/<ins><strong>photos</strong></ins>_controller_test.rb</code>, add a test for the update action
  
      <pre><code>  test "update" do
          <ins><strong>photo</strong></ins> = <ins><strong>Photo</strong></ins>.first
          patch "/<ins><strong>photos</strong></ins>/#{<ins><strong>photo</strong></ins>.id}.json", params: { <ins><strong>name: "Updated name"</strong></ins> }
          assert_response 200

          data = JSON.parse(response.body)
          assert_equal <ins><strong>"Updated name", data["name"]</strong></ins>
        end</code></pre>
      
    - <details><summary>In <code>config/routes.rb</code>, add a route</summary>
  
      ```ruby
        patch "/photos/:id" => "photos#update"
      ```
  
      </details>

    - <details><summary>In <code>app/controllers/<ins><strong>photos</strong></ins>_controller.rb</code>, add an update action</summary>
  
      ```ruby
        def update
          @photo = Photo.find_by(id: params[:id])
          @photo.update(
            name: params[:name] || @photo.name,
            width: params[:width] || @photo.width,
            height: params[:height] || @photo.height,
          )
          render :show
        end
      ```
  
      </details>
    - In the terminal, run `rails test` to make sure the code is working
      > You can also use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a PATCH request to <code>localhost:3000/<ins><strong>photos</strong></ins>/<ins><strong>1</strong></ins>.json</code> with the appropriate parameters and see the response
      > 
      > You CANNOT test this in the browser address bar (since the browser defaults to a GET request)
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add update action"
      > ```

8. Make the **DESTROY** action
    - In <code>test/controllers/<ins><strong>photos</strong></ins>_controller_test.rb</code>, add a test for the update action
  
      <pre><code>  test "destroy" do
          assert_difference "<ins><strong>Photo</strong></ins>.count", -1 do
            delete "/<ins><strong>photos</strong></ins>/#{<ins><strong>Photo</strong></ins>.first.id}.json"
            assert_response 200
          end
        end</code></pre>
      
    - <details><summary>In <code>config/routes.rb</code>, add a route</summary>
  
      ```ruby
        delete "/photos/:id" => "photos#destroy"
      ```
  
      </details>

    - <details><summary>In <code>app/controllers/<ins><strong>photos</strong></ins>_controller.rb</code>, add a show action</summary>
  
      ```ruby
        def destroy
          @photo = Photo.find_by(id: params[:id])
          @photo.destroy
          render json: { message: "Photo destroyed successfully" }
        end
      ```
  
      </details>
    - In the terminal, run `rails test` to make sure the code is working
      > You can also use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a DELETE request to <code>localhost:3000/<ins><strong>photos</strong></ins>/<ins><strong>1</strong></ins>.json</code> with the appropriate parameters and see the response
      > 
      > You CANNOT test this in the browser address bar (since the browser defaults to a GET request)
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add destroy action"
      > ```
