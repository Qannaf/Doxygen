# Initiation à Doxygen pour C et C++
# I. Introduction
À l'image de Javadoc, l'outil d'autodocumentation pour Java, Doxygen permet de créer des documentations techniques pour notamment le C et le C++, mais couvre également d'autres langages, y compris Java !

Ce qui sera étudié dans ce tutoriel, c'est l'utilisation basique de Doxygen avec un petit détour vers des fonctions avancées pour générer des documentations de références par exemple celles de GTK+.

## II. Les balises standards
Ce chapitre vous présente succinctement les balises les plus utilisées avec Doxygen. Pour la liste complète, rendez-vous sur le site officiel: Special Commands.

* A. Les blocs de documentation
Diverses combinaisons sont possibles pour créer des blocs de documentation, dans le style C ou C++ pour notre cas !
```ruby
Style C avec deux *
/**
 * ... Documentation ...
 */
Style C avec un !
/*!
 * ... Documentation ...
 */
Style C++ avec trois /
///
/// ... Documentation ...
///
Style C++ avec un !
//!
//! ... Documentation ...
//!
```
* B. \file

Permet de créer un bloc de documentation pour un fichier source ou d'en-tête.
```ruby
Déclaration
\file [<name>]
Exemple 
\file main.c
```
* C. \brief

Permet de créer une courte description dans un bloc de documentation. La description peut se faire sur une seule ligne ou plusieurs.
```ruby
Déclaration 
\brief {brief description}
Exemple : description sur une seule ligne 
\brief Courte description.
Exemple : description sur plusieurs lignes 
\brief Courte description.
       Suite de la courte description.
```
* D. \author▲

Permet d'ajouter un nom d'auteur (ex. : l'auteur d'un fichier ou d'une fonction). Plusieurs balises peuvent être présentes, lors de la génération un seul paragraphe Auteur sera créé, mais les listes d'auteurs seront séparées d'une ligne vide. Une seule balise peut accueillir plusieurs auteurs.
```ruby
Déclaration 
\author { list of authors }
Exemple 
\author Qannaf.AL-SAHMI
```
* E. \version

Permet l'ajout du numéro de version (ex. : dans le bloc de documentation d'un fichier).
```ruby
Déclaration 
\version { version number }
Exemple 
\version 0.1
```
* F. \date

Permet l'ajout d'une date. Plusieurs balises peuvent être présentes, lors de la génération un seul paragraphe Date sera créé, mais chaque date sera séparée par une ligne vide. Une seule balise peut accueillir plusieurs dates.
```ruby
Déclaration 
\date { date description }
Exemple 
\date 10 septembre 2007
```
* G. \struct

Permet la création d'un bloc de documentation pour une structure. Cette balise peut prendre jusqu'à trois arguments qui sont dans l'ordre :

Nom de la structure ;
Nom du fichier d'en-tête (ex. : le fichier où elle est définie) ;
Nom optionnel pour masquer le nom affiché par le second argument.
Si le second argument est fourni, un lien HTML vers le code source du fichier d'en-tête spécifié sera créé. Le dernier argument permet quant à lui de simplement changer éventuellement le nom de ce lien qui par défaut est le nom du fichier fournit en second argument.
```ruby
Déclaration 
\struct <name> [<header-file>] [<header-name>]
Exemple 
\struct Str_t
Exemple 
\struct Str_t str.h "Définition"
```
* H. \enum

Permet la création d'un bloc de documentation pour une énumération de constantes.
```ruby
Déclaration 
\enum <name>
Exemple 
\enum Str_err_e
```
* I. \union
Permet la création d'un bloc de documentation pour une union. L'utilisation est la même que pour une structure (voir le chapitre G).
```ruby
Déclaration 
\union <name> [<header-file>] [<header-name>]
```
* J. \fn

