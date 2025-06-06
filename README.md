# 🌐 Déploiement sécurisé d’une application Web sur Azure App Service

Projet réalisé dans le cadre du cours **CYB1123 – Sécurité de l’infonuagique et des services Web** à l’UQO.

## 🎯 Objectif

Documenter la création d’un **Azure App Service**, le développement d’une appli Web simple dans **Visual Studio** et son déploiement, en évaluant l’accessibilité et la sécurité post-production.

---

## 🔑 Points clés du rapport

| Étape | Sujet | Points abordés |
|-------|-------|----------------|
| 1 | Création de l’App Service | Ressource group `webapp-rg`, nom d’hôte `sankarauqo.azurewebsites.net`, choix de la région Canada Central |
| 2 | Développement local | Appli .NET/ASP.NET testée sur `localhost:5273` |
| 3 | Déploiement | Publication via Visual Studio (profil HTTPS) |
| 4 | Vérifications sécurité | • Réponse *403 – Web app stopped* sans authentification.<br>• Déploiement via HTTPS → intégrité & authenticité.<br>• Risque limité à une mauvaise gestion des accès IAM. |

---

## 📸 Captures d’écran

1. **Instance Azure** – aperçu après provisionnement  
2. **Adresse locale** – exécution de l’appli sur `localhost`  
3. **Publication Visual Studio** – fenêtre « Publish » et log de déploiement  
4. **Site en ligne** – page d’accueil sur `https://sankarauqo.azurewebsites.net`  

*(Fichiers dans `/screenshots`)*

---

## 🚀 Reproduire le labo

1. **Créer l’App Service**  
   ```bash
   az group create --name webapp-rg --location "canada central"
   az appservice plan create -g webapp-rg -n webapp-plan --sku B1 --is-linux false
   az webapp create -g webapp-rg -p webapp-plan -n sankarauqo --runtime "DOTNET:8"

Coder & tester localement (dotnet run → https://localhost:5273)

Publier depuis Visual Studio

Profil : Azure App Service (Windows)

Méthode : Web Deploy (HTTPS)

Vérifier l’accessibilité

Session Azure fermée → https://sankarauqo.azurewebsites.net

Résultat attendu : 403 – This web app is stopped (pas accessible sans relancer l’instance)

🛡️ Analyse sécurité rapide
Sujet	État	Recommandation
Accès anonyme	Bloqué (403)	Mettre en place authentification Azure AD si l’app doit être active
Transport	HTTPS	Renforcer avec HSTS
Code source	Déploiement signé via Web Deploy	Limiter les rôles « Contributor » et activer MFA
Logs	Diagnostics par défaut off	Activer Application Insights & Log Analytics
