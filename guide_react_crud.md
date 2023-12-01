# Guide: React CRUD practice

## Backend setup

- In your terminal, navigate to your Rails backend app and run your server: `rails server`
- Make sure each RESTful route (index, create, show, update, destroy) is working by manually testing each request.
- Make sure to configure the [rack-cors](https://github.com/cyu/rack-cors) gem to allow web requests from `localhost:5173` (instructions [here](https://gist.github.com/peterxjang/77d6243cf85103b027a56b401b62b289)).

## Frontend setup

- In a new terminal window, navigate to your Actualize directory and create a new project:
  ```
  npm create vite@latest
  ```

  _When prompted, enter a project name, select the React framework, select the JavaScript variant_

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

- In your text editor, make a new file src/Header.jsx:

  ```jsx
  export function Header() {
    return (
      <header>
        <nav>
          <a href="#">Home</a> | <a href="#">Link</a>
        </nav>
      </header>
    )
  }
  ```

- In your text editor, make a new file src/Content.jsx:

  ```jsx
  export function Content() {
    return (
      <main>
        <h1>Welcome to React!</h1>
      </main>
    )
  }
  ```

- In your text editor, make a new file src/Footer.jsx:

  ```jsx
  export function Footer() {
    return (
      <footer>
        <p>Copyright 2022</p>
      </footer>
    )
  }
  ```

- In your text editor, open the file src/App.jsx and replace the code with the following:

  ```jsx
  import { Header } from "./Header";
  import { Content } from "./Content";
  import { Footer } from "./Footer";

  function App() {
    return (
      <div>
        <Header />
        <Content />
        <Footer />
      </div>
    )
  }

  export default App;
  ```

- In your text editor, open the file src/index.css and replace all the CSS code with the following:

  ```css
  html {
    scroll-behavior: smooth;
  }
  ```
  
- In your browser, visit http://localhost:5173 to make sure your app is working!

FIX FOR PROPS TYPE ERROR:
Add this line of code to your `eslintrc.cjs` file `rules` object:
```
 rules: {
    "react/prop-types": "off", <<<<<<< ADD THIS LINE!
    "react-refresh/only-export-components": ["warn", { allowConstantExport: true }],
  },
```

## CRUD Single Page Application

- Index action

  - <details><summary>Make a new <strong>src/<ins>Photos</ins>Index.jsx</strong> file that returns placeholder HTML</strong></summary>
  
      ```jsx
      export function PhotosIndex() {
        return (
          <div>
            <h1>All photos</h1>
          </div>
        );
      }
      ```
      </details>

  - <details><summary>In <strong>src/Content.jsx</strong>, import the <strong>src/<ins>Photos</ins>Index.jsx</strong> component</summary>
      
      ```diff
      + import { PhotosIndex } from "./PhotosIndex";
  
        export function Content() {
          return (
            <div>
      -       <h1>Welcome to React!</h1>
      +       <PhotosIndex />
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to see the placeholder HTML from the index component.
    
  - <details><summary>In <strong>src/Content.jsx</strong>, pass placeholder data to the index component as props</summary>
      
      ```diff
        import { PhotosIndex } from "./PhotosIndex";
  
        export function Content() {
      +   const photos = [
      +     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
      +     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
      +   ];
  
          return (
            <div>
      +       <PhotosIndex photos={photos} />
            </div>
          );
        }
      ```
      </details>
      
  - <details><summary>In <strong>src/<ins>Photos</ins>Index.jsx</strong>, map through the array given in props</summary>
      
      ```diff
      - export function PhotosIndex() {
      + export function PhotosIndex(props) {
          return (
            <div>
              <h1>All photos</h1>
      +       {props.photos.map((photo) => (
      +         <div key={photo.id}>
      +           <h2>{photo.name}</h2>
      +           <img src={photo.url} />
      +           <p>Width: {photo.width}</p>
      +           <p>Height: {photo.height}</p>
      +         </div>
      +       ))}
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to see the placeholder data in the index component.
    
  - <details><summary>In <strong>src/Content.jsx</strong>, use the useState and useEffect hooks to define the data via a backend web request</summary>
      
      ```diff
      + import axios from "axios";
      + import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
  
        export function Content() {
      -   const photos = [
      -     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
      -     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
      -   ];
  
      +   const [photos, setPhotos] = useState([]);

      +   const handleIndexPhotos = () => {
      +     console.log("handleIndexPhotos");
      +     axios.get("http://localhost:3000/photos.json").then((response) => {
      +       console.log(response.data);
      +       setPhotos(response.data);
      +     });
      +   };

      +   useEffect(handleIndexPhotos, []);
  
          return (
            <div>
              <PhotosIndex photos={photos} />
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to see the real data from the backend in the index component.
    
- New/Create actions

  - <details><summary>Make a new <strong>src/<ins>Photos</ins>New.jsx</strong> file that returns an HTML form</strong></summary>
  
      ```jsx
      export function PhotosNew() {
        return (
          <div>
            <h1>New Photo</h1>
            <form>
              <div>
                Name: <input name="name" type="text" />
              </div>
              <div>
                Url: <input name="url" type="text" />
              </div>
              <div>
                Width: <input name="width" type="text" />
              </div>
              <div>
                Height: <input name="height" type="text" />
              </div>
              <button type="submit">Create photo</button>
            </form>
          </div>
        );
      }
      ```
      </details>

  - <details><summary>In <strong>src/Content.jsx</strong>, import the <strong>src/<ins>Photos</ins>New.jsx</strong> component</summary>
      
      ```diff
        import axios from "axios";
        import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
      + import { PhotosNew } from "./PhotosNew";

        export function Content() {
          const [photos, setPhotos] = useState([]);

          const handleIndexPhotos = () => {
            console.log("handleIndexPhotos");
            axios.get("http://localhost:3000/photos.json").then((response) => {
              console.log(response.data);
              setPhotos(response.data);
            });
          };

          useEffect(handleIndexPhotos, []);

          return (
            <div>
      +       <PhotosNew />
              <PhotosIndex photos={photos} />
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to see the HTML form in the new component.

  - <details><summary>In <strong>src/Content.jsx</strong>, make a create function and pass it to the new component</summary>
      
      ```diff
  
        import axios from "axios";
        import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
        import { PhotosNew } from "./PhotosNew";

        export function Content() {
          const [photos, setPhotos] = useState([]);

          const handleIndexPhotos = () => {
            console.log("handleIndexPhotos");
            axios.get("http://localhost:3000/photos.json").then((response) => {
              console.log(response.data);
              setPhotos(response.data);
            });
          };

      +   const handleCreatePhoto = (params, successCallback) => {
      +     console.log("handleCreatePhoto", params);
      +     axios.post("http://localhost:3000/photos.json", params).then((response) => {
      +       setPhotos([...photos, response.data]);
      +       successCallback();
      +     });
      +   };
  
          useEffect(handleIndexPhotos, []);

          return (
            <div>
      -       <PhotosNew />
      +       <PhotosNew onCreatePhoto={handleCreatePhoto} />
              <PhotosIndex photos={photos} />
            </div>
          );
        }
      ```
      </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>New.jsx</strong>, handle the submit event to run the given props create function</strong></summary>
  
      ```diff
      - export function PhotosNew() {
      + export function PhotosNew(props) {

      +   const handleSubmit = (event) => {
      +     event.preventDefault();
      +     const params = new FormData(event.target);
      +     props.onCreatePhoto(params, () => event.target.reset());
      +   };

          return (
            <div>
              <h1>New Photo</h1>
      -       <form>
      +       <form onSubmit={handleSubmit}>
                <div>
                  Name: <input name="name" type="text" />
                </div>
                <div>
                  Url: <input name="url" type="text" />
                </div>
                <div>
                  Width: <input name="width" type="text" />
                </div>
                <div>
                  Height: <input name="height" type="text" />
                </div>
                <button type="submit">Create photo</button>
              </form>
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to fill in the HTML form and create a new item.
    
- Show action

  - <details><summary>Make a new <strong>src/Modal.css</strong> file with the following content:</summary>
  
    ```css
    .modal-background {
      display: block;
      position: fixed;
      top: 0;
      left: 0;
      width:100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.6);
      z-index: 1000;
    }

    .modal-main {
      position: fixed;
      background: white;
      width: 80%;
      height: auto;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      padding: 1em;
    }

    .modal-main button.close {
      font-size: 2em;
      background: none;
      border: none;
      position: absolute;
      top: 0em;
      right: 0.2em;
    }
    ```
    </details>

  - <details><summary>Make a new <strong>src/Modal.jsx</strong> file with the following content:</summary>
  
    ```jsx
    import "./Modal.css";

    export function Modal(props) {
      if (props.show) {
        return (
          <div className="modal-background">
            <section className="modal-main">
              {props.children}
              <button className="close" type="button" onClick={props.onClose}>
                &#x2715;
              </button>
            </section>
          </div>
        );
      }
    }
    ```
    </details>

  - <details><summary>In <strong>src/Content.jsx</strong>, import the modal component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
    + import { Modal } from "./Modal";

      export function Content() {
        const [photos, setPhotos] = useState([]);

        const handleIndexPhotos = () => {
          console.log("handleIndexPhotos");
          axios.get("http://localhost:3000/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreatePhoto = (params, successCallback) => {
          console.log("handleCreatePhoto", params);
          axios.post("http://localhost:3000/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        useEffect(handleIndexPhotos, []);

        return (
          <div>
            <PhotosNew onCreatePhoto={handleCreatePhoto} />
            <PhotosIndex photos={photos} />
    +       <Modal show={true}>
    +         <h1>Test</h1>
    +       </Modal>
          </div>
        );
      }
    ```
    </details>
    
    > Check the browser to see the modal with placeholder HTML

  - <details><summary>In <strong>src/Content.jsx</strong>, make state and functions to show and hide the modal</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { Modal } from "./Modal";

      export function Content() {
        const [photos, setPhotos] = useState([]);
    +   const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
    +   const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndexPhotos = () => {
          console.log("handleIndexPhotos");
          axios.get("http://localhost:3000/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreatePhoto = (params, successCallback) => {
          console.log("handleCreatePhoto", params);
          axios.post("http://localhost:3000/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
    +   const handleShowPhoto = (photo) => {
    +     console.log("handleShowPhoto", photo);
    +     setIsPhotosShowVisible(true);
    +     setCurrentPhoto(photo);
    +   };

    +   const handleClose = () => {
    +     console.log("handleClose");
    +     setIsPhotosShowVisible(false);
    +   };
  
        useEffect(handleIndexPhotos, []);

        return (
          <div>
            <PhotosNew onCreatePhoto={handleCreatePhoto} />
    -       <PhotosIndex photos={photos} />
    +       <PhotosIndex photos={photos} onShowPhoto={handleShowPhoto} />
    -       <Modal show={true}>
    +       <Modal show={isPhotosShowVisible} onClose={handleClose}>
              <h1>Test</h1>
            </Modal>
          </div>
        );
      }
    ```
    </details>
  
  - <details><summary>In <strong>src/<ins>Photos</ins>Index.jsx</strong>, make a button to show the modal</summary>
  
    ```diff
      export function PhotosIndex(props) {
        return (
          <div>
            <h1>All photos</h1>
            {props.photos.map((photo) => (
              <div key={photo.id}>
                <h2>{photo.name}</h2>
                <img src={photo.url} />
                <p>Width: {photo.width}</p>
                <p>Height: {photo.height}</p>
    +           <button onClick={() => props.onShowPhoto(photo)}>More info</button>
              </div>
            ))}
          </div>
        );
      }
    ```
    </details>
            
    > Check the browser to test showing and closing the modal with the appropriate buttons
            
  - <details><summary>Make a new <strong>src/<ins>Photos</ins>Show.jsx</strong> file that renders attributes from props</summary>
  
    ```jsx
    export function PhotosShow(props) {
      return (
        <div>
          <h1>Photo information</h1>
          <p>Name: {props.photo.name}</p>
          <p>Url: {props.photo.url}</p>
          <p>Width: {props.photo.width}</p>
          <p>Height: {props.photo.height}</p>
        </div>
      );
    }
    ```
    </details>

  - <details><summary>In <strong>src/Content.jsx</strong>, import the show component and use it in the modal</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
    + import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function Content() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndexPhotos = () => {
          console.log("handleIndexPhotos");
          axios.get("http://localhost:3000/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreatePhoto = (params, successCallback) => {
          console.log("handleCreatePhoto", params);
          axios.post("http://localhost:3000/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShowPhoto = (photo) => {
          console.log("handleShowPhoto", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };

        const handleClose = () => {
          console.log("handleClose");
          setIsPhotosShowVisible(false);
        };
  
        useEffect(handleIndexPhotos, []);

        return (
          <div>
            <PhotosNew onCreatePhoto={handleCreatePhoto} />
            <PhotosIndex photos={photos} onShowPhoto={handleShowPhoto} />
            <Modal show={isPhotosShowVisible} onClose={handleClose}>             
    -         <h1>Test</h1>
    +         <PhotosShow photo={currentPhoto} />
              </Modal>
          </div>
        );
      }
    ```
    </details>
    
    > Check the browser to see the selected item attributes within the modal

- Edit/Update actions

  - <details><summary>Modify <strong>src/<ins>Photos</ins>Show.jsx</strong> to include an edit form with default values</summary>
  
    ```diff
      export function PhotosShow(props) {
        return (
          <div>
            <h1>Photo information</h1>
            <p>Name: {props.photo.name}</p>
            <p>Url: {props.photo.url}</p>
            <p>Width: {props.photo.width}</p>
            <p>Height: {props.photo.height}</p>
    +       <form>
    +         <div>
    +           Name: <input defaultValue={props.photo.name} name="name" type="text" />
    +         </div>
    +         <div>
    +           Url: <input defaultValue={props.photo.url} name="url" type="text" />
    +         </div>
    +         <div>
    +           Width: <input defaultValue={props.photo.width} name="width" type="text" />
    +         </div>
    +         <div>
    +           Height: <input defaultValue={props.photo.height} name="height" type="text" />
    +         </div>
    +         <button type="submit">Update photo</button>
    +       </form>
          </div>
        );
      }
    ```
    </details>

    > Check the browser to see the edit HTML form within the modal

  - <details><summary>In <strong>src/Content.jsx</strong>, make an update function and pass it into the show component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function Content() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndexPhotos = () => {
          console.log("handleIndexPhotos");
          axios.get("http://localhost:3000/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreatePhoto = (params, successCallback) => {
          console.log("handleCreatePhoto", params);
          axios.post("http://localhost:3000/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShowPhoto = (photo) => {
          console.log("handleShowPhoto", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };
            
    +   const handleUpdatePhoto = (id, params, successCallback) => {
    +     console.log("handleUpdatePhoto", params);
    +     axios.patch(`http://localhost:3000/photos/${id}.json`, params).then((response) => {
    +       setPhotos(
    +         photos.map((photo) => {
    +           if (photo.id === response.data.id) {
    +             return response.data;
    +           } else {
    +             return photo;
    +           }
    +         })
    +       );
    +       successCallback();
    +       handleClose();
    +     });
    +   };

        const handleClose = () => {
          console.log("handleClose");
          setIsPhotosShowVisible(false);
        };
  
        useEffect(handleIndexPhotos, []);

        return (
          <div>
            <PhotosNew onCreatePhoto={handleCreatePhoto} />
            <PhotosIndex photos={photos} onShowPhoto={handleShowPhoto} />
            <Modal show={isPhotosShowVisible} onClose={handleClose}>
    -         <PhotosShow photo={currentPhoto} />
    +         <PhotosShow photo={currentPhoto} onUpdatePhoto={handleUpdatePhoto} />
            </Modal>
          </div>
        );
      }
    ```
    </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Show.jsx</strong>, handle the form submit to run the props update function</summary>
  
    ```diff
      export function PhotosShow(props) {
            
    +   const handleSubmit = (event) => {
    +     event.preventDefault();
    +     const params = new FormData(event.target);
    +     props.onUpdatePhoto(props.photo.id, params, () => event.target.reset());
    +   };
            
        return (
          <div>
            <h1>Photo information</h1>
            <p>Name: {props.photo.name}</p>
            <p>Url: {props.photo.url}</p>
            <p>Width: {props.photo.width}</p>
            <p>Height: {props.photo.height}</p>
    -       <form>
    +       <form onSubmit={handleSubmit}>
              <div>
                Name: <input defaultValue={props.photo.name} name="name" type="text" />
              </div>
              <div>
                Url: <input defaultValue={props.photo.url} name="url" type="text" />
              </div>
              <div>
                Width: <input defaultValue={props.photo.width} name="width" type="text" />
              </div>
              <div>
                Height: <input defaultValue={props.photo.height} name="height" type="text" />
              </div>
              <button type="submit">Update photo</button>
            </form>
          </div>
        );
      }
    ```
    </details>

    > Check the browser to edit an attribute using the HTML form and submit the form
            
- Destroy action

  - <details><summary>In <strong>src/Content.jsx</strong>, make a destroy function and pass it into the show component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function Content() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});

        const handleIndexPhotos = () => {
          console.log("handleIndexPhotos");
          axios.get("http://localhost:3000/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreatePhoto = (params, successCallback) => {
          console.log("handleCreatePhoto", params);
          axios.post("http://localhost:3000/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };

        const handleShowPhoto = (photo) => {
          console.log("handleShowPhoto", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };

        const handleClose = () => {
          console.log("handleClose");
          setIsPhotosShowVisible(false);
        };

        const handleUpdatePhoto = (id, params, successCallback) => {
          console.log("handleUpdatePhoto", params);
          axios.patch(`http://localhost:3000/photos/${id}.json`, params).then((response) => {
            setPhotos(
              photos.map((photo) => {
                if (photo.id === response.data.id) {
                  return response.data;
                } else {
                  return photo;
                }
              })
            );
            successCallback();
            handleClose();
          });
        };

    +   const handleDestroyPhoto = (photo) => {
    +     console.log("handleDestroyPhoto", photo);
    +     axios.delete(`http://localhost:3000/photos/${photo.id}.json`).then((response) => {
    +       setPhotos(photos.filter((p) => p.id !== photo.id));
    +       handleClose();
    +     });
    +   };

        useEffect(handleIndexPhotos, []);

        return (
          <div>
            <PhotosNew onCreatePhoto={handleCreatePhoto} />
            <PhotosIndex photos={photos} onShowPhoto={handleShowPhoto} />
            <Modal show={isPhotosShowVisible} onClose={handleClose}>
    -         <PhotosShow photo={currentPhoto} onUpdatePhoto={handleUpdatePhoto} />
    +         <PhotosShow photo={currentPhoto} onUpdatePhoto={handleUpdatePhoto} onDestroyPhoto={handleDestroyPhoto} />
            </Modal>
          </div>
        );
      }
    ```
    </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Show.jsx</strong>, make a button to run the props destroy function on click</summary>
  
    ```diff
      export function PhotosShow(props) {
        const handleSubmit = (event) => {
          event.preventDefault();
          const params = new FormData(event.target);
          props.onUpdatePhoto(props.photo.id, params, () => event.target.reset());
        };

    +   const handleClick = () => {
    +     props.onDestroyPhoto(props.photo);
    +   };

        return (
          <div>
            <h1>Photo information</h1>
            <form onSubmit={handleSubmit}>
              <div>
                Name: <input defaultValue={props.photo.name} name="name" type="text" />
              </div>
              <div>
                Url: <input defaultValue={props.photo.url} name="url" type="text" />
              </div>
              <div>
                Width: <input defaultValue={props.photo.width} name="width" type="text" />
              </div>
              <div>
                Height: <input defaultValue={props.photo.height} name="height" type="text" />
              </div>
              <button type="submit">Update photo</button>
            </form>
    +       <button onClick={handleClick}>Destroy photo</button>
          </div>
        );
      }
    ```
    </details>

    > Check the browser to destroy a photo by clicking the destroy button
