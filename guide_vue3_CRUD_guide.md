# Guide: Vue.js v3 CRUD practice

## Backend setup

- In your terminal, navigate to your Rails backend app and run your server: `rails server`
- Make sure each RESTful route (index, create, show, update, destroy) is working by manually testing each request.
- Make sure `config/initializers/cors.rb` is configured to allow web requests from localhost:5173.

## Frontend setup

- In a new terminal window, navigate to your Actualize directory and create a new project:

  ```
  npm create vite@latest
  ```

  _When prompted, enter a project name, select the Vue framework, select the JavaScript variant_

- In the terminal, go to your new directory, install the dependencies, and start the dev server:

  ```
  cd <name-of-your-app>
  npm install
  npm run dev
  ```

- In a new terminal tab, install the axios library:

  ```
  npm install axios --save
  ```

- In the terminal, open your text editor for the frontend app:

  ```
  code .
  ```

- In your text editor, edit main.js as follows:

  ```diff
    import { createApp } from "vue";
    import "./style.css";
    import App from "./App.vue";
  + import axios from "axios";

  + axios.defaults.baseURL = process.env.NODE_ENV === "development" ? "http://localhost:3000" : "/";

    createApp(App).mount("#app");
  ```

- In your text editor, open the file src/App.vue and replace it with the following code:

  ```html
  <script>
    export default {
      data: function () {
        return {
          message: "Welcome to Vue.js!",
        };
      },
      created: function () {},
      methods: {},
    };
  </script>

  <template>
    <div class="home">
      <h1>{{ message }}</h1>
    </div>
  </template>

  <style></style>
  ```

- In your text editor, open the file src/style.css and delete the CSS code.

- In your browser, visit http://localhost:5173 to make sure your app is working!

## CRUD Single Page Application

Use this approach to display all backend CRUD actions on one frontend page.

- <details><summary>Index action</summary>

  ```html
  <script>
    import axios from "axios";
    export default {
      data: function () {
        return {
          photos: [],
        };
      },
      created: function () {
        this.indexPhotos();
      },
      methods: {
        indexPhotos: function () {
          axios.get("/photos.json").then((response) => {
            console.log("photos index", response);
            this.photos = response.data;
          });
        },
      },
    };
  </script>

  <template>
    <div class="home">
      <h1>All Photos</h1>
      <div v-for="photo in photos" v-bind:key="photo.id">
        <h2>{{ photo.name }}</h2>
        <img v-bind:src="photo.url" v-bind:alt="photo.name" />
        <p>Width: {{ photo.width }}</p>
        <p>Height: {{ photo.height }}</p>
      </div>
    </div>
  </template>

  <style></style>
  ```

  </details>


  - In `<script>` data: Define an empty array to store the items
  - In `<script>` methods: Define an index method with GET "/<ins>photos</ins>" to set the items
  - In `<script>` created: Run the index method
  - In `<template>`: Use v-for to display items with {{ }} and/or v-bind: for dynamic attributes

- <details><summary>New/create actions</summary>
    
    ```diff
      <script>
      import axios from "axios";
      export default {
        data: function () {
          return {
            photos: [],
    +       newPhotoParams: {},
          };
        },
        created: function () {
          this.indexPhotos();
        },
        methods: {
          indexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
    +     createPhoto: function () {
    +       axios
    +         .post("/photos.json", this.newPhotoParams)
    +         .then((response) => {
    +           console.log("photos create", response);
    +           this.photos.push(response.data);
    +           this.newPhotoParams = {};
    +         })
    +         .catch((error) => {
    +           console.log("photos create error", error.response);
    +         });
    +     },
        },
      };
      </script>

      <template>
        <div class="home">
    +     <h1>New Photo</h1>
    +     <div>
    +       Name:
    +       <input type="text" v-model="newPhotoParams.name" />
    +       Width:
    +       <input type="text" v-model="newPhotoParams.width" />
    +       Height:
    +       <input type="text" v-model="newPhotoParams.height" />
    +       Url:
    +       <input type="text" v-model="newPhotoParams.url" />
    +       <button v-on:click="createPhoto()">Create Photo</button>
    +     </div>
          <h1>All Photos</h1>
          <div v-for="photo in photos" v-bind:key="photo.id">
            <h2>{{ photo.name }}</h2>
            <img v-bind:src="photo.url" v-bind:alt="photo.name" />
            <p>Width: {{ photo.width }}</p>
            <p>Height: {{ photo.height }}</p>
          </div>
        </div>
      </template>

      <style></style>
    ```

  </details>

  - In `<script>` data: Make an empty object to store user inputted parameters
  - In `<script>` methods: Define a create function with POST "/<ins>photos</ins>" to create the item
  - In `<template>`: Use inputs with v-model & a button with v-on:click to run the create method