Permet la création d'un bloc de documentation pour une fonction ou méthode.
```ruby
Déclaration 
\fn (function declaration)
Exemple 
\fn int main (void)
Il est possible d'omettre cette balise lorsque le bloc de documentation commence juste au-dessus de la déclaration/définition de la fonction/méthode. Doxygen ajoutera de lui-même la signature de la fonction ou de la méthode. On peut donc en conclure que nous pouvons documenter une fonction à peu près n'importe où dans le code, mais dans ce cas, il faut ajouter la balise !
```
* K. \def

Permet la création d'un bloc de documentation pour une macro ou constante symbolique.
```ruby
Déclaration 
\def <name>
Exemple 
\def MAX(x,y)
```
* L. \param

Permet d'ajouter un paramètre dans un bloc de documentation d'une fonction ou d'une macro. Le premier paramètre est le nom de l'argument à documenter et le second le bloc de description le concernant.
```ruby
Déclaration 
\param <parameter-name> { parameter description }
Exemple 
\param self Pointeur sur un objet de type Str_t.
```
Cette balise permet aussi d'ajouter en option un tag pour montrer qu'il s'agit d'un argument entrant, sortant ou les deux en précisant au choix : [in], [out] ou [in, out] de cette manière :
```ruby
Exemple avec un tag [in] 
\param[in] self Pointeur sur un objet de type Str_t.
```
La sortie serait alors :
```ruby
Sortie 
Paramètres: [in] self Pointeur sur un objet de type Str_t.
```
* M. \return

Permet de décrire le retour d'une fonction ou d'une méthode.
```ruby
Déclaration 
\return { description of the return value }
Exemple 
\return Instance de l'objet, NULL en cas d'erreur.
```
* N. \bug

Permet de commencer un paragraphe décrivant un bug (ou plusieurs). L'utilisation est identique à la balise \author (voir le chapitre).
```ruby
Déclaration 
\bug { bug description }
Exemple 
\bug Problème d'affichage du texte en sortie
```
* O. \deprecated

Permet d'ajouter un paragraphe précisant que la fonction, marco, etc. est dépréciée, qu'il ne faut donc plus l'utiliser.
```ruby
Déclaration 
\deprecated { description }
Exemple 
\deprecated Fonction dépréciée, ne plus utiliser !
```
* P. \class

Permet de créer un bloc de documentation d'une classe (C++). L'utilisation est identique à la balise \struct (voir le chapitre).
```ruby
Déclaration 
\class <name> [<header-file>] [<header-name>]
Exemple 
\class Str
```
* Q. \namespace

Permet de créer un bloc de documentation pour un espace de nom (C++).
```ruby
Déclaration 
\namespace <name>
Exemple 
\namespace std
```
## III. Mise en place de la documentation
* A. Informations d'en-tête▲

Nous allons voir ici une manière de mettre un bloc d'informations d'en-tête d'un fichier avec les numéros de versions, auteurs, nom de fichiers, etc.

```ruby
/**
 * \file main.c
 * \brief Programme de tests.
 * \author Franck.H
 * \version 0.1
 * \date 11 septembre 2007
 *
 * Programme de test pour l'objet de gestion des chaines de caractères Str_t.
 *
 */
 ```
L'ordre des balises n'a que très peu d'importance, mais cela a un impact sur l'ordre de génération du paragraphe et donc de l'affichage. Ce qu'on peut remarquer de plus et qui n'a pas été abordé jusque-là, c'est qu'on peut ajouter des commentaires en dehors de la balise . Ceci sera en effet considéré par Doxygen comme une description détaillée et se trouvera alors dans un paragraphe intitulé Description détaillée.

On peut également voir que dans le champ date, la date peut prendre la forme que l'on souhaite. La balise sert surtout pour créer le paragraphe avec le bon titre.

* B. Documentation d'une fonction/méthode

