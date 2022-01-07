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