- <details><summary>Show action</summary>
    
    ```diff
      <script>
      import axios from "axios";
      export default {
        data: function () {
          return {
            photos: [],
            newPhotoParams: {},
    +       currentPhoto: {},
          };
        },
        created: function () {
          this.indexPhotos();
        },
        methods: {
          indexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          createPhoto: function () {
            axios
              .post("/photos.json", this.newPhotoParams)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
                this.newPhotoParams = {};
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
    +     showPhoto: function (photo) {
    +       this.currentPhoto = photo;
    +       document.querySelector("#photo-details").showModal();
    +     },
        },
      };
      </script>
      
      <template>
        <div class="home">
          <h1>New Photo</h1>
          <div>
            Name:
            <input type="text" v-model="newPhotoParams.name" />
            Width:
            <input type="text" v-model="newPhotoParams.width" />
            Height:
            <input type="text" v-model="newPhotoParams.height" />
            Url:
            <input type="text" v-model="newPhotoParams.url" />
            <button v-on:click="createPhoto()">Create Photo</button>
          </div>
          <h1>All Photos</h1>
          <div v-for="photo in photos" v-bind:key="photo.id">
            <h2>{{ photo.name }}</h2>
            <img v-bind:src="photo.url" v-bind:alt="photo.name" />
            <p>Width: {{ photo.width }}</p>
            <p>Height: {{ photo.height }}</p>
    +       <button v-on:click="showPhoto(photo)">More info</button>
          </div>
    +     <dialog id="photo-details">
    +       <form method="dialog">
    +         <h1>Photo info</h1>
    +         <p>Name: {{ currentPhoto.name }}</p>
    +         <p>Width: {{ currentPhoto.width }}</p>
    +         <p>Height: {{ currentPhoto.height }}</p>
    +         <p>Url: {{ currentPhoto.url }}</p>
    +         <button>Close</button>
    +       </form>
    +     </dialog>
        </div>
      </template>
      
      <style></style>    
    ```

  </details>
  
  - In `<script>` data: Make an empty object to keep track of the current item
  - In `<script>` methods: Define a show function to set the current item & open a modal
  - In `<template>`: Make the modal and a button with v-on:click to run the show method

- <details><summary>Edit/update actions</summary>
    
    ```diff
      <script>
      import axios from "axios";
      export default {
        data: function () {
          return {
            photos: [],
            newPhotoParams: {},
    +       editPhotoParams: {},
            currentPhoto: {},
          };
        },
        created: function () {
          this.indexPhotos();
        },
        methods: {
          indexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          createPhoto: function () {
            axios
              .post("/photos.json", this.newPhotoParams)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
                this.newPhotoParams = {};
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
          showPhoto: function (photo) {
            this.currentPhoto = photo;
    +       this.editPhotoParams = photo;
            document.querySelector("#photo-details").showModal();
          },
    +     updatePhoto: function (photo) {
    +       axios
    +         .patch("/photos/" + photo.id + ".json", this.editPhotoParams)
    +         .then((response) => {
    +           console.log("photos update", response);
    +           this.currentPhoto = {};
    +         })
    +         .catch((error) => {
    +           console.log("photos update error", error.response);
    +         });
    +     },
        },
      };
      </script>
      
      <template>
        <div class="home">
          <h1>New Photo</h1>
          <div>
            Name:
            <input type="text" v-model="newPhotoParams.name" />
            Width:
            <input type="text" v-model="newPhotoParams.width" />
            Height:
            <input type="text" v-model="newPhotoParams.height" />
            Url:
            <input type="text" v-model="newPhotoParams.url" />
            <button v-on:click="createPhoto()">Create Photo</button>
          </div>
          <h1>All Photos</h1>
          <div v-for="photo in photos" v-bind:key="photo.id">
            <h2>{{ photo.name }}</h2>
            <img v-bind:src="photo.url" v-bind:alt="photo.name" />
            <p>Width: {{ photo.width }}</p>
            <p>Height: {{ photo.height }}</p>
            <button v-on:click="showPhoto(photo)">More info</button>
          </div>
          <dialog id="photo-details">
            <form method="dialog">
              <h1>Photo info</h1>
    -         <p>Name: {{ currentPhoto.name }}</p>
    -         <p>Width: {{ currentPhoto.width }}</p>
    -         <p>Height: {{ currentPhoto.height }}</p>
    -         <p>Url: {{ currentPhoto.url }}</p>
    +         <p>Name: <input type="text" v-model="editPhotoParams.name" /></p>
    +         <p>Width: <input type="text" v-model="editPhotoParams.width" /></p>
    +         <p>Height: <input type="text" v-model="editPhotoParams.height" /></p>
    +         <p>Url: <input type="text" v-model="editPhotoParams.url" /></p>
    +         <button v-on:click="updatePhoto(currentPhoto)">Update</button>
              <button>Close</button>
            </form>
          </dialog>
        </div>
      </template>
      
      <style></style>
    ```

  </details>

  - In `<script>` data: Make an empty object to store user inputted parameters
  - In `<script>` methods: Define an update function with PATCH "/<ins>photos</ins>/" + <ins>photo</ins>.id
  - In `<template>`: Use inputs with v-model & a button with v-on:click to run the update method


