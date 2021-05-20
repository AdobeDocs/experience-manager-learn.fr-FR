---
title: Installation d’AEM Forms sous Linux
description: Découvrez comment installer des bibliothèques 32 bits pour qu’AEM Forms fonctionne sur une installation Linux.
feature: Formulaires adaptatifs
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: développement
role: Developer
level: Beginner
kt: 7593
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# Installation de la version 32 bits des bibliothèques partagées

Lorsque AEM FORMS OSGi ou AEM Forms j2EE est déployé sous Linux, vous devez vous assurer que les versions 32 bits d’un ensemble de bibliothèques partagées sont installées et disponibles.  Les descriptions proviennent des packages eux-mêmes.

* expat (bibliothèque C d’analyseur XML orienté flux pour l’analyse XML, écrite par James Clark)
* fontconfig (bibliothèque de configuration et de personnalisation des polices conçue pour localiser les polices dans le système et les sélectionner selon les exigences spécifiées par les applications)
* freetype (moteur de rendu des polices), développé pour offrir une prise en charge avancée des polices sur diverses plateformes et environnements. Il peut ouvrir et gérer des fichiers de polices, ainsi que charger, conseiller et générer efficacement des glyphes individuels. Il ne s’agit pas d’un serveur de polices ou d’une bibliothèque complète de rendu de texte)
* glibc (bibliothèques Core pour le système GNU et les systèmes GNU/Linux, ainsi que de nombreux autres systèmes qui utilisent Linux comme noyau)
* libcurl (bibliothèque de transfert d’URL côté client)
* libICE (Bibliothèque d’échange inter-clients)
* libicu (Bibliothèque fournissant une prise en charge Unicode et locale robuste et complète - Composants internationaux pour Unicode). Les éditions 64 bits et 32 bits de cette bibliothèque sont requises.
* libSM (bibliothèque de gestion de session X11)
* libuuid (bibliothèque d’identifiants uniques compatibles avec le DCE - utilisée pour générer des identifiants uniques pour les objets qui peuvent être accessibles au-delà du système local)
* libX11 (bibliothèque côté client X11)
* libXau (X11 Authorization Protocol, utile pour limiter l’accès du client à l’affichage)
* libxcb (protocole X - liaison de langue C - XCB)
* libXext (bibliothèque pour les extensions courantes du protocole X11)
* libXinerama (extension X11) qui permet d’étendre un bureau sur plusieurs écrans. Le nom est un jeu de mots sur Cinerama, un format de film grand écran qui utilisait plusieurs projecteurs. libXinerama est la bibliothèque qui utilise l’extension RandR)
* libXrandr (l&#39;extension Xinerama est largement obsolète de nos jours - elle a été remplacée par l&#39;extension RandR)
* libXrender (bibliothèque cliente de l’extension de rendu X)
nss-softokn-freebl (Bibliothèque libre pour les services de sécurité réseau)
* zlib (bibliothèque de compression de données d’usage général, sans brevet, sans perte)

À partir de Red Hat Enterprise Linux 6, l’édition 32 bits d’une bibliothèque aura l’extension .686, tandis que l’édition 64 bits aura .x86_64. Par exemple, expat.i686. Avant RHEL 6, les éditions 32 bits avaient l’extension .i386. Avant d’installer les éditions 32 bits, vérifiez que les dernières éditions 64 bits sont installées. Si l’édition 64 bits d’une bibliothèque est antérieure à la version 32 bits installée, vous obtenez une erreur comme ci-dessous :

0mError : Versions multilib protégées : libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError : Problèmes de version multi-lib détectés.]

## Première installation

Sous Red Hat Enterprise Linux, utilisez le YellowDog Update Modifier (YUM) pour installer, comme illustré ci-dessous :

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

En outre, vous devez créer les liens symboliques libcurl.so, libcrypto.so et libssl.so pointant vers les dernières versions 32 bits des bibliothèques libcurl, libcrypto et libssl, respectivement. Vous trouverez les fichiers dans /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Mises à jour du système existant

il peut y avoir des conflits entre les architectures x86_64 et i686 lors de mises à jour, par exemple :
Erreur : Erreur de vérification de transaction :
fichier /lib/ld-2.28.so à partir de l’installation de glibc-2.28-72.el8.i686 provoque des conflits avec le fichier du package glibc32-2.28-42.1.el8.x86_64

Si vous rencontrez ce problème, désinstallez d’abord le package incriminé, comme dans ce cas :
yum remove glibc32-2.28-42.1.el8.x86_64

Cela dit, vous souhaitez que les versions x86_64 et i686 soient exactement les mêmes, comme par exemple depuis cette sortie vers la commande :
yum info glibc

Dernière vérification d’expiration des métadonnées : 0:41:33 il y a 18 janvier 2020 11:37:08 AM EST.
Packages installés
Nom : glibc
Version : 2,28
Version : 72.el8
Architecture : i686
Taille : 13 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
Depuis le référentiel : BaseOS
Résumé : Bibliothèques libc GNU
URL : http://www.gnu.org/software/glibc/
Licence : LGPLv2+ et LGPLv2+ avec exceptions et GPLv2+ et GPLv2+ avec exceptions et BSD et Inner-Net et ISC et Public Domain et GFDL
Description : Le package glibc contient des bibliothèques standard utilisées par : plusieurs programmes sur le système. Pour économiser de l’espace disque et : mémoire, ainsi que pour faciliter la mise à niveau, le code système courant est : Ils sont gardés à un seul endroit et partagés entre les programmes. Ce package particulier : contient les ensembles les plus importants de bibliothèques partagées : la norme C : et la bibliothèque mathématique standard. Sans ces deux bibliothèques, une : Le système Linux ne fonctionnera pas.

Nom : glibc
Version : 2,28
Version : 72.el8
Architecture : x86_64
Taille : 15 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
Depuis le référentiel : BaseOS
Résumé : Bibliothèques libc GNU
URL : http://www.gnu.org/software/glibc/
Licence : LGPLv2+ et LGPLv2+ avec exceptions et GPLv2+ et GPLv2+ avec exceptions et BSD et Inner-Net et ISC et Public Domain et GFDL
Description : Le package glibc contient des bibliothèques standard utilisées par : plusieurs programmes sur le système. Pour économiser de l’espace disque et : mémoire, ainsi que pour faciliter la mise à niveau, le code système courant est : Ils sont gardés à un seul endroit et partagés entre les programmes. Ce package particulier : contient les ensembles les plus importants de bibliothèques partagées : la norme C : et la bibliothèque mathématique standard. Sans ces deux bibliothèques, une : Le système Linux ne fonctionnera pas.

## Certaines commandes yum utiles

yum list installée
yum search [part_of_package_name]
yum what fournit [nom_module]
yum install [nom_module]
yum reinstall [nom_module]
yum info [nom_module]
yum s’épuise [nom_module]
yum remove [nom_module]
yum check-update [nom_module]
yum update [nom_module]
