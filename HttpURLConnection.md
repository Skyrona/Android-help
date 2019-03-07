# HttpURLConnection

Cette classe est une extension de la classe **URLConnection**.


```java
   URL url = new URL("http://www.android.com/");
   HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
   try {
     InputStream in = new BufferedInputStream(urlConnection.getInputStream());
     readStream(in);
   } finally {
     urlConnection.disconnect();
   }
```

## HTTP Authentification

**HttpURLConnection** supporte l'authentification basique HTTP. On utilise l'**Authenticator**.

```java
Authenticator.setDefault(new Authenticator() {
     protected PasswordAuthentication getPasswordAuthentication() {
       return new PasswordAuthentication(username, password.toCharArray());
     }
   });
```

Cette méthode n'est pas sécurisée pour de l'authentification (à moins qu'elle soit lié avec du HTTPS). Le username, mot de passe, requête et réponse sont envoyés sur tout le réseau sans encryption.

## Cookies

Pour maintenir une longue session entre le client et le server, on peut utiliser un manager de cookie.

```java
CookieManager cookieManager = new CookieManager();
CookieHandler.setDefault(cookieManager);
```

Par défaut, le cookie manager accepte des cookies seulement du serveur d'origine. Deux autres polices sont incluses : 
* **CookiePolicy.ACCEPT_ALL**
* **CookiePolicy.ACCEPT_NONE**

On utilisera **CookiePolicy** pour définir une police.

Par défaut les cookies sont enregistrés en mémoire et seront effacés à la fin de la connexion.

On utilisera **CookieStore** pour définir un cookie store.

Pour être compatible avec la plupart des serveurs web, il faut mettre la version des cookies à zéro : par exemple pour avoir twitter en français.

```java
 HttpCookie cookie = new HttpCookie("lang", "fr");
cookie.setDomain("twitter.com");
cookie.setPath("/");
cookie.setVersion(0);
cookieManager.getCookieStore().add(new URI("http://twitter.com/"), cookie);
```

## Méthode HTTP

**HttpURLConnection** utilise la méthode **GET** par défaut.

La méthode **POST** sera utilisée si **setDoOutput(true)** a été appelé (permet d'envoyer de la donnée sur un serveur). 

D'autres méthodes peuvent être utilisées (**OPTIONS**, **HEAD**, **PUT**, **DELETE** et **TRACE**) avec **setRequestMethod(String)**.