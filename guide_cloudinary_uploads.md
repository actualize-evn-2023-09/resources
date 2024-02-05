# Guide: File uploads with Cloudinary

## Backend Rails setup

- Sign up for an account from https://cloudinary.com to get an API key.

- In the terminal, open the rails credentials file:

  ```bash
  rails credentials:edit
  ```

- Add the cloud name, api key, and api secret values from https://cloudinary.com/console to the credentials file, then save and close the credentials file.

  ```yml
  cloudinary:
    cloud_name: *****
    api_key: *****
    api_secret: *****
  ```

- In your Gemfile, add the [cloudinary](https://github.com/cloudinary/cloudinary_gem) gem, then run `bundle install` in the terminal.

  ```ruby
  gem 'cloudinary'
  ```

- In the `config/initializers` folder, make a new file called `cloudinary.rb` and paste the following:

  ```ruby
  Cloudinary.config do |config|
    config.cloud_name =  Rails.application.credentials.cloudinary[:cloud_name]
    config.api_key =  Rails.application.credentials.cloudinary[:api_key]
    config.api_secret = Rails.application.credentials.cloudinary[:api_secret]
    config.secure = true
    config.cdn_subdomain = true
  end
  ```

- Restart your rails server. Now you can upload files on your backend using the [Cloudinary::Uploader.upload](https://cloudinary.com/documentation/rails_image_and_video_upload#rails_image_upload) method.

- For example, if you have a `Post` model with `title`, `body`, and `image` attributes, you could have a `PostsController` `create` method as follows:

  ```ruby
  def create
    response = Cloudinary::Uploader.upload(params[:image_file], resource_type: :auto)
    cloudinary_url = response["secure_url"]

    post = Post.new(
      title: params[:title],
      body: params[:body],
      image: cloudinary_url # use this instead of params
    )
    if post.save
      render json: post
    else
      render json: {errors: post.errors.full_messages}, status: 422
    end
  end
  ```

  For this example, the frontend must send `params[:image_file]` as a file upload instead of normal string params. See the next section for a frontend example.

## Frontend Vue.js setup

_This example sends a `POST "/posts"` web request with params for `title`, `body`, and `image_file` using Vue.js and axios (`image_file` is a file upload as opposed to a normal string params)._

Upload a file from an HTML frontend using Vue.js and axios:

- Note the HTML `<input type="file">` tag with the `v-on:change`
- Note the JavaScript use of `FormData` instead of normal parameters

```html
<template>
  <div>
    <form v-on:submit.prevent="submit()">
      <h2>Create a new post:</h2>
      <div>
        Title:
        <input type="text" v-model="title" />
      </div>
      <div>
        Body:
        <input type="text" v-model="body" />
      </div>
      <div>
        Image:
        <input type="file" v-on:change="setFile($event)" ref="fileInput" />
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        title: "",
        body: "",
        imageFile: "",
      };
    },
    created: function () {},
    methods: {
      setFile: function (event) {
        if (event.target.files.length > 0) {
          this.imageFile = event.target.files[0];
        }
      },
      submit: function () {
        var formData = new FormData();
        formData.append("title", this.title);
        formData.append("body", this.body);
        formData.append("image_file", this.imageFile);

        axios.post("/posts", formData).then((response) => {
          this.title = "";
          this.body = "";
          this.$refs.fileInput.value = "";
        });
      },
    },
  };
</script>
```

## Frontend vanilla HTML setup (no JavaScript)

_This example sends a `POST "/posts"` web request with params for `title`, `body`, and `image_file` using a plain HTML form (`image_file` is a file upload as opposed to a normal string params)._

Upload a file from an HTML frontend using a plain HTML form:

- Note the form tag attribute `enctype="multipart/form-data"`
- Note the input tag attribute `type="file"`

```html
<form action="/posts" method="post" enctype="multipart/form-data">
  <h2>Create a new post:</h2>
  <div>
    Title:
    <input type="text" name="title" />
  </div>
  <div>
    Body:
    <input type="text" name="body" />
  </div>
  <div>
    Image:
    <input type="file" name="image_file" />
  </div>
  <input type="submit" value="Submit" />
</form>
```