Que ce soit pour une fonction ou une méthode de classes (C++), ce bloc ne change pas. Il faut noter qu'il doit y avoir une balise par argument. Tous les paramètres seront alors dans un même paragraphe Paramètres !
```ruby
Exemple pour une fonction en C 
/**
 * \fn static Str_t * str_new (const char * sz)
 * \brief Fonction de création d'une nouvelle instance d'un objet Str_t.
 *
 * \param sz Chaine à stocker dans l'objet Str_t, ne peut être NULL.
 * \return Instance nouvellement allouée d'un objet de type Str_t ou NULL.
 */
 ```
Remarquez que pour la balise , il faut fournir la déclaration complète de la fonction ou de la méthode !

Il ne faut surtout pas oublier de bien fournir la balise, car une vérification syntaxique de celle-ci sera faite par Doxygen et au moindre problème, il ne générera pas votre documentation !

* C. Documentation d'une structure/union

La documentation d'une structure et d'une union se fait de la même manière, nous allons donc voir ici que le cas d'une structure, ce qui sera retranscrit dans les exemples complets plus bas.
```ruby
/**
 * \struct Str_t
 * \brief Objet chaine de caractères.
 *
 * Str_t est un petit objet de gestion de chaines de caractères. 
 * La chaine se termine obligatoirement par un zéro de fin et l'objet 
 * connait la taille de chaine contient !
 */
 ```
Jusque-là rien de bien difficile, on peut simplement remarquer que nous n'avons ici, pas renseigné les champs optionnels de la balise. Une particularité qui n'a pas été abordée est que nous pouvons commenter les différents champs d'une structure, d'une union et même des variables membres d'une classe.

Voici comment procéder :
```ruby
typedef enum
{
   STR_NO_ERR,    /*!< Pas d'erreur. */
   STR_EMPTY_ERR, /*!< Erreur: Objet vide ou non initialisé. */
 
   NB_STR_ERR     /*!< Nombre total de constantes d'erreur. */
}
Str_err_e;
```
L'opérateur qu'il faut essentiellement retenir est « < » qui permet alors de documenter un membre, ici d'une structure, ce qui produit une génération de ce genre :
```ruby
Exemple de sortie 
Énumération:
    STR_NO_ERR      Pas d'erreur.
    STR_EMPTY_ERR   Erreur: Objet vide ou non initialisé.
    NB_STR_ERR      Nombre total de constantes d'erreur.
```
* D. Exemple complet en C