- <details><summary>Destroy action</summary>
    
    ```diff
      <script>
      import axios from "axios";
      export default {
        data: function () {
          return {
            photos: [],
            newPhotoParams: {},
            editPhotoParams: {},
            currentPhoto: {},
          };
        },
        created: function () {
          this.indexPhotos();
        },
        methods: {
          indexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          createPhoto: function () {
            axios
              .post("/photos.json", this.newPhotoParams)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
                this.newPhotoParams = {};
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
          showPhoto: function (photo) {
            this.currentPhoto = photo;
            this.editPhotoParams = photo;
            document.querySelector("#photo-details").showModal();
          },
          updatePhoto: function (photo) {
            axios
              .patch("/photos/" + photo.id + ".json", this.editPhotoParams)
              .then((response) => {
                console.log("photos update", response);
                this.currentPhoto = {};
              })
              .catch((error) => {
                console.log("photos update error", error.response);
              });
          },
    +     destroyPhoto: function (photo) {
    +       axios.delete("/photos/" + photo.id + ".json").then((response) => {
    +         console.log("photos destroy", response);
    +         var index = this.photos.indexOf(photo);
    +         this.photos.splice(index, 1);
    +       });
    +     },
        },
      };
      </script>
      
      <template>
        <div class="home">
          <h1>New Photo</h1>
          <div>
            Name:
            <input type="text" v-model="newPhotoParams.name" />
            Width:
            <input type="text" v-model="newPhotoParams.width" />
            Height:
            <input type="text" v-model="newPhotoParams.height" />
            Url:
            <input type="text" v-model="newPhotoParams.url" />
            <button v-on:click="createPhoto()">Create Photo</button>
          </div>
          <h1>All Photos</h1>
          <div v-for="photo in photos" v-bind:key="photo.id">
            <h2>{{ photo.name }}</h2>
            <img v-bind:src="photo.url" v-bind:alt="photo.name" />
            <p>Width: {{ photo.width }}</p>
            <p>Height: {{ photo.height }}</p>
            <button v-on:click="showPhoto(photo)">More info</button>
          </div>
          <dialog id="photo-details">
            <form method="dialog">
              <h1>Photo info</h1>
              <p>Name: <input type="text" v-model="editPhotoParams.name" /></p>
              <p>Width: <input type="text" v-model="editPhotoParams.width" /></p>
              <p>Height: <input type="text" v-model="editPhotoParams.height" /></p>
              <p>Url: <input type="text" v-model="editPhotoParams.url" /></p>
              <button v-on:click="updatePhoto(currentPhoto)">Update</button>
    +         <button v-on:click="destroyPhoto(currentPhoto)">Destroy Photo</button>
              <button>Close</button>
            </form>
          </dialog>
        </div>
      </template>
      
      <style></style>
    ```

  </details>
  
  - In `<script>` methods: Define an destroy function with DELETE "/<ins>photos</ins>/" + <ins>photo</ins>.id
  - In `<template>`: Use a button with v-on:click to run the destroy method
