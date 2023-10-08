# Bookmarklet Twitter → RSS

Un [Bookmarklet](https://fr.wikipedia.org/wiki/Bookmarklet) pour créer un flux [RSS](https://fr.wikipedia.org/wiki/RSS) à partir d'un compte X-Twitter.

## Code

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
    
    alert('L\'URL du flux RSS a bien été copiée dans le presse-papier : ' + nitterUrl);
  } else {
    alert('Impossible de trouver le nom du compte Twitter dans l\'URL.');
  }
})();
```

## Installation
- Créer un favori/bookmark/marque-page directement sur la barre des favoris du navigateur.
- Copier le code ci-dessus et le coller dans le champ 'URL'.

## Utilisation
- Une fois sur la page d'un compte X-Twitter, cliquer sur le bookmarklet. L'url du flux RSS est générée et copiée dans le presse-papier.
- Ajouter le flux dans votre lecteur RSS préféré.

## Précisions
- Le flux RSS est généré sur une des instances du projet [Nitter](https://github.com/zedeus/nitter). 
- L'instance hébergée sur `nohost.network` continue de générer les flux RSS à la différence de l'instance principale `nitter.net`.

---

Une autre version permettant de choisir ou non d'inclure les réponses du compte dans le flux RSS généré :

```javascript
javascript:(function() {
  var twitterUrl = window.location.href;
  var usernameRegex = /twitter\.com\/([^\/]+)/;
  var match = twitterUrl.match(usernameRegex);

  if (match) {
    var username = match[1];

    var popupHTML = `
      <body style="b">
        <div style="font-family: sans-serif">
          <h3>Twitter → flux RSS</h3>
          <label for="withRepliesCheckbox">
            <input type="checkbox" id="withRepliesCheckbox">Inclure les réponses
          </label>
          <br><br>
          <button id="generateButton">Générer l'URL du flux RSS</button>
        </div>
      </body>
    `;

    var popup = window.open('', 'TwitterToNitterPopup', 'width=220,height=160');
    popup.document.body.innerHTML = popupHTML;

    popup.document.getElementById('generateButton').addEventListener('click', function() {
      var withReplies = popup.document.getElementById('withRepliesCheckbox').checked;
      var nitterUrl = 'https://nitter.nohost.network/' + username + (withReplies ? '/with_replies/rss' : '/rss');

      var tempInput = popup.document.createElement('input');
      tempInput.value = nitterUrl;
      popup.document.body.appendChild(tempInput);
      tempInput.select();
      popup.document.execCommand('copy');
      popup.document.body.removeChild(tempInput);

      alert('L\'URL du flux RSS a bien été copiée dans le presse-papier : ' + nitterUrl);
      popup.close();
    });
  } else {
    alert('Impossible de trouver le nom du compte Twitter dans l\'URL.');
  }
})();
```
