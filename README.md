# Xwitter2NitterRSS
Un bookmarklet pour créer un flux RSS à partir de la page d'un compte Xwitter :
```javascript
javascript:(function() {
  var twitterUrl = window.location.href;
  var usernameRegex = /twitter\.com\/([^\/]+)/;
  var match = twitterUrl.match(usernameRegex);
  
  if (match) {
    var username = match[1];
    var nitterUrl = 'https://nitter.nohost.network/' + username + '/rss'; // Cette instance a encore le flux RSS activé, ce qui n'est pas le cas de nitter.net
    
    var tempInput = document.createElement('input');
    tempInput.value = nitterUrl;
    document.body.appendChild(tempInput);
    tempInput.select();
    document.execCommand('copy');
    document.body.removeChild(tempInput);
    
    alert('L\'URL Nitter RSS a bien copiée dans le presse-papier : ' + nitterUrl);
  } else {
    alert('Impossible de trouver le nom du compte Twitter dans l\'URL.');
  }
})();

```
