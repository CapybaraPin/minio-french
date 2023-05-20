# Guide de démarrage MinIO

[![Slack](https://slack.min.io/slack?type=svg)](https://slack.min.io) [![Docker Pulls](https://img.shields.io/docker/pulls/minio/minio.svg?maxAge=604800)](https://hub.docker.com/r/minio/minio/) [![license](https://img.shields.io/badge/license-AGPL%20V3-blue)](https://github.com/minio/minio/blob/master/LICENSE)

[![MinIO](https://raw.githubusercontent.com/minio/minio/master/.github/logo.svg?sanitize=true)](https://min.io)

MinIO est un système stockage d'objets hautes performances publié sous la licence publique générale GNU Affero v3.0. Il est compatible API avec le service de stockage en nuage Amazon S3. Utilisez MinIO pour créer une infrastructure hautes performances pour les charges de travail d'apprentissage automatique, d'analyse et de données d'application.

Ce fichier README fournit des instructions de démarrage rapide sur l'exécution de MinIO sur du matériel nu, y compris les installations basées sur des conteneurs. Pour les environnements Kubernetes, utilisez [l'opérateur MinIO Kubernetes](https://github.com/minio/operator/blob/master/README.md).

## Installation du conteneur

Utilisez les commandes suivantes pour exécuter un serveur MinIO autonome en tant que conteneur. 

Les serveurs MinIO autonomes sont les mieux adaptés au développement et à l'évaluation précoces. Certaines fonctionnalités telles que la gestion des versions, le verrouillage d'objet et la réplication de bucket nécessitent le déploiement distribué de MinIO avec Erasure Coding. Pour un développement et une production étendus, déployez MinIO avec le codage d'effacement activé - en particulier, avec un *minimum* de 4 disques par serveur MinIO. Voir [Présentation du code d'effacement MinIO](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html)
pour une documentation plus complète.

### Stable

Exécutez la commande suivante pour exécuter la dernière image stable de MinIO en tant que conteneur à l'aide d'un volume de données éphémère : 

```sh
podman run -p 9000:9000 -p 9001:9001 \
  quay.io/minio/minio server /data --console-address ":9001"
```

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un outil intégré navigateur d'objets intégré au serveur MinIO. Sur votre navigateur Web exécuté sur la machine hôte l'adresse <http://127.0.0.1:9000> et connectez-vous avec les identifiants racine. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir 
[Test d'utilisation MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html> pour afficher les SDK MinIO pour les langues prises en charge.

> REMARQUE : Pour déployer MinIO avec un stockage persistant, vous devez mapper les répertoires persistants locaux du système d'exploitation hôte vers le conteneur à l'aide de l'option "podman -v". Par exemple, `-v /mnt/data:/data` mappe le lecteur du système d'exploitation hôte à `/mnt/data` à `/data` sur le conteneur. 

## macOS

Utilisez les commandes suivantes pour exécuter un serveur MinIO autonome sur macOS. 

Les serveurs MinIO autonomes sont les mieux adaptés au développement et à l'évaluation précoces. Certaines fonctionnalités telles que la gestion des versions, le verrouillage d'objet et la réplication de compartiment nécessitent un déploiement distribué de MinIO avec codage d'effacement. Pour un développement et une production étendus, déployez MinIO avec le codage d'effacement activé - en particulier, avec un *minimum* de 4 disques par serveur MinIO. Voir [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html) pour une documentation plus complète.

### Homebrew (recommended)

Exécutez la commande suivante pour installer le dernier package MinIO stable à l'aide de [Homebrew] (https://brew.sh/). Remplacez ``/data`` par le chemin d'accès au lecteur ou au répertoire dans lequel vous souhaitez que MinIO stocke les données.

```sh
brew install minio/stable/minio
minio server /data
```

> REMARQUE : Si vous avez précédemment installé minio à l'aide de `brew install minio`, il est recommandé de réinstaller minio à partir du dépôt officiel `minio/stable/minio` à la place.

```sh
brew uninstall minio
brew install minio/stable/minio
```

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un navigateur d'objets Web intégré intégré au serveur MinIO. Pointez un navigateur Web exécuté sur la machine hôte vers <http://127.0.0.1:9000> et connectez-vous avec les informations d'identification root. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir [Test using MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html/> pour afficher les SDK MinIO pour les langues prises en charge.

### Téléchargement binaire

Utilisez la commande suivante pour télécharger et exécuter un serveur MinIO autonome sur macOS. Remplacez ``/data`` par le chemin d'accès au lecteur ou au répertoire dans lequel vous souhaitez que MinIO stocke les données.

```sh
wget https://dl.min.io/server/minio/release/darwin-amd64/minio
chmod +x minio
./minio server /data
```

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un navigateur d'objets Web intégré intégré au serveur MinIO. Pointez un navigateur Web exécuté sur la machine hôte vers <http://127.0.0.1:9000> et connectez-vous avec les informations d'identification root. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir [Test using MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html> pour afficher les SDK MinIO pour les langues prises en charge.

## GNU/Linux

Utilisez la commande suivante pour exécuter un serveur MinIO autonome sur des hôtes Linux exécutant des architectures Intel/AMD 64 bits. Remplacez ``/data`` par le chemin d'accès au lecteur ou au répertoire dans lequel vous souhaitez que MinIO stocke les données.

```sh
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
./minio server /data
```

Le tableau suivant répertorie les architectures prises en charge. Remplacez l'URL `wget` par l'architecture de votre hôte Linux.

| Architecture                   | URL                                                        |
| --------                       | ------                                                     |
| 64-bit Intel/AMD               | <https://dl.min.io/server/minio/release/linux-amd64/minio>   |
| 64-bit ARM                     | <https://dl.min.io/server/minio/release/linux-arm64/minio>   |
| 64-bit PowerPC LE (ppc64le)    | <https://dl.min.io/server/minio/release/linux-ppc64le/minio> |
| IBM Z-Series (S390X)           | <https://dl.min.io/server/minio/release/linux-s390x/minio>   |

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un navigateur d'objets Web intégré intégré au serveur MinIO. Pointez un navigateur Web exécuté sur la machine hôte vers <http://127.0.0.1:9000> et connectez-vous avec les informations d'identification root. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir [Test using MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html> pour afficher les SDK MinIO pour les langues prises en charge.

> REMARQUE : Les serveurs MinIO autonomes sont les mieux adaptés au développement et à l'évaluation précoces. Certaines fonctionnalités telles que la gestion des versions, le verrouillage d'objet et la réplication de compartiment nécessitent un déploiement distribué de MinIO avec codage d'effacement. Pour un développement et une production étendus, déployez MinIO avec le codage d'effacement activé - en particulier, avec un *minimum* de 4 disques par serveur MinIO. Voir [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html#) pour une documentation plus complète.

## Microsoft Windows

Pour exécuter MinIO sur des hôtes Windows 64 bits, téléchargez l'exécutable MinIO à partir de l'URL suivante :

```sh
https://dl.min.io/server/minio/release/windows-amd64/minio.exe
```

Utilisez la commande suivante pour exécuter un serveur MinIO autonome sur l'hôte Windows. Remplacez ``D:\`` par le chemin d'accès au lecteur ou au répertoire dans lequel vous souhaitez que MinIO stocke les données. Vous devez changer le répertoire du terminal ou du powershell à l'emplacement de l'exécutable ``minio.exe``, *ou* ajouter le chemin de ce répertoire au système ``$PATH`` :

```sh
minio.exe server D:\
```

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un navigateur d'objets Web intégré intégré au serveur MinIO. Pointez un navigateur Web exécuté sur la machine hôte vers <http://127.0.0.1:9000> et connectez-vous avec les informations d'identification root. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir [Test using MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html> pour afficher les SDK MinIO pour les langues prises en charge.

> REMARQUE : Les serveurs MinIO autonomes sont les mieux adaptés au développement et à l'évaluation précoces. Certaines fonctionnalités telles que la gestion des versions, le verrouillage d'objet et la réplication de compartiment nécessitent un déploiement distribué de MinIO avec codage d'effacement. Pour un développement et une production étendus, déployez MinIO avec le codage d'effacement activé - en particulier, avec un *minimum* de 4 disques par serveur MinIO. Voir [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html#) pour une documentation plus complète.


## Installer à partir de la source

Utilisez les commandes suivantes pour compiler et exécuter un serveur MinIO autonome à partir de la source. L'installation des sources est uniquement destinée aux développeurs et aux utilisateurs avancés. Si vous ne disposez pas d'un environnement Golang opérationnel, veuillez suivre [Comment installer Golang](https://golang.org/doc/install). La version minimale requise est [go1.19](https://golang.org/dl/#stable)

```sh
go install github.com/minio/minio@latest
```

Le déploiement MinIO démarre en utilisant les informations d'identification racine par défaut `minioadmin:minioadmin`. Vous pouvez tester le déploiement à l'aide de la console MinIO, un navigateur d'objets Web intégré intégré au serveur MinIO. Pointez un navigateur Web exécuté sur la machine hôte vers <http://127.0.0.1:9000> et connectez-vous avec les informations d'identification root. Vous pouvez utiliser le navigateur pour créer des compartiments, télécharger des objets et parcourir le contenu du serveur MinIO.

Vous pouvez également vous connecter à l'aide de n'importe quel outil compatible S3, tel que l'outil de ligne de commande "mc" du client MinIO. Voir [Test using MinIO Client `mc`](#test-using-minio-client-mc) pour plus d'informations sur l'utilisation de l'outil de ligne de commande `mc`. Pour les développeurs d'applications, voir <https://min.io/docs/minio/linux/developers/minio-drivers.html> pour afficher les SDK MinIO pour les langues prises en charge.

> REMARQUE : Les serveurs MinIO autonomes sont les mieux adaptés au développement et à l'évaluation précoces. Certaines fonctionnalités telles que la gestion des versions, le verrouillage d'objet et la réplication de compartiment nécessitent un déploiement distribué de MinIO avec codage d'effacement. Pour un développement et une production étendus, déployez MinIO avec le codage d'effacement activé - en particulier, avec un *minimum* de 4 disques par serveur MinIO. Voir [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html) pour une documentation plus complète.

MinIO recommande fortement * contre * l'utilisation de serveurs MinIO compilés à partir de la source pour les environnements de production.

## Recommandations de déploiement 

### Autoriser l'accès au port pour les pare-feu

Par défaut, MinIO utilise le port 9000 pour écouter les connexions entrantes. Si votre plate-forme bloque le port par défaut, vous devrez peut-être activer l'accès au port.

### ufw

Pour les hôtes avec ufw activé (distributions basées sur Debian), vous pouvez utiliser la commande `ufw` pour autoriser le trafic vers des ports spécifiques. Utilisez la commande ci-dessous pour autoriser l'accès au port 9000.

```sh
ufw allow 9000
```

La commande ci-dessous active tout le trafic entrant vers les ports allant de 9000 à 9010.

```sh
ufw allow 9000:9010/tcp
```

### firewall-cmd

Pour les hôtes avec firewall-cmd activé (CentOS), vous pouvez utiliser la commande `firewall-cmd` pour autoriser le trafic vers des ports spécifiques. Utilisez les commandes ci-dessous pour autoriser l'accès au port 9000.

```sh
firewall-cmd --get-active-zones
```

Cette commande obtient la ou les zones actives. Maintenant, appliquez les règles de port aux zones pertinentes renvoyées ci-dessus. Par exemple, si la zone est "publique", utilisez.

```sh
firewall-cmd --zone=public --add-port=9000/tcp --permanent
```

Notez que "permanent" garantit que les règles sont persistantes lors du démarrage, du redémarrage ou du rechargement du pare-feu. Enfin, rechargez le pare-feu pour que les modifications prennent effet.

```sh
firewall-cmd --reload
```

### iptables

Pour les hôtes avec iptables activé (RHEL, CentOS, etc.), vous pouvez utiliser la commande `iptables` pour activer tout le trafic provenant de ports spécifiques. Utilisez la commande ci-dessous pour autoriser l'accès au port 9000.

```sh
iptables -A INPUT -p tcp --dport 9000 -j ACCEPT
service iptables restart
```

La commande ci-dessous active tout le trafic entrant vers les ports allant de 9000 à 9010.

```sh
iptables -A INPUT -p tcp --dport 9000:9010 -j ACCEPT
service iptables restart
```

## Testez la connectivité MinIO

### Test à l'aide de la console MinIO

MinIO Server est livré avec un navigateur d'objets Web intégré. Pointez votre navigateur Web sur <http://127.0.0.1:9000> pour vous assurer que votre serveur a démarré avec succès.

> REMARQUE : MinIO exécute la console sur un port aléatoire par défaut si vous souhaitez choisir un port spécifique, utilisez `--console-address` pour choisir une interface et un port spécifiques.

### Choses à considérer ⚠️

MinIO redirige les demandes d'accès du navigateur vers le port de serveur configuré (c'est-à-dire `127.0.0.1:9000`) vers le port de console configuré. MinIO utilise le nom d'hôte ou l'adresse IP spécifié dans la requête lors de la création de l'URL de redirection. L'URL et le port *doivent* être accessibles par le client pour que la redirection fonctionne.

Pour les déploiements derrière un équilibreur de charge, un proxy ou une règle d'entrée où l'adresse IP ou le port de l'hôte MinIO n'est pas public, utilisez la variable d'environnement `MINIO_BROWSER_REDIRECT_URL` pour spécifier le nom d'hôte externe pour la redirection. Le LB/Proxy doit avoir des règles pour diriger spécifiquement le trafic vers le port de la console.

Par exemple, considérez un déploiement MinIO derrière un proxy `https://minio.example.net`, `https://console.minio.example.net` avec des règles pour transférer le trafic sur les ports :9000 et :9001 vers MinIO et la console MinIO respectivement sur le réseau interne. Définissez `MINIO_BROWSER_REDIRECT_URL` sur `https://console.minio.example.net` pour vous assurer que le navigateur reçoit une URL accessible valide.

De même, si vos certificats TLS n'ont pas le SAN IP pour l'hôte du serveur MinIO, la console MinIO peut ne pas valider la connexion au serveur. Utilisez la variable d'environnement `MINIO_SERVER_URL` et spécifiez le nom d'hôte accessible par proxy du serveur MinIO pour permettre à la console d'utiliser l'API du serveur MinIO à l'aide du certificat TLS.

Par exemple : `export MINIO_SERVER_URL="https://minio.example.net"`

| Dashboard                                                                                   | Création d'un bucket                                                                       |
| -------------                                                                               | -------------                                                                               |
| ![Dashboard](https://github.com/minio/minio/blob/master/docs/screenshots/pic1.png?raw=true) | ![Dashboard](https://github.com/minio/minio/blob/master/docs/screenshots/pic2.png?raw=true) |

## Test avec le client MinIO `mc`

`mc` fournit une alternative moderne aux commandes UNIX telles que ls, cat, cp, mirror, diff, etc. Il prend en charge les systèmes de fichiers et les services de stockage en nuage compatibles avec Amazon S3. Suivez le MinIO Client [Quickstart Guide](https://min.io/docs/minio/linux/reference/minio-mc.html#quickstart) pour plus d'instructions.

## Mise à jour MinIO

Les mises à niveau ne nécessitent aucun temps d'arrêt dans MinIO, toutes les mises à niveau sont sans interruption, toutes les transactions sur MinIO sont atomiques. La mise à niveau simultanée de tous les serveurs est donc la méthode recommandée pour mettre à niveau MinIO.

> REMARQUE : nécessite un accès Internet pour mettre à jour directement depuis <https://dl.min.io>, vous pouvez éventuellement héberger n'importe quel miroir sur <https://my-artifactory.example.com/minio/>

- Pour les déploiements qui ont installé manuellement le binaire du serveur MinIO, utilisez [`mc admin update`](https://min.io/docs/minio/linux/reference/minio-mc-admin/mc-admin-update.html )

```sh
mc admin update <minio alias, e.g., myminio>
```

- Pour les déploiements sans accès Internet externe (par exemple, les environnements isolés), téléchargez le binaire depuis <https://dl.min.io> et remplacez le binaire MinIO existant, disons par exemple `/opt/bin/minio`, appliquez les autorisations exécutables `chmod +x /opt/bin/minio` et procédez à l'exécution de `mc admin service restart alias/`.

- Pour les installations utilisant le service Systemd MinIO, mettez à niveau via les packages RPM/DEB **parallèlement** sur tous les serveurs ou remplacez le binaire disons `/opt/bin/minio` sur tous les nœuds, appliquez les autorisations exécutables `chmod +x /opt/ bin/minio' et processus pour effectuer 'mc admin service restart alias/'.

### Liste de contrôle de mise à niveau

- Testez toutes les mises à niveau dans un environnement inférieur (DEV, QA, UAT) avant de les appliquer à la production. L'exécution de mises à niveau aveugles dans des environnements de production comporte des risques importants.
- Lisez les notes de version de MinIO * avant * d'effectuer une mise à niveau, il n'y a aucune exigence forcée de mettre à niveau vers les dernières versions à chaque version. Certaines versions peuvent ne pas être pertinentes pour votre configuration, évitez de mettre à niveau inutilement les environnements de production.
- Si vous prévoyez d'utiliser `mc admin update`, le processus MinIO doit avoir un accès en écriture au répertoire parent où le binaire est présent sur le système hôte.
- `mc admin update` n'est pas pris en charge et doit être évité dans les environnements kubernetes/conteneurs, veuillez mettre à niveau les conteneurs en mettant à niveau les images de conteneur pertinentes.
- **Nous ne recommandons pas de mettre à niveau un serveur MinIO à la fois, le produit est conçu pour prendre en charge les mises à niveau parallèles, veuillez suivre nos directives recommandées.**

## Explorez plus

- [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html)
- [Use `mc` with MinIO Server](https://min.io/docs/minio/linux/reference/minio-mc.html)
- [Use `minio-go` SDK with MinIO Server](https://min.io/docs/minio/linux/developers/go/minio-go.html)
- [The MinIO documentation website](https://min.io/docs/minio/linux/index.html)

## Contribuer au projet MinIO

Merci de suivre MinIO [Contributor's Guide](https://github.com/minio/minio/blob/master/CONTRIBUTING.md)

## License

- MinIO source is licensed under the GNU AGPLv3 license that can be found in the [LICENSE](https://github.com/minio/minio/blob/master/LICENSE) file.
- MinIO [Documentation](https://github.com/minio/minio/tree/master/docs) © 2021 by MinIO, Inc is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- [License Compliance](https://github.com/minio/minio/blob/master/COMPLIANCE.md)