Voici un exemple de génération de la documentation de cet exemple : Exemple
```ruby 
/**
 * \file main.c
 * \brief Programme de tests.
 * \author Franck.H
 * \version 0.1
 * \date 6 septembre 2007
 *
 * Programme de test pour l'objet de gestion des chaines de caractères Str_t.
 *
 */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
 
/**
 * \struct Str_t
 * \brief Objet chaine de caractères.
 *
 * Str_t est un petit objet de gestion de chaines de caractères. 
 * La chaine se termine obligatoirement par un zéro de fin et l'objet 
 * connait la taille de chaine contient !
 */
typedef struct
{
   char * sz;  /*!< Chaine avec  caractère null de fin de chaine. */
   size_t len; /*!< Taille de la chaine sz sans compter le zéro de fin. */
}
Str_t;
 
 
/**
 * \enum Str_err_e
 * \brief Constantes d'erreurs.
 *
 * Str_err_e est une série de constantes prédéfinie pour diverses futures 
 * fonctions de l'objet Str_t.
 */
typedef enum
{
   STR_NO_ERR,    /*!< Pas d'erreur. */
   STR_EMPTY_ERR, /*!< Erreur: Objet vide ou non initialisé. */
 
   NB_STR_ERR     /*!< Nombre total de constantes d'erreur. */
}
Str_err_e;
 
 
/**
 * \fn static Str_err_e str_destroy (Str_t ** self)
 * \brief Fonction de destruction de l'objet Str_t.
 *
 * \param self Adresse de ll'objet Str_t à détruire.
 * \return STR_NO_ERR si aucune erreur, STR_EMPTY_ERR sinon.
 */
static Str_err_e str_destroy (Str_t ** self)
{
   Str_err_e err = STR_EMPTY_ERR;
 
   if (self != NULL && *self != NULL)
   {
      free (* self);
      *self = NULL;
 
      err = STR_NO_ERR;
   }
 
   return err;
}
 
 
/**
 * \fn static Str_t * str_new (const char * sz)
 * \brief Fonction de création d'une nouvelle instance d'un objet Str_t.
 *
 * \param sz Chaine à stocker dans l'objet Str_t, ne peut être NULL.
 * \return Instance nouvelle allouée d'un objet de type Str_t ou NULL.
 */
static Str_t * str_new (const char * sz)
{
   Str_t * self = NULL;
 
   if (sz != NULL && strlen (sz) > 0)
   {
      self = malloc (sizeof (* self));
 
      if (self != NULL)
      {
         self->len = strlen (sz);
         self->sz = malloc (self->len + 1);
 
         if (self->sz != NULL)
         {
            strcpy (self->sz, sz);
         }
         else
         {
            str_destroy (& self);
         }
      }
   }
 
   return self;
}
 
 
/**
 * \fn int main (void)
 * \brief Entrée du programme.
 *
 * \return EXIT_SUCCESS - Arrêt normal du programme.
 */
int main (void)
{
   Str_err_e err;
   Str_t * my_str = str_new ("Ma chaine de caracteres !");
 
   if (my_str != NULL)
   {
      printf ("%s\n", my_str->sz);
      printf ("Taille de la chaine : %d\n", my_str->len);
 
      err = str_destroy (& my_str);
 
      if (! err)
      {
         printf ("L'objet a ete libere correctement !\n");
      }
   }
 
   return EXIT_SUCCESS;
}
```
* E. Exemple complet en C++

Voici un exemple de génération de la documentation de cet exemple : Exemple

```ruby
#ifndef CPLAYER_H_
#define CPLAYER_H_
 
/*!
 * \file CPlayer.h
 * \brief Lecteur de musique de base
 * \author hiko-seijuro
 * \version 0.1
 */
#include <string>
#include <list>
 
/*! \namespace player
 * 
 * espace de nommage regroupant les outils composants 
 * un lecteur audio
 */
namespace player
{
  /*! \class CPlayer
   * \brief classe representant le lecteur
   *
   *  La classe gere la lecture d'une liste de morceaux
   */
  class CPlayer
  {
  private:
    std::list<string> m_listSongs; /*!< Liste des morceaux*/
    std::list<string>::iterator m_currentSong; /*!< Morceau courant */
 
 
  public:
    /*!
     *  \brief Constructeur
     *
     *  Constructeur de la classe CPlayer
     *
     *  \param listSongs : liste initiale des morceaux
     */
    CPlayer(std::list<string> listSongs);
 
    /*!
     *  \brief Destructeur
     *
     *  Destructeur de la classe CPlayer
     */
    virtual ~CPlayer();
 
  public:
    /*!
     *  \brief Ajout d'un morceau
     *
     *  Methode qui permet d'ajouter un morceau a liste de
     *  lecture
     *
     *  \param strSong : le morceau a ajouter
     *  \return true si morceau deja present dans la liste,
     *  false sinon
     */
    bool add(std::string strSong);
 
    /*!
     *  \brief Morceau suivant
     *
     *  Passage au morceau suivant
     */
    void next();
 
    /*!
     *  \brief Morceau precedent
     *
     *  Passage au morceau precedent
     */
    void previous();
 
    /*!
     *  \brief Lecture 
     *
     *  Lance la lecture de la liste
     */
    void play();
 
    /*!
     *  \brief Arret
     *
     *  Arrete la lecture
     */
    void stop();
  };
};
 
#endif
```
