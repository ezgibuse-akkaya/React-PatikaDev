# React-PatikaDev

[Patika.dev](https://app.patika.dev/egitimler) adresindeki "Bootcamp Hızlandırma Programı-Javascript Patikası React" eğitiminde kullandığım kaynak kodları içeren bir repo.

Bu README dosyasında Patika.Dev React Ödev Soruları Ve Yanıtlarını bulacaksınız.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## :brain: Ödev 1

### :question: SORU 
-  Bu fonksiyon **"async"** olarak tanımlanmalı ve default olarak dışa aktarılmalıdır. Fonksiyonun içindeki asenkron fonksiyonlar **"await"** ile tanımlanmalıdır.
-  Fonksiyon **Number** tipinde tek parametre alır. Bu parametre **user id**'yi belirtir.
-  Fonksiyonun görevi aşağıdaki endpoint'e giderek parametrede verilen user id ile ilgili kullanıcının verilerini çekmek olmalı. İstekleri **"axios"** kütüphanesini kullanarak yapmanız gerekiyor. İsteği yaparken aşağıdaki endpointin sonundaki rakamı parametrede gelen user id'ile değiştirmeniz gerekiyor.

[https://jsonplaceholder.typicode.com/users/1](https://jsonplaceholder.typicode.com/users/1)

-  Yine aynı fonksiyonun içerisinde ve yine aynı user id için bir de "posts" isteği yapılmalıdır.İsteği yaparken aşağıdaki endpoint'in sonundaki rakamı parametrede gelen user id'ile değiştirmeniz gerekiyor.

[https://jsonplaceholder.typicode.com/posts?userId=1](https://jsonplaceholder.typicode.com/posts?userId=1)

-  Artık elimizde kullanıcı bilgileri ve bu kullanıcının post'ları var. Bu iki veriyi birleştirip return edin. Birleştirme sonucunda elinizde aşağıdaki gibi bir obje bulunması gerekiyor.

	```
	{
		id: 1,
		name: "Leanne Graham",
		username: "Bret",
		email: "Sincere@april.biz",
		address: {
			street: "Kulas Light",
			suite: "Apt. 556",
			city: "Gwenborough",
			zipcode: "92998-3874",
			geo: {
				lat: "-37.3159",
				lng: "81.1496"
			}
		},
		phone: "1-770-736-8031 x56442",
		website: "hildegard.org",
		company: {
			name: "Romaguera-Crona",
			catchPhrase: "Multi-layered client-server neural-net",
			bs: "harness real-time e-markets"
		},
		posts: [{
			userId: 1,
			id: 1,
			title: "sunt aut facere repellat",
			body: "quia et suscipit suscipit recusandae"
		}]
	}
	```
"app.js" dosyasına yazmış olduğunuz "getData" isimli fonksiyonu "import" edin.
Bir alt satırda bu fonksiyonu çalıştırın ve gelen sonucu log'layın.

### :green_square: CEVAP

<details>
<summary>Kodu görmek için tıklayınız.</summary>

  ```javascript	
import axios from "axios";
const USER_API_URL = 'https://jsonplaceholder.typicode.com/users';
const USER_POSTS_API_URL = 'https://jsonplaceholder.typicode.com/posts?userId=';
export default async function GetUserByIdWithPosts(id) {
id = Number(id);
if(id < 1) return;
return new Promise(async function(resolve, reject) {
const {data} = await axios(`${USER_API_URL}/${id}`);     
const posts = await new Promise(async function(resolve, reject) {
const posts = await axios(`${USER_POSTS_API_URL}${id}`);
resolve(posts.data);
});
data.posts = [...posts];
resolve(data);
});    
}
const userInfo = await GetUserByIdWithPosts(1);
console.log(userInfo);
 ```
</details>
	
## :brain: Ödev 2

### :question: SORU 
Aşağıda çalışır hali paylaşılan çalışmayı bir React uygulaması olarak çalışır hale getirmek.

[Çalışmanın orijinal hali](https://codepen.io/dmitrysharabin/pen/MWgQNYZ)

Yukarıda paylaşılan ikinci bağlantıda orijinal çalışmada bulunan ancak sizin işinize yaramayacak olan tanımlar kaldırıldı. O bağlantıdaki HTML ve CSS tanımlarını kullanarak geliştirmeye başlayabilirsiniz.

Dosya dizin isimlendirmesini dilediğiniz gibi yapabilirsiniz.
### :green_square: CEVAP

<details>
<summary>Kodu görmek için tıklayınız.</summary>
	
Footer.js
```javascript
import React from "react";

const Footer = ({
  filteredList,
  setActiveCategory,
  activeCategory,
  handleClear,
}) => {
  return (
    <footer className="footer">
      <span className="todo-count">
        <strong>{filteredList.length} </strong>
        item{filteredList.length > 1 && "s"} left
      </span>
      <ul className="filters">
        <li onClick={() => setActiveCategory("All")}>
          <a className={activeCategory === "All" && "selected"}>All</a>
        </li>
        <li onClick={() => setActiveCategory("Active")}>
          <a className={activeCategory === "Active" && "selected"}>Active</a>
        </li>
        <li onClick={() => setActiveCategory("Completed")}>
          <a className={activeCategory === "Completed" && "selected"}>
            Completed
          </a>
        </li>
      </ul>

      <button className="clear-completed" onClick={handleClear}>
        Clear completed
      </button>
    </footer>
  );
};

export default Footer;
```
	
Header.js
```javascript
import React, { useState } from "react";
import { v4 as uuid } from "uuid";
const Header = ({ tasks, setTasks }) => {
  const [value, setValue] = useState("");

  const onFormSubmit = (event) => {
    // Formun yenilenmesini engelliyorum.
    event.preventDefault();
    const task = {
      // Eklenen taskların unique bir degeri olması için uuid kütüphanesinden yararlandım. bu id'ye göre tamamlama ve silem islemleri yapılıyor.
      id: uuid(),
      title: value,
      isCompleted: false,
    };
    // Eklene task'ı state üzerinde güncelliyorum.
    setTasks([task, ...tasks]);
    setValue("");
  };

  return (
    <header className="header">
      <h1>todos</h1>
      <form onSubmit={onFormSubmit}>
        <input
          className="new-todo"
          placeholder="What needs to be done?"
          value={value}
          onChange={(event) => setValue(event.target.value)}
        />
      </form>
    </header>
  );
};

export default Header;
```
List.js
```Javascript
import React from "react";
import Task from "./Task";

const List = ({ handleToggleCompleted, handleDelete, filteredList }) => {
  return (
    <ul className="todo-list">
      {filteredList.map((task) => (
        <Task
          key={task.id}
          task={task}
          handleToggleCompleted={handleToggleCompleted}
          handleDelete={handleDelete}
        />
      ))}
    </ul>
  );
};

export default List;
```
Main.js
```Javascript
import React from "react";
import Footer from "./Footer";
import List from "./List";

const Main = ({
  handleToggleCompleted,
  handleDelete,
  setActiveCategory,
  filteredList,
  activeCategory,
  handleClear,
}) => {
  return (
    <section>
      <input className="toggle-all" type="checkbox" />
      <label for="toggle-all">Mark all as complete</label>
      <List
        filteredList={filteredList}
        handleToggleCompleted={handleToggleCompleted}
        handleDelete={handleDelete}
      />
      <Footer
        filteredList={filteredList}
        setActiveCategory={setActiveCategory}
        activeCategory={activeCategory}
        handleClear={handleClear}
      />
    </section>
  );
};

export default Main;
```
Task.js
```javascript
import React from "react";

const Task = ({ task, handleToggleCompleted, handleDelete }) => {
  return (
    <li className={task.isCompleted ? "completed" : null}>
      <div className="view">
        <input
          className="toggle"
          type="checkbox"
          onClick={() => handleToggleCompleted(task.id)}
        />
        <label>{task.title}</label>
        <button
          className="destroy"
          onClick={() => handleDelete(task.id)}
        ></button>
      </div>
    </li>
  );
};

export default Task;
```
App.css
```css
/* index.css */
html,
body {
  margin: 0;
  padding: 0;
}

button {
  margin: 0;
  padding: 0;
  border: 0;
  background: none;
  font-size: 100%;
  vertical-align: baseline;
  font-family: inherit;
  font-weight: inherit;
  color: inherit;
  -webkit-appearance: none;
  appearance: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font: 14px "Helvetica Neue", Helvetica, Arial, sans-serif;
  line-height: 1.4em;
  background: #f5f5f5;
  color: #4d4d4d;
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 300;
}

:focus {
  outline: 0;
}

.hidden {
  display: none;
}

.todoapp {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.todoapp input::-webkit-input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp input::-moz-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp input::input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp h1 {
  position: absolute;
  top: -155px;
  width: 100%;
  font-size: 100px;
  font-weight: 100;
  text-align: center;
  color: rgba(175, 47, 47, 0.15);
  -webkit-text-rendering: optimizeLegibility;
  -moz-text-rendering: optimizeLegibility;
  text-rendering: optimizeLegibility;
}

.new-todo,
.edit {
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  color: inherit;
  padding: 6px;
  border: 1px solid #999;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.new-todo {
  padding: 16px 16px 16px 60px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}

.main {
  position: relative;
  z-index: 2;
  border-top: 1px solid #e6e6e6;
}

.toggle-all {
  width: 1px;
  height: 1px;
  border: none; /* Mobile Safari */
  opacity: 0;
  position: absolute;
  right: 100%;
  bottom: 100%;
}

.toggle-all + label {
  width: 60px;
  height: 34px;
  font-size: 0;
  position: absolute;
  top: -52px;
  left: -13px;
  -webkit-transform: rotate(90deg);
  transform: rotate(90deg);
}

.toggle-all + label:before {
  content: "❯";
  font-size: 22px;
  color: #e6e6e6;
  padding: 10px 27px 10px 27px;
}

.toggle-all:checked + label:before {
  color: #737373;
}

.todo-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

.todo-list li {
  position: relative;
  font-size: 24px;
  border-bottom: 1px solid #ededed;
}

.todo-list li:last-child {
  border-bottom: none;
}

.todo-list li.editing {
  border-bottom: none;
  padding: 0;
}

.todo-list li.editing .edit {
  display: block;
  width: calc(100% - 43px);
  padding: 12px 16px;
  margin: 0 0 0 43px;
}

.todo-list li.editing .view {
  display: none;
}

.todo-list li .toggle {
  text-align: center;
  width: 40px;
  /* auto, since non-WebKit browsers doesn't support input styling */
  height: auto;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
  border: none; /* Mobile Safari */
  -webkit-appearance: none;
  appearance: none;
}

.todo-list li .toggle {
  opacity: 0;
}

.todo-list li .toggle + label {
  /*
		Firefox requires `#` to be escaped - https://bugzilla.mozilla.org/show_bug.cgi?id=922433
		IE and Edge requires *everything* to be escaped to render, so we do that instead of just the `#` - https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/7157459/
	*/
  background-image: url("data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: center left;
}

.todo-list li .toggle:checked + label {
  background-image: url("data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E");
}

.todo-list li label {
  word-break: break-all;
  padding: 15px 15px 15px 60px;
  display: block;
  line-height: 1.2;
  transition: color 0.4s;
}

.todo-list li.completed label {
  color: #d9d9d9;
  text-decoration: line-through;
}

.todo-list li .destroy {
  display: none;
  position: absolute;
  top: 0;
  right: 10px;
  bottom: 0;
  width: 40px;
  height: 40px;
  margin: auto 0;
  font-size: 30px;
  color: #cc9a9a;
  margin-bottom: 11px;
  transition: color 0.2s ease-out;
}

.todo-list li .destroy:hover {
  color: #af5b5e;
}

.todo-list li .destroy:after {
  content: "×";
}

.todo-list li:hover .destroy {
  display: block;
}

.todo-list li .edit {
  display: none;
}

.todo-list li.editing:last-child {
  margin-bottom: -1px;
}

.footer {
  color: #777;
  padding: 10px 15px;
  height: 20px;
  text-align: center;
  border-top: 1px solid #e6e6e6;
}

.footer:before {
  content: "";
  position: absolute;
  right: 0;
  bottom: 0;
  left: 0;
  height: 50px;
  overflow: hidden;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6,
    0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6,
    0 17px 2px -6px rgba(0, 0, 0, 0.2);
}

.todo-count {
  float: left;
  text-align: left;
}

.todo-count strong {
  font-weight: 300;
}

.filters {
  margin: 0;
  padding: 0;
  list-style: none;
  position: absolute;
  right: 0;
  left: 0;
}

.filters li {
  display: inline;
}

.filters li a {
  color: inherit;
  margin: 3px;
  padding: 3px 7px;
  text-decoration: none;
  border: 1px solid transparent;
  border-radius: 3px;
  cursor: pointer; /* Because of Mavo: we don't use the href attribute in such situations */
}

.filters li a:hover {
  border-color: rgba(175, 47, 47, 0.1);
}

.filters li a.selected {
  border-color: rgba(175, 47, 47, 0.2);
}

.clear-completed,
html .clear-completed:active {
  float: right;
  position: relative;
  line-height: 20px;
  text-decoration: none;
  cursor: pointer;
}

.clear-completed:hover {
  text-decoration: underline;
}

.info {
  margin: 65px auto 0;
  color: #bfbfbf;
  font-size: 10px;
  text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
  text-align: center;
}

.info p {
  line-height: 1;
}

.info a {
  color: inherit;
  text-decoration: none;
  font-weight: 400;
}

.info a:hover {
  text-decoration: underline;
}

/*
	Hack to remove background from Mobile Safari.
	Can't use it globally since it destroys checkboxes in Firefox
*/
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  .toggle-all,
  .todo-list li .toggle {
    background: none;
  }

  .todo-list li .toggle {
    height: 40px;
  }
}

@media (max-width: 430px) {
  .footer {
    height: 50px;
  }

  .filters {
    bottom: 10px;
  }
}

/* Mavo */

[property]:hover,
[mv-app] [mv-mode="edit"] .mv-editor {
  box-shadow: none !important;
}

[mv-app] [mv-mode="edit"] .mv-editor {
  width: 100% !important;
}

[property="todo"]:focus-within {
  border-bottom: 0;
}

[property="todo"] label:focus-within {
  padding: 0;
  margin: 0;
  background-image: none !important;
}

[property="todo"].completed label:focus-within {
  color: #4d4d4d;
  text-decoration: none;
}

[mv-app] [mv-mode="edit"] .mv-editor:focus {
  width: calc(100% - 78px) !important;
  padding: 14px 16px !important;
  margin: 0 0 0 43px !important;
  border: 1px solid #999 !important;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2) !important;
}

.mv-item-bar,
.mv-ui.mv-message,
button.mv-add,
meta,
[property="todo"] .view:focus-within .toggle,
[property="todo"] .view:focus-within button {
  display: none !important;
}
```
App.js
```javascript
import { useState } from "react";
import "./App.css";
import Header from "./components/Header";
import Main from "./components/Main";

function App() {
  const [tasks, setTasks] = useState([]);
  const [activeCategory, setActiveCategory] = useState("All");

  // activeCategory state'ine göre kosullu render islemi. Componentlere tasks state'i yerine filteredList gönderiliyor.
  const filteredList =
    activeCategory === "All"
      ? tasks
      : activeCategory === "Active"
      ? tasks.filter((task) => task.isCompleted !== true)
      : tasks.filter((task) => task.isCompleted !== false);

  // Task tamamla fonksiyonu
  const handleToggleCompleted = (id) => {
    // Gelen id'ye göre task'ı bulup geri dönderir.
    const updatedTask = tasks.find((task) => task.id === id);
    // Task'ın isCompleted durumunu bir önceki durumunun tersi yapar.
    updatedTask.isCompleted = !updatedTask.isCompleted;
    // Güncellenen taskı state içerisinde yenisi ile değiştirir.
    const newTasks = tasks.map((task) => (task.id === id ? updatedTask : task));
    // State'i günceller.
    setTasks(newTasks);
  };

  // Task silme fonksiyonu
  const handleDelete = (id) => {
    // Gelen id'ye göre state'i filtreler
    setTasks(tasks.filter((task) => task.id !== id));
  };

  // Tüm taskları temizleme fonksiyonu
  const handleClear = () => {
    // Task state'ni boş bir diziye çevirir.
    setTasks([]);
  };

  return (
    <section className="todoapp">
      <Header setTasks={setTasks} tasks={tasks} />
      <Main
        filteredList={filteredList}
        tasks={tasks}
        handleToggleCompleted={handleToggleCompleted}
        handleDelete={handleDelete}
        setActiveCategory={setActiveCategory}
        activeCategory={activeCategory}
        handleClear={handleClear}
      />
    </section>
  );
}

export default App;
```
<details/>
