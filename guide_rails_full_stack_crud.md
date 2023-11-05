# Guide: Rails Full Stack CRUD practice

## Create a new Rails app

- (refer to the Rails API CRUD practice guide)

## Create the data layer

- (refer to the Rails API CRUD practice guide)

## Create the web layer

1. In `config/routes.rb`, use the [resources shortcut](https://guides.rubyonrails.org/getting_started.html#resourceful-routing) to define all the routes

   ```ruby
   resources :photos

   # Automatically generates the following routes
   # get "/photos" => "photos#index", as: "photos"
   # get "/photos/new" => "photos#new", as: "new_photo"
   # post "/photos" => "photos#create"
   # get "/photos/:id" => "photos#show", as: "photo"
   # get "/photos/:id/edit" => "photos#edit", as: "edit_photo"
   # patch "/photos/:id" => "photos#update"
   # delete "/photos/:id" => "photos#destroy"
   ```

2. Make the [index action](https://gist.github.com/peterxjang/657be7f14974ec79217904f46c746a2e)

3. Make the [show action](https://gist.github.com/peterxjang/a32ef0e6a7231621137b0afff343cda7)

4. Make the [new/create actions](https://gist.github.com/peterxjang/bd31797a9b81c0bf9e6d655804865c2c)

5. Make the [edit/update actions](https://gist.github.com/peterxjang/b478ae23b35bf5ad1246749ee827a62e)

6. Make the [destroy action](https://gist.github.com/peterxjang/1bdc821c5563fb3d4291e40f041ffce6)

## Optional steps

- In `app/controllers/photos_controller.rb`, use strong parameters to DRY up the new and create actions ([example](https://guides.rubyonrails.org/getting_started.html#using-strong-parameters))

- In `app/models/photo.rb`, add validations, then in `app/controllers/photos_controller.rb`, add happy/sad paths, then in `app/views/new.html.erb`, display validation errors ([example](https://guides.rubyonrails.org/getting_started.html#validations-and-displaying-error-messages))

- In `app/views/new.html.erb` and `app/views/edit.html.erb`, use view partials to DRY up the form ([example](https://guides.rubyonrails.org/getting_started.html#using-partials-to-share-view-code))

- Add system tests to test the frontend: https://guides.rubyonrails.org/testing.html#implementing-a-system-test
