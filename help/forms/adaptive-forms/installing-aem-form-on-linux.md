---
title: Installer AEM Forms sous Linux
description: Découvrez comment installer des bibliothèques 32 bits pour qu’AEM Forms fonctionne sur une installation Linux.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 264
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 100%

---

# Installer la version 32 bits des bibliothèques partagées

Lorsque AEM Forms OSGi ou AEM Forms j2EE est déployé sous Linux, vérifiez que les versions 32 bits d’un ensemble de bibliothèques partagées sont installées et disponibles.  Les descriptions proviennent des packages eux-mêmes.

* expat (bibliothèque C d’analyseur XML orientée flux pour l’analyse XML, écrite par James Clark).
* fontconfig (bibliothèque de configuration et de personnalisation des polices conçue pour localiser les polices dans le système et les sélectionner selon les exigences spécifiées par les applications).
* freetype (moteur de rendu des polices, développé pour offrir une prise en charge avancée des polices sur diverses plateformes et différents environnements. Il peut ouvrir et gérer des fichiers de polices, ainsi que charger et rendre efficacement des glyphes individuels et proposer des conseils à ce sujet. Il ne s’agit pas d’un serveur de polices ni d’une bibliothèque complète de rendu de texte).
* glibc (bibliothèques principale pour le système GNU et les systèmes GNU/Linux, ainsi que de nombreux autres systèmes qui utilisent Linux comme noyau).
* libcurl (bibliothèque de transfert d’URL côté client).
* libICE (bibliothèque d’échange inter-clients).
* libicu (bibliothèque fournissant une prise en charge robuste et complète d’Unicode et des paramètres régionaux – Composants internationaux pour Unicode). Les éditions 64 bits et 32 bits de cette bibliothèque sont requises.
* libSM (bibliothèque de gestion de session X11).
* libuuid (bibliothèque d’identifiants universels uniques, compatibles DCE, utilisée pour générer des identifiants uniques pour les objets qui peuvent être accessibles au-delà du système local).
* libX11 (bibliothèque X11 côté client).
* libXau (protocole d’autorisation X11, utile pour limiter l’accès du client à l’affichage).
* libxcb (liaison du langage C et protocole X - XCB).
* libXext (bibliothèque pour les extensions communes du protocole X11).
* libXinerama (extension X11 qui prend en charge l’extension d’un bureau sur plusieurs écrans. Le nom est un jeu de mots issu de Cinerama, un format de film sur grand écran qui utilisait plusieurs projecteurs. libXinerama est la bibliothèque qui s’interface avec l’extension RandR).
* libXrandr (l’extension Xinerama est généralement obsolète de nos jours, elle a été remplacée par l’extension RandR).
* libXrender (bibliothèque cliente d’extensions de rendu X).
nss-softokn-freebl (bibliothèque Freebl pour les services de sécurité réseau).
* zlib (bibliothèque de compression de données sans perte, sans brevet et d’usage général).

À partir de Red Hat Enterprise Linux 6, l’édition 32 bits d’une bibliothèque aura l’extension de nom de fichier .686, tandis que l’édition 64 bits aura l’extension .x86_64. Par exemple, expat.i686. Avant RHEL 6, les éditions 32 bits avaient l’extension .i386. Avant d’installer les éditions 32 bits, vérifiez que les dernières éditions 64 bits sont installées. Si l’édition 64 bits d’une bibliothèque est plus ancienne que la version 32 bits installée, un message d’erreur comme celui-ci s’affiche :

0mError : versions multilib protégées : libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError : problèmes de version Multilib détectés.]

## Première installation

Sous Red Hat Enterprise Linux, utilisez YellowDog Update Modifier (YUM) pour l’installation, comme illustré ci-dessous :

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

## Liens symboliques

En outre, vous devez créer les liens symboliques libcurl.so, libcrypto.so et libssl.so pointant vers les dernières versions 32 bits des bibliothèques libcurl, libcrypto et libssl, respectivement. Vous trouverez les fichiers dans /usr/lib/
dans -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
dans -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
dans -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so.

## Mises à jour du système existant

il peut y avoir des conflits entre les architectures x86_64 et i686 lors de mises à jour, par exemple :
Erreur : erreur de vérification de transaction :
le fichier /lib/ld-2.28.so à partir de l’installation de glibc-2.28-72.el8.i686 est en conflit avec le fichier du package glibc32-2.28-42.1.el8.x86_64.

Si vous rencontrez ce problème, désinstallez d’abord le package en cause, dans le cas présent :
yum remove glibc32-2.28-42.1.el8.x86_64.

En définitive, vous souhaitez que les versions x86_64 et i686 correspondent, à l’instar de la sortie de la commande suivante, par exemple :
yum info glibc.

Dernière vérification d’expiration des métadonnées : Il y a 0:41:33, Sam 18 Jan 2020 à 11:37:08 AM EST.
Packages installés
Nom : glibc
Version : 2.28
Build : 72.el8
Architecture : i686
Taille : 13 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
À partir du référentiel : BaseOS
Résumé : les bibliothèques GNU libc
URL : hhttp://www.gnu.org/software/glibc/
Licence : LGPLv2 et ultérieures et LGPLv2 et ultérieures avec exceptions, GPLv2 et ultérieures, GPLv2 et ultérieures avec exceptions et BSD, Inner-Net, ISC, le domaine public et GFDL
Description : le package glibc contient des bibliothèques standard utilisées par plusieurs programmes sur le système. Pour économiser de l’espace disque et de la mémoire, ainsi que pour faciliter la mise à niveau, le code système commun est conservé à un emplacement unique et partagé entre les programmes. Ce package particulier contient les bibliothèques partagées essentielles : la bibliothèque C standard et la bibliothèque mathématique standard. Sans ces deux bibliothèques, le système Linux ne fonctionne pas.

Name : glibc
Version : 2.28
Build : 72.el8
Architecture : x86_64
Tzille : 15 M
Source : glibc-2.28-72.el8.src.rpm
Référentiel : @System
À partir du référentiel : BaseOS
Résumé : les bibliothèques GNU libc
URL : http://www.gnu.org/software/glibc/
License : LGPLv2 et ultérieures et LGPLv2 et ultérieures avec exceptions, GPLv2 et ultérieures et GPLv2 et ultérieures avec exceptions, BSD, Inner-Net, ISC, le domaine public et GFDL
Description : le package glibc contient des bibliothèques standard utilisées par plusieurs programmes sur le système. Pour économiser de l’espace disque et de la mémoire, ainsi que pour faciliter la mise à niveau, le code système commun est conservé à un emplacement unique et partagé entre les programmes. Ce package particulier contient les bibliothèques partagées essentielles : la bibliothèque C standard et la bibliothèque mathématique standard. Sans ces deux bibliothèques, le système Linux ne fonctionne pas.

## Certaines commandes yum utiles

yum list installed
yum search [part_of_package_name]
yum whatprovides [package_name]
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
