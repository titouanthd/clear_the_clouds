# Gardez votre code PHP à l'abri : 5 bonnes pratiques de sécurité à connaître

## 1. Injection SQL

Exemple de faille :
```
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```
Dans cet exemple, les variables $username et $password sont directement utilisées dans une requête SQL sans être échappées, ce qui permet à un attaquant d'injecter du code SQL malveillant dans ces variables et d'exécuter des requêtes non autorisées.

Correction :
```
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = :username AND password = :password");
$stmt->bindParam(':username', $username);
$stmt->bindParam(':password', $password);
$stmt->execute();
```
Dans cette correction, nous utilisons une requête préparée avec des paramètres nommés pour échapper automatiquement les valeurs utilisateur et éviter l'injection SQL.

## 2. Traversée du répertoire

Exemple de faille :
```
$file = $_GET['file'];
include('/path/to/files/' . $file);
```
Dans cet exemple, la valeur de la variable $_GET['file'] est directement utilisée pour inclure un fichier sans validation, ce qui permet à un attaquant de naviguer à travers la structure de fichiers du serveur et d'accéder à des fichiers sensibles.

Correction :
```
$file = $_GET['file'];
// Valider et filtrer la valeur de $_GET['file'] pour éviter la traversée du répertoire
if (preg_match('/^[a-zA-Z0-9_\-\.]+$/', $file)) {
    include('/path/to/files/' . $file);
} else {
    // Gérer l'erreur de façon sécurisée
    echo "Fichier non valide";
}
```
Dans cette correction, nous utilisons une validation basée sur une expression régulière pour s'assurer que la valeur de $_GET['file'] ne contient que des caractères autorisés, ce qui évite la traversée du répertoire.

## 3. Scripts intersites (attaques XSS)

Exemple de faille :
```
<div><?php echo $_GET['message']; ?></div>
```
Dans cet exemple, la valeur de la variable $_GET['message'] est directement affichée dans une balise HTML sans être échappée, ce qui permet à un attaquant d'injecter du code JavaScript malveillant dans cette valeur et d'exécuter des scripts sur le navigateur des utilisateurs.

Correction :
```
<div><?php echo htmlspecialchars($_GET['message'], ENT_QUOTES, 'UTF-8'); ?></div>
```
Dans cette correction, nous utilisons la fonction htmlspecialchars pour échapper les caractères spéciaux dans la valeur de $_GET['message'] et éviter les attaques XSS.

## 4. Stockage des mots de passe

Exemple de faille :
```
$password = $_POST['password'];
// Stockage du mot de passe en clair dans la base de données
$query = "INSERT INTO users (username, password) VALUES ('$username', '$password')";
```
Dans cet exemple, le mot de passe est stocké en clair dans la base de données, ce qui compromet la sécurité des utilisateurs en cas de fuite de données.

Correction :
```
$password = $_POST['password'];
// Hashage sécurisé du mot de passe avant de le stocker dans la base de données
$hash = password_hash($password, PASSWORD_DEFAULT);
$query = "INSERT INTO users (username, password) VALUES ('$username', '$hash')";
```
Dans cette correction, nous utilisons la fonction `password_hash` pour hasher de manière sécurisée le mot de passe avant de le stocker dans la base de données. Cela garantit que le mot de passe est stocké de manière sécurisée et ne peut pas être récupéré en clair en cas de fuite de données.

## 5. Détournement de session

Exemple de faille :
```
session_start();
$_SESSION['user_id'] = $_GET['user_id'];
```
Dans cet exemple, la valeur de la variable $_GET['user_id'] est directement assignée à la variable de session $_SESSION['user_id'] sans validation, ce qui permet à un attaquant de détourner la session d'un autre utilisateur en modifiant cette valeur.

Correction :
```
session_start();
$_SESSION['user_id'] = (int)$_GET['user_id'];
```
Dans cette correction, nous utilisons la validation de type en convertissant la valeur de $_GET['user_id'] en entier (int) pour éviter toute injection de code malveillant. Il est également important de mettre en place d'autres mesures de sécurité telles que la gestion de la durée de vie des sessions, l'utilisation de cookies sécurisés, et la régénération des identifiants de session pour renforcer la sécurité contre le détournement de session.