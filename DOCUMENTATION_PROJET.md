# 📋 Documentation Complète - Gestion des Employés

## Table des Matières

1. [Vue d'ensemble du projet](#vue-densemble)
2. [Architecture technique](#architecture-technique)
3. [Modèle de données](#modèle-de-données)
4. [Fonctionnalités principales](#fonctionnalités-principales)
5. [Interfaces utilisateur](#interfaces-utilisateur)
6. [Guide d'utilisation](#guide-dutilisation)
7. [Structure du code](#structure-du-code)

---

## 🎯 Vue d'ensemble

### Nom du projet

**Gestion des Employés (Employee Manager)**

### Objectif

Application web Django permettant la gestion complète d'une base de données d'employés avec les opérations **CRUD** (Create, Read, Update, Delete).

### Technologies utilisées

- **Framework Web** : Django 6.0.2
- **Base de données** : SQLite (db.sqlite3)
- **Frontend** : HTML5 + Tailwind CSS + DaisyUI
- **Langage serveur** : Python 3.14
- **Port d'écoute** : 8000

### Environnement

- Virtualenv : `/env/`
- Serveur de développement : Django development server
- OS : Linux

---

## 🏗️ Architecture Technique

### Structure des dossiers

```
learn_django/
├── manage.py                          # Script de gestion Django
├── db.sqlite3                         # Base de données SQLite
├── employe/                           # Application principale
│   ├── models.py                      # Modèles de données
│   ├── views.py                       # Logique métier (vues)
│   ├── urls.py                        # Routage des URLs
│   ├── forms.py                       # Formulaires Django
│   ├── admin.py                       # Interface d'administration
│   ├── apps.py                        # Configuration de l'app
│   ├── migrations/                    # Migrations de BD
│   │   ├── 0001_initial.py           # Migration initiale
│   │   └── __pycache__/
│   ├── templates/employe/             # Templates HTML
│   │   ├── base.html                 # Template de base
│   │   ├── list.html                 # Affichage liste employés
│   │   ├── formulaire.html           # Formulaire ajout/modification
│   │   └── confirmer_suppression.html # Confirmation suppression
│   ├── __pycache__/
│   └── tests.py                       # Tests unitaires
│
├── employe_project/                   # Configuration Django
│   ├── settings.py                    # Paramètres du projet
│   ├── urls.py                        # URLs principales
│   ├── asgi.py                        # Serveur ASGI
│   ├── wsgi.py                        # Serveur WSGI
│   └── __pycache__/
│
└── env/                               # Environnement virtuel
    └── lib/python3.14/site-packages/
        ├── django/                    # Framework Django
        ├── asgiref/                   # Support ASGI
        └── sqlparse/                  # Parser SQL
```

### Pattern MVC/MVT utilisé

L'application suit le pattern **MTV** (Model-Template-View) de Django :

```
        ┌─────────────────────┐
        │   URL Dispatcher    │  (urls.py)
        │  (Routage HTTP)     │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │     VIEW            │  (views.py)
        │  (Logique métier)   │
        └──────────┬──────────┘
                   │
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
    ┌────────┐         ┌──────────┐
    │ MODEL  │         │ TEMPLATE │
    │ (BD)   │         │  (HTML)  │
    └────────┘         └──────────┘
```

---

## 🗄️ Modèle de Données

### Entité : Employe

```python
class Employe(models.Model):
    nom = models.CharField(max_length=100)
    email = models.EmailField()
    poste = models.CharField(max_length=100)
    salaire = models.DecimalField(max_digits=10, decimal_places=2)
```

### Attributs

| Champ     | Type           | Validation               | Description                                 |
| --------- | -------------- | ------------------------ | ------------------------------------------- |
| `id`      | Integer (Auto) | PK                       | Identifiant unique (généré automatiquement) |
| `nom`     | String         | Max 100 char             | Nom complet de l'employé                    |
| `email`   | Email          | Format email             | Adresse email professionnelle               |
| `poste`   | String         | Max 100 char             | Titre/position du poste                     |
| `salaire` | Decimal        | 10 chiffres, 2 décimales | Salaire mensuel en FCFA                     |

### Exemple d'enregistrement

```json
{
  "id": 1,
  "nom": "Jean Dupont",
  "email": "jean.dupont@entreprise.com",
  "poste": "Développeur",
  "salaire": 450000.0
}
```

---

## 🎯 Fonctionnalités Principales

### 1. **Lister les employés** (READ)

- **URL** : `/` ou `/liste/`
- **Méthode HTTP** : GET
- **Fonction** : `liste_employes()`
- **Description** : Affiche tous les employés enregistrés dans la base de données
- **Données affichées** : Nom, Email, Poste, Salaire
- **Actions disponibles** : Modifier, Supprimer, Ajouter nouveau

### 2. **Ajouter un employé** (CREATE)

- **URL** : `/ajouter/`
- **Méthode HTTP** : GET (formulaire) / POST (soumission)
- **Fonction** : `ajouter_employe()`
- **Description** : Affiche un formulaire pour créer un nouvel employé
- **Champs** : nom, email, poste, salaire
- **Validation** : Effectuée par le formulaire Django ModelForm
- **Redirection après succès** : Vers la liste des employés

### 3. **Modifier un employé** (UPDATE)

- **URL** : `/modifier/<int:id>`
- **Méthode HTTP** : GET (pré-remplissage) / POST (soumission)
- **Fonction** : `modifier_employe(id)`
- **Description** : Édite les informations d'un employé existant
- **Pré-remplissage** : Les données actuelles sont affichées dans le formulaire
- **Validation** : Même validation que l'ajout
- **Redirection après succès** : Vers la liste des employés

### 4. **Supprimer un employé** (DELETE)

- **URL** : `/supprimer/<int:id>`
- **Méthode HTTP** : GET (confirmation) / POST (suppression)
- **Fonction** : `supprimer_employe(id)`
- **Description** : Supprime un employé après confirmation
- **Confirmation** : Écran de confirmation avant suppression définitive
- **Protection** : Gestion des erreurs 404 si l'employé n'existe pas
- **Redirection après succès** : Vers la liste des employés

---

## 🖥️ Interfaces Utilisateur

### Interface 1 : Liste des Employés

**URL** : `http://127.0.0.1:8000/`

**Description** : Page d'accueil affichant tous les employés avec actions

**Composants** :

```
┌─────────────────────────────────────────┐
│  GESTION DES EMPLOYÉS - PAGE PRINCIPALE │
├─────────────────────────────────────────┤
│                                         │
│  [+ Ajouter un employé] (Bouton bleu)  │
│                                         │
├─────────────────────────────────────────┤
│  Employé #1                             │
│  ├─ Nom: Jean Dupont                   │
│  ├─ Email: jean.dupont@entreprise.com  │
│  ├─ Poste: Développeur                 │
│  ├─ Salaire: 450000 FCFA               │
│  └─ [Modifier] [Supprimer]             │
├─────────────────────────────────────────┤
│  Employé #2                             │
│  ├─ Nom: Marie Martin                  │
│  ├─ Email: marie.martin@entreprise.com │
│  ├─ Poste: Designer UX/UI              │
│  ├─ Salaire: 380000 FCFA               │
│  └─ [Modifier] [Supprimer]             │
├─────────────────────────────────────────┤
│                                         │
└─────────────────────────────────────────┘
```

**Styles appliqués** :

- Thème : Forest (DaisyUI)
- Conteneur : Couleur base-200 avec coins arrondis
- Cartes employés : Couleur base-100
- Badges : Affichage email, poste, salaire

---

### Interface 2 : Formulaire d'Ajout/Modification

**URL** : `http://127.0.0.1:8000/ajouter/` ou `/modifier/<id>`

**Description** : Formulaire pour créer ou éditer un employé

**Composants** :

```
┌─────────────────────────────────────────┐
│      FORMULAIRE D'EMPLOYÉ               │
├─────────────────────────────────────────┤
│                                         │
│  Nom*                                   │
│  [____________ Nom ________]            │
│                                         │
│  Email*                                 │
│  [____________ Email ________]          │
│                                         │
│  Poste*                                 │
│  [____________ Poste ________]          │
│                                         │
│  Salaire*                               │
│  [____________ Salaire ________]        │
│                                         │
│  [Enregistrer] (Bouton bleu)            │
│                                         │
└─────────────────────────────────────────┘
```

**Validation** :

- Les champs sont obligatoires
- Format d'email vérifié
- Salaire doit être un nombre avec jusqu'à 2 décimales
- Classes CSS Tailwind appliquées : `input w-full`

---

### Interface 3 : Confirmation de Suppression

**URL** : `http://127.0.0.1:8000/supprimer/<id>`

**Description** : Pour confirmation avant suppression définitive

**Composants** :

```
┌─────────────────────────────────────────┐
│         CONFIRMATION DE SUPPRESSION     │
├─────────────────────────────────────────┤
│                                         │
│  Êtes-vous sûr de vouloir supprimer     │
│  Jean Dupont ?                          │
│                                         │
│  [Oui, Supprimer] (Bouton rouge)        │
│  [Annuler] (Bouton bleu)                │
│                                         │
└─────────────────────────────────────────┘
```

**Comportement** :

- POST : Supprime l'employé et redirige vers la liste
- Annuler : Redirige vers la liste sans supprimer

---

## 📖 Guide d'Utilisation

### Scénario 1 : Ajouter un nouvel employé

**Étapes** :

1. Accéder à `http://127.0.0.1:8000/`
2. Cliquer sur le bouton **[+ Ajouter un employé]**
3. Remplir le formulaire :
   - **Nom** : Jean Dupont
   - **Email** : jean.dupont@entreprise.com
   - **Poste** : Développeur Python
   - **Salaire** : 450000
4. Cliquer sur **[Enregistrer]**
5. ✅ Vous êtes redirigé vers la liste et le nouvel employé apparaît

### Scénario 2 : Modifier un employé existant

**Étapes** :

1. Sur la page principale, localiser l'employé à modifier
2. Cliquer sur le bouton **[Modifier]** de cet employé
3. Le formulaire se pré-remplit avec ses données actuelles
4. Modifier les champs souhaités
5. Cliquer sur **[Enregistrer]**
6. ✅ Les modifications sont sauvegardées et vous retournez à la liste

### Scénario 3 : Supprimer un employé

**Étapes** :

1. Sur la page principale, localiser l'employé à supprimer
2. Cliquer sur le bouton **[Supprimer]** de cet employé
3. Une page demande confirmation : "Êtes-vous sûr de vouloir supprimer [Nom] ?"
4. **Options** :
   - Cliquer **[Oui, Supprimer]** : suppression définitive
   - Cliquer **[Annuler]** : retour à la liste sans suppression
5. ✅ Si confirmé, l'employé est supprimé et vous retournez à la liste

### Scénario 4 : Consulter la liste des employés

**Étapes** :

1. Accéder à `http://127.0.0.1:8000/`
2. La page affiche tous les employés avec :
   - Informations personnelles
   - Poste occupé
   - Salaire en FCFA
3. Pour chaque employé, deux boutons sont disponibles :
   - **Modifier** : MàJ les infos
   - **Supprimer** : Supprime l'employé

---

## 💻 Structure du Code

### 1. Modèle (models.py)

```python
from django.db import models

class Employe(models.Model):
    nom = models.CharField(max_length=100)
    email = models.EmailField()
    poste = models.CharField(max_length=100)
    salaire = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.nom
```

**Points clés** :

- Héritage de `models.Model`
- Champs avec validations intégrées
- Méthode `__str__()` pour représentation texte

### 2. Formulaire (forms.py)

```python
from django import forms
from .models import Employe

class EmployeForm(forms.ModelForm):
    class Meta:
        model = Employe
        fields = ['nom', 'email', 'poste', 'salaire']
        widgets = {
            'nom': forms.TextInput(attrs={
                'class': 'input w-full',
                'placeholder': 'Nom'
            }),
            'email': forms.TextInput(attrs={
                'class': 'input w-full',
                'placeholder': 'Email'
            }),
            'poste': forms.TextInput(attrs={
                'class': 'input w-full',
                'placeholder': 'Poste'
            }),
            'salaire': forms.TextInput(attrs={
                'class': 'input w-full',
                'placeholder': 'Salaire'
            })
        }
```

**Points clés** :

- `ModelForm` : Génère le formulaire automatiquement depuis le modèle
- `widgets` : Personnalisation des champs avec classes CSS Tailwind
- Placeholders pour améliorer l'UX

### 3. Vues (views.py)

#### Vue 1 : liste_employes()

```python
def liste_employes(request):
    employes = Employe.objects.all()
    return render(request, 'employe/list.html', {'employes': employes})
```

- Récupère tous les employés de la BD
- Les passe au template

#### Vue 2 : ajouter_employe()

```python
def ajouter_employe(request):
    form = EmployeForm(request.POST or None)
    if form.is_valid():
        form.save()
        return redirect('liste_employes')
    return render(request, 'employe/formulaire.html', {'form': form})
```

- GET : Affiche le formulaire vierge
- POST : Valide et sauvegarde les données

#### Vue 3 : modifier_employe(id)

```python
def modifier_employe(request, id):
    employe = get_object_or_404(Employe, id=id)
    form = EmployeForm(request.POST or None, instance=employe)
    if form.is_valid():
        form.save()
        return redirect('liste_employes')
    return render(request, 'employe/formulaire.html', {'form': form})
```

- Récupère l'employé (erreur 404 si absent)
- Pré-remplit le formulaire
- Sauvegarde les modifications

#### Vue 4 : supprimer_employe(id)

```python
def supprimer_employe(request, id):
    employe = get_object_or_404(Employe, id=id)
    if request.method == "POST":
        employe.delete()
        return redirect('liste_employes')
    return render(request, 'employe/confirmer_suppression.html', {'employe': employe})
```

- GET : Affiche page de confirmation
- POST : Supprime et redirige

### 4. URLs (urls.py)

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.liste_employes, name='liste_employes'),
    path('ajouter/', views.ajouter_employe, name='ajouter_employe'),
    path('modifier/<int:id>', views.modifier_employe, name='modifier_employe'),
    path('supprimer/<int:id>', views.supprimer_employe, name='supprimer_employe'),
]
```

**Correspondances** :
| URL | Vue | Nom |
|-----|-----|-----|
| `/` | liste_employes | `liste_employes` |
| `/ajouter/` | ajouter_employe | `ajouter_employe` |
| `/modifier/<id>` | modifier_employe | `modifier_employe` |
| `/supprimer/<id>` | supprimer_employe | `supprimer_employe` |

### 5. Templates

#### base.html

- Template parent
- Contient la structure HTML de base
- Utilise DaisyUI pour le design
- Thème "forest"

#### list.html

- Affiche la liste de tous les employés
- Boucle sur `employes`
- Affiche badges pour email, poste, salaire

#### formulaire.html

- Formulaire réutilisable (ajout et modification)
- Rendu automatique du formulaire Django
- Protection CSRF via `{% csrf_token %}`

#### confirmer_suppression.html

- Page de confirmation avec le nom de l'employé
- Deux boutons : Supprimer ou Annuler

---

## 🔒 Sécurité

### Mesures implémentées :

1. **Protection CSRF** : `{% csrf_token %}` dans tous les formulaires
2. **Validation des entrées** : Effectuée par les champs Django
3. **Gestion des erreurs 404** : `get_object_or_404()` pour les ressources inexistantes
4. **Redirection POST** : Prévention de la double soumission

---

## 🚀 Démarrage de l'Application

### Prérequis

- Python 3.14+
- Virtualenv

### Installation

```bash
# Créer l'environnement virtuel
python3 -m venv env

# Activer l'environnement
source env/bin/activate

# Installer Django
pip install django~=6.0

# Appliquer les migrations
python manage.py migrate

# Créer un superutilisateur (optionnel)
python manage.py createsuperuser
```

### Lancer le serveur

```bash
python manage.py runserver 0.0.0.0:8000
```

**URL d'accès** : http://127.0.0.1:8000/

---

## 📊 Flux de Données

```
UTILISATEUR
    │
    ├─── GET /
    │    └──► liste_employes()
    │         └──► Employe.objects.all()
    │              └──► list.html (affichage)
    │
    ├─── GET /ajouter/
    │    └──► ajouter_employe()
    │         └──► formulaire.html (formulaire vierge)
    │
    ├─── POST /ajouter/ + données
    │    └──► ajouter_employe()
    │         └──► EmployeForm.save()
    │              └──► INSERT BD
    │                   └──► Redirect /
    │
    ├─── GET /modifier/1
    │    └──► modifier_employe(1)
    │         └──► Employe.objects.get(id=1)
    │              └──► formulaire.html (pré-rempli)
    │
    ├─── POST /modifier/1 + données
    │    └──► modifier_employe(1)
    │         └──► EmployeForm.save()
    │              └──► UPDATE BD
    │                   └──► Redirect /
    │
    ├─── GET /supprimer/1
    │    └──► supprimer_employe(1)
    │         └──► confirmer_suppression.html
    │
    └─── POST /supprimer/1
         └──► supprimer_employe(1)
              └──► Employe.objects.get(id=1).delete()
                   └──► DELETE BD
                        └──► Redirect /
```

---

## 📈 Améliorations Possibles

1. **Authentification** : Ajouter login/logout
2. **Pagination** : Pour listes longues
3. **Recherche** : Barre de recherche dans la liste
4. **Tri** : Trier par nom, salaire, etc.
5. **Validation avancée** : Vérifier les doublons d'email
6. **Export** : Exporter en CSV/PDF
7. **API REST** : Ajouter une API Django REST
8. **Tests** : Tests unitaires complets
9. **Logging** : Historique des modifications
10. **Permissions** : Contrôle d'accès basé sur les rôles

---

## ✅ Résumé Technique

| Aspect         | Détail                         |
| -------------- | ------------------------------ |
| **Framework**  | Django 6.0.2                   |
| **BD**         | SQLite                         |
| **Python**     | 3.14                           |
| **Frontend**   | Tailwind CSS + DaisyUI         |
| **Pattern**    | MTV (Django)                   |
| **Opérations** | CRUD complète                  |
| **Sécurité**   | CSRF, Validation, 404 handling |
| **État**       | ✅ Fonctionnel                 |

---

**Document généré le** : 12/02/2026  
**Application** : Gestion des Employés  
**Version** : 1.0
