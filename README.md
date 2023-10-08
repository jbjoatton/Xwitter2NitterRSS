# Xwitter2NitterRSS
Un bookmarklet pour créer un flux RSS à partir de la page d'un compte X-Twitter :
```javascript
javascript:(function() {
  var twitterUrl = window.location.href;
  var usernameRegex = /twitter\.com\/([^\/]+)/;
  var match = twitterUrl.match(usernameRegex);
  
  if (match) {
    var username = match[1];
    var nitterUrl = 'https://nitter.nohost.network/' + username + '/rss';
    
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

Le flux RSS est généré sur une des instances du projet Nitter https://github.com/zedeus/nitter

L'instance hébergée sur nohost.network continue de générer le flux RSS à la différence de l'instance principale nitter.net.
