---
title: Installation de AEM Forms sous Linux.
description: Installation de bibliothèques 32 bits pour AEM Forms en vue de son fonctionnement sur une installation Linux.
feature: Formulaires adaptatifs
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: développement
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: da7837d45a9d5f614a4f6527b7bfe98aaf980d4f
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---


# Installation de la version 32 bits des bibliothèques partagées

Lorsque AEM FORMS OSGi ou AEM Forms j2EE est déployé sur Linux, vous devez vous assurer que les versions 32 bits d’un ensemble de bibliothèques partagées sont installées et disponibles.  Les descriptions proviennent des paquets eux-mêmes.

* expat (bibliothèque C d’analyseur XML orienté flux pour l’analyse XML, écrite par James Clark)
* fontconfig (bibliothèque de configuration et de personnalisation des polices conçue pour localiser les polices dans le système et les sélectionner selon les exigences spécifiées par les applications)
* freetype (moteur de rendu des polices, développé pour fournir une prise en charge avancée des polices pour une variété de plateformes et d&#39;environnements. Il peut ouvrir et gérer les fichiers de polices, ainsi que charger, conseiller et générer efficacement des glyphes individuels. Il ne s’agit ni d’un serveur de polices ni d’une bibliothèque de rendu de texte complet)
* glibc (bibliothèques de base pour le système GNU et les systèmes GNU/Linux, ainsi que de nombreux autres systèmes qui utilisent Linux comme noyau)
* libcurl (bibliothèque de transfert d’URL côté client)
* libICE (bibliothèque Exchange entre clients)
* libicu (bibliothèque qui fournit une prise en charge complète et robuste d&#39;Unicode et de paramètres régionaux - Composants internationaux pour Unicode). Les éditions 64 bits et 32 bits de cette bibliothèque sont requises.
* libSM (bibliothèque X11 Session Management)
* libuuid (DCE bibliothèque d&#39;identificateurs universels uniques compatible - utilisée pour générer des identifiants uniques pour des objets qui peuvent être accessibles au-delà du système local)
* libX11 (bibliothèque côté client X11)
* libXau (X11 Authorization Protocol - utile pour limiter l&#39;accès du client à l&#39;affichage)
* libxcb (protocole X - liaison en langage C - XCB)
* libXext (Bibliothèque pour les extensions courantes du protocole X11)
* L&#39;extension libXinerama (X11 qui permet d&#39;étendre un bureau sur plusieurs écrans. Le nom est un jeu de mots sur Cinerama, un format de film grand écran qui utilisait plusieurs projecteurs. libXinerama est la bibliothèque qui interface avec l&#39;extension RandR)
* libXrandr (l&#39;extension Xinerama est aujourd&#39;hui largement obsolète - elle a été remplacée par l&#39;extension RandR)
* libXrender (bibliothèque cliente de l’extension de rendu X)
nss-softokn-freebl (bibliothèque gratuite pour les services de sécurité réseau)
* zlib (bibliothèque de compression de données à usage général, sans brevet, sans perte)

A partir de Red Hat Enterprise Linux 6, l&#39;édition 32 bits d&#39;une bibliothèque aura l&#39;extension de nom de fichier .686 tandis que l&#39;édition 64 bits aura .x86_64. Exemple, expat.i686. Avant RHEL 6, les éditions 32 bits avaient l&#39;extension .i386. Avant d&#39;installer les éditions 32 bits, assurez-vous que les dernières éditions 64 bits sont installées. Si l’édition 64 bits d’une bibliothèque est antérieure à la version 32 bits installée, vous obtenez une erreur du type suivant :

0mError : Versions multilib protégées : libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mErreur : Problèmes de version multilib détectés.]

## Première installation

Sur Red Hat Enterprise Linux, utilisez YellowDog Update Modificateur (YUM) pour installer, comme indiqué ci-dessous :

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

## Symliens

De plus, vous devez créer libcurl.so, libcrypto.so et libssl.so symlinks pointant vers les dernières versions 32 bits des bibliothèques libcurl, libcrypto et libssl, respectivement. Vous pouvez trouver les fichiers dans /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Mises à jour du système existant

il peut y avoir des conflits entre les architectures x86_64 et i686 lors des mises à jour, par exemple :
Erreur : Erreur de vérification de transaction :
fichier /lib/ld-2.28.so à partir de l&#39;installation de glibc-2.28-72.el8.i686 entre en conflit avec le fichier du package glibc32-2.28-42.1.el8.x86_64

Si vous rencontrez ce problème, désinstallez d’abord le pack concerné, comme dans ce cas :
yum remove glibc32-2.28-42.1.el8.x86_64

Cela dit, vous souhaitez que les versions x86_64 et i686 soient exactement les mêmes, comme par exemple depuis cette sortie vers la commande :
yum info glibc

Dernière vérification d’expiration des métadonnées : Il y a 0:41:33 le Sat 18 Jan 2020 11:37:08 AM EST.
Packages installés
Nom : glibc
Version : 2,28
Version : 72.el8
Architecture : i686
Taille : 13 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
De la repo : BaseOS
Résumé : Bibliothèques libc GNU
URL : http://www.gnu.org/software/glibc/
Licence : LGPLv2+ et LGPLv2+ avec exceptions et GPLv2+ et GPLv2+ avec exceptions et BSD et InnerNet et ISC et Public Domain et GFDL
Description : Le package glibc contient des bibliothèques standard utilisées par : plusieurs programmes sur le système. Pour économiser de l&#39;espace disque et : en plus de faciliter la mise à niveau, le code système commun est : gardé en un seul endroit et partagé entre les programmes. Ce paquet particulier : contient les ensembles les plus importants de bibliothèques partagées : la norme C : et la bibliothèque mathématique standard. Sans ces deux bibliothèques, a : Le système Linux ne fonctionnera pas.

Nom : glibc
Version : 2,28
Version : 72.el8
Architecture : x86_64
Taille : 15 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
De la repo : BaseOS
Résumé : Bibliothèques libc GNU
URL : http://www.gnu.org/software/glibc/
Licence : LGPLv2+ et LGPLv2+ avec exceptions et GPLv2+ et GPLv2+ avec exceptions et BSD et InnerNet et ISC et Public Domain et GFDL
Description : Le package glibc contient des bibliothèques standard utilisées par : plusieurs programmes sur le système. Pour économiser de l&#39;espace disque et : en plus de faciliter la mise à niveau, le code système commun est : gardé en un seul endroit et partagé entre les programmes. Ce paquet particulier : contient les ensembles les plus importants de bibliothèques partagées : la norme C : et la bibliothèque mathématique standard. Sans ces deux bibliothèques, a : Le système Linux ne fonctionnera pas.

## Quelques commandes de yum utiles

liste d&#39;aération installée
yum search [part_of_package_name]
yum whatfournit [nom_package]
yum install [nom_package]
yum reinstall [nom_package]
yum info [nom_package]
yum déplorist [nom_package]
yum remove [nom_package]
yum check-update [nom_package]
yum update [nom_package]
