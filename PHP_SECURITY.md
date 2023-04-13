# Gardez votre code PHP à l'abri : 5 bonnes pratiques de sécurité à connaître

1. **Utiliser des requêtes SQL paramétrées** (ou préparées) avec PDO pour éviter les injections SQL.

Faille courante : Injection SQL

Exemple de faille :
```
// Code non sécurisé
$user = $_POST['user'];
$password = $_POST['password'];

$query = "SELECT * FROM users WHERE user = '$user' AND password = '$password'";
```

Correction :
```
// Code sécurisé avec requêtes SQL paramétrées et PDO
$user = $_POST['user'];
$password = $_POST['password'];

$query = "SELECT * FROM users WHERE user = :user AND password = :password";
$stmt = $pdo->prepare($query);
$stmt->bindParam(':user', $user);
$stmt->bindParam(':password', $password);
$stmt->execute();
```

2. **Valider, filtrer et échapper correctement toutes les données utilisateur avant de les utiliser dans des requêtes SQL ou dans le code PHP.**

Faille courante : Cross-Site Scripting (XSS)

Exemple de faille :
```
// Code non sécurisé
$name = $_GET['name'];
echo "Bienvenue, " . $name . "!";
```

Correction :
```
// Code sécurisé avec validation, filtrage et échappement des données utilisateur
$name = $_GET['name'];
$name = htmlspecialchars($name, ENT_QUOTES, 'UTF-8');
echo "Bienvenue, " . $name . "!";
```

3. **Mettre à jour régulièrement PHP ainsi que les frameworks, CMS et bibliothèques utilisés pour bénéficier des derniers correctifs de sécurité.**

Faille courante : Utilisation de versions obsolètes de PHP ou de bibliothèques vulnérables

Correction : Mettre à jour PHP, les frameworks, CMS et bibliothèques utilisés vers les dernières versions sécurisées et appliquer régulièrement les correctifs de sécurité.

4. **Configurer correctement les options de sécurité dans le fichier php.ini, notamment en désactivant les fonctions dangereuses et en activant les fonctions de sécurité appropriées.**

Faille courante : Utilisation de configurations php.ini non sécurisées

Correction : Configurer le fichier php.ini pour désactiver les fonctions dangereuses (comme eval() ou system()) et activer les fonctions de sécurité appropriées (comme open_basedir ou disable_functions) en fonction des besoins de l'application.

5. **Utiliser HTTPS pour chiffrer les communications entre le navigateur de l'utilisateur et le serveur, afin de protéger les données sensibles transmises sur le réseau.**

Faille courante : Transmission non sécurisée de données sensibles sur le réseau

Correction : Utiliser HTTPS avec un certificat SSL valide pour chiffrer les communications entre le navigateur de l'utilisateur et le serveur, afin de protéger les données sensibles (comme les informations d'identification ou les données de paiement) transmises sur le réseau.

sources:
[Comment sécuriser le site PHP](https://www.getastra.com/blog/php-security/comment-securiser-le-site-php/)
[PHP Manual - Sécurité - Injection SQL](https://www.php.net/manual/fr/security.database.sql-injection.php)
[Sécurité PHP](https://blog.lesjeudis.com/securite-php)