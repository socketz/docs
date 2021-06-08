---
title: 'Éditer une zone DNS OVHcloud'
slug: editer-ma-zone-dns
excerpt: 'Découvrez comment éditer une zone DNS OVHcloud via votre espace client'
section: 'DNS et zone DNS'
order: 3
---

**Dernière mise à jour le 16/02/2021**

## Objectif

### Comprendre la notion de DNS <a name="understanddns"></a>

Le sigle DNS, signifiant **D**omain **N**ame **S**ystem, est un ensemble d'éléments permettant de faire correspondre un nom de domaine avec une adresse IP.

Par exemple, lorsque vous souhaitez accéder au site *mydomain.ovh*, votre requête est initialement traitée par cet ensemble DNS qui va l'aiguiller vers l'adresse IP du serveur hébergeant le site *mydomain.ovh*.

Au vu des manipulations que vous serez amenés à effectuer dans l'espace client, il est important de différencier les **serveurs DNS** et la **zone DNS**. En effet, c'est au niveau du **serveur DNS** qu'est configurée la **zone DNS**. 

Vous trouvez les informations relatives aux **serveurs DNS** et leur modification sur notre guide « [Modifier les serveurs DNS d’un nom de domaine OVHcloud](../generalites-serveurs-dns/) » .

![DNS](images/dnsserver.png){.thumbnail}

Si nous reprenons l'exemple plus haut, lorsque vous tapez *mydomain.ovh*, les **serveurs DNS** associés à ce nom de domaine seront interrogés. Ces derniers contiennent la **zone DNS** du nom de domaine *mydomain.ovh* où est renseignée l'adresse IP de l'hébergement de *mydomain.ovh*. Ainsi votre navigateur est en mesure d'afficher le site internet *mydomain.ovh* contenu sur l'hébergement. On appelle cela une résolution DNS.

![DNS](images/dnssolve.gif){.thumbnail}

### La zone DNS 

La zone DNS d'un nom de domaine est un fichier de configuration composé d'**enregistrements**. Ces derniers permettent de faire le lien entre votre nom de domaine et les serveurs qui hébergent vos services internet, tels que des sites web (via l'enregistrement A) ou des adresses e-mail (enregistrement MX).

![DNS](images/dnszone.png){.thumbnail}

**Découvrez comment éditer votre zone DNS OVHcloud via votre espace client.**

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/BvrUi26ShzI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Prérequis

- Disposer d'un accès à la gestion du nom de domaine concerné depuis votre [espace client OVHcloud](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/fr/&ovhSubsidiary=fr){.external}.
- Être connecté à votre [espace client OVHcloud](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/fr/&ovhSubsidiary=fr){.external}.
- Utiliser la configuration OVHcloud (ses serveurs DNS) pour le nom de domaine concerné. 

> [!warning]
>
> - Si votre nom de domaine n'utilise pas les serveurs DNS d'OVHcloud, vous devez réaliser la modification depuis l'interface du prestataire gérant la configuration de votre nom de domaine.
> 
> - Si votre nom de domaine est enregistré chez OVHcloud, vous pouvez vérifier si ce dernier utilise notre configuration. Pour cela, rendez-vous dans votre [espace client](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/fr/&ovhSubsidiary=fr){.external}, dans l'onglet `Serveurs DNS`{.action} du nom de domaine concerné.
>

## En pratique

### Accéder à la gestion d'une zone DNS OVHcloud

Connectez-vous à votre [espace client OVHcloud](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/fr/&ovhSubsidiary=fr){.external} dans la section `Web Cloud`{.action}. Cliquez sur `Noms de domaine`{.action} dans la barre de services à gauche, puis choisissez le nom de domaine concerné. Positionnez-vous enfin sur l'onglet `Zone DNS`{.action}.

Le tableau qui apparaît affiche pour chaque ligne un enregistrement DNS lié à votre nom de domaine chez OVHCloud. Vous avez la possibilité d'en filtrer le contenu par type d'enregistrement ou par domaine.

![dnszone](images/edit-dns-zone-ovh-control-panel.png){.thumbnail}

### Les enregistrements DNS

**L'édition d'une zone DNS est une manipulation sensible** : réaliser un changement inopportun pourrait, par exemple, rendre impossible l'accès à votre site internet ou la réception de nouveaux messages sur vos adresses e-mail.

Comprendre ces différents enregistrements vous permettra de mieux appréhender les changements que vous allez effectuer si vous éditez la zone DNS de votre nom de domaine. Nous vous invitons vivement à consulter la liste ci-dessous. Elle reprend les objectifs et spécificités de chaque enregistrement.

#### Enregistrements de pointage

**A** : Relie un nom de domaine à une adresse IPv4. Par exemple, l'adresse IPv4 du serveur où est hébergé votre site internet.

**AAAA** : Relie un nom de domaine à une adresse IPv6. Par exemple, l'adresse IPv6 du serveur où est hébergé votre site internet.

**CNAME** : Utilise l'adresse IP d'un autre nom de domaine en créant un lien appelé alias. Par exemple, si *www.mydomain.ovh* est un alias de *mydomain.ovh*, cela indique que *www.mydomain.ovh* utilisera l'adresse IP de *mydomain.ovh*.

> [!alert]
>
> Un enregistrement TXT utilisant le même domaine ou sous-domaine qu'un enregistrement CNAME perturbe le fonctionnement de ce dernier. Votre enregistrement CNAME ne fonctionnera alors que partiellement ou pas du tout.
>

**Champ NS** : Définit les serveurs DNS associés à votre zone DNS. Par exemple, si les enregistrements NS de votre zone DNS affichent les serveurs *dns19.ovh.net* et *ns19.ovh.net*, vous devrez alors utiliser ces derniers dans l'onglet `Serveurs DNS`{.action} de votre espace client. Consultez notre documentation « [Modifier les serveurs DNS d’un nom de domaine OVHcloud](../generalites-serveurs-dns/) » pour plus d'informations.

> [!warning]
>
> Ne modifiez pas, via le bouton `Modifier en mode textuel`{.action}, les enregistrements NS de votre zone DNS au profit de serveurs DNS externes à OVHcloud. En effet, cette zone DNS fonctionne **uniquement** avec des serveurs DNS OVHcloud.
>

#### Enregistrements e-mail

**MX** : Relie un nom de domaine à un serveur e-mail. Par exemple, l'adresse *10 mx1.mail.ovh.net* correspond à l'un des serveurs e-mail OVHcloud lorsque vous possédez une offre e-mail OVHcloud. Il est probable que votre fournisseur e-mail dispose de plusieurs serveurs e-mail : plusieurs champs MX doivent donc être créés. Consultez notre documentation « [Ajouter un champ MX à la configuration de son nom de domaine](../mail-mutualise-guide-de-configuration-mx-avec-zone-dns-ovh/) ».

**SPF** : Permet d'éviter les potentielles usurpations d’identité sur les adresses e-mail utilisant votre nom de domaine (spoofing). Par exemple, l'enregistrement *v=spf1 include:mx.ovh.com ~all* indique que seuls les serveurs d'envoi liés à votre offre mail OVHCloud peuvent être considérés par le serveur de réception comme légitimes. Il est possible de renseigner cet enregistrement sous la forme d'un champ TXT ou via notre système de configuration automatique. Consultez notre documentation « [Ajouter un champ SPF à la configuration de son nom de domaine](../le-champ-spf/) » pour en savoir plus.

**DKIM** : Permet de vérifier l'authenticité du nom de domaine de l'expéditeur et assurer l'intégrité de l'e-mail envoyé. L'enregistrement DKIM se présente sous la forme d'une clé composée de plusieurs caractères. La clé DKIM est fournie par votre prestataire e-mail, il est possible de la renseigner sous la forme d'un champ TXT.

**DMARC** : Contribue à l'authentification des e-mails en association avec les méthodes SPF et/ou DKIM. Cette valeur vous sera donnée par votre fournisseur e-mail, elle sera au minimum associée à un enregistrement SPF ou DKIM.

#### Enregistrements étendus

**TXT** : Permet d'ajouter la valeur de votre choix, au format textuel, dans la zone DNS de votre nom de domaine. Cet enregistrement est souvent utilisé lors de processus de vérification.

> [!warning]
> 
> L'enregistrement TXT est limité à 255 caractères. Il est néanmoins possible, dans certains cas, de scinder votre valeur en plusieurs enregistrements. Renseignez-vous auprès de votre prestataire lorsque celui-ci vous demande de renseigner une valeur dépassant le quota des 255 caractères.
> 

**SRV** : Permet d'indiquer l'adresse d'un serveur gérant un service. Par exemple, il peut indiquer l'adresse d'un serveur SIP ou celle d'un serveur permettant la configuration automatique d'un logiciel de messagerie.

**CAA** : Permet de lister les autorités de certification autorisées à délivrer des certificats SSL pour un nom de domaine.

**NAPTR** : Utilisé en télécommunication pour diriger une requête émise par un terminal mobile vers un serveur. 

**LOC** : Utilisé pour renseigner les informations de position géographique.

**SSHFP** : Utilisé pour renseigner l'empreinte d'une clé publique SSH.

**TLSA** : Utilisé pour renseigner l'empreinte d'un certificat SSL/TLS.

### Éditer la zone DNS OVHcloud de votre nom domaine

Vous pouvez éditer la zone DNS OVHcloud de votre nom de domaine en ajoutant, modifiant ou en supprimant un enregistrement DNS. Pour cela, deux possibilités s'offrent à vous :

#### Modifier manuellement la zone en mode textuel 

Pour les utilisateurs avertis uniquement. 

Depuis l'onglet `Zone DNS`{.action}, cliquez sur `Modifier en mode textuel`{.action} puis suivez les étapes qui s'affichent.

#### Utiliser nos assistants de configuration

À partir de ce point, cette documentation abordera uniquement la configuration via nos assistants.

> [!primary]
>
> Munissez-vous des informations à modifier dans votre zone DNS OVHcloud. Si vous effectuez cette modification à la demande d'un fournisseur de service, ce dernier doit vous communiquer la liste des éléments à modifier.
>

#### Ajouter un nouvel enregistrement DNS

Pour ajouter un nouvel enregistrement DNS, depuis l'onglet `Zone DNS`{.action} de votre nom de domaine, cliquez sur le bouton `Ajouter une entrée`{.action} à droite du tableau. Sélectionnez le type de champ DNS puis suivez les étapes.

Nous vous invitons à vérifier au préalable si cet enregistrement n'existe pas déjà et ne pointe pas vers une cible différente. Pour cela, filtrez le contenu du tableau par type d'enregistrement ou par domaine. Si l'enregistrement existe déjà, nous vous invitons à le modifier grâce à la manipulation décrite juste après.

![dnszone](images/edit-dns-zone-ovh-add-entry.png){.thumbnail}

> Lorsque la cible de votre enregistrement est une URL, pensez à ponctuer celle-ci. En effet, si vous ne le faites pas, votre nom de domaine sera automatiquement ajouté à la fin de votre cible.
>
>Exemple : vous souhaitez créer un enregistrement CNAME de *test.mydomain.ovh* vers *mydomain.ovh*.
>
>Vous devez alors avoir comme cible *mydomain.ovh.* et non pas *mydomain.ovh* sans le **.** à la fin.

#### Modifier un enregistrement DNS existant 

Pour modifier un enregistrement DNS, toujours depuis l'onglet `Zone DNS`{.action} de votre espace client, cliquez sur le pictogramme `...`{.action} dans le tableau à droite de l'entrée choisie. Cliquez ensuite sur `Modifier l'entrée`{.action} puis suivez les étapes qui s'affichent.

![dnszone](images/edit-dns-zone-ovh-modify-entry.png){.thumbnail}

#### Supprimer un enregistrement DNS

Pour supprimer un enregistrement DNS, toujours depuis l'onglet `Zone DNS`{.action} de votre espace client, cliquez sur le pictogramme `...`{.action} dans le tableau à droite de l'entrée choisie. Cliquez ensuite sur `Supprimer l'entrée`{.action} puis suivez les étapes qui s'affichent.

Vous pouvez supprimer plusieurs entrées en une seule fois en les cochant depuis la partie gauche du tableau, puis en cliquant sur le bouton `Supprimer`{.action}.

![dnszone](images/edit-dns-zone-ovh-delete-entry.png){.thumbnail}

### Le temps de propagation

Une fois la zone DNS de votre nom de domaine modifiée, un temps de propagation de 24 heures maximum est nécessaire afin que les modifications soient effectives.

Si vous souhaitez réduire ce délai pour les prochaines éditions de votre zone DNS OVHcloud, vous pouvez le modifier, dans une certaine mesure, en ajustant le TTL (*Time To Live*) qui s'appliquera à tous les enregistrements de la zone DNS. 
Pour ce faire, positionnez-vous sur l'onglet `Zone DNS`{.action} de votre espace client, cliquez sur le bouton `TTL par défaut`{.action} puis suivez les étapes qui s'affichent. 

Vous pouvez aussi modifier le TTL d'un enregistrement DNS. Cependant, cette manipulation ne peut être effectuée que sur un enregistrement à la fois, en le modifiant ou lors d'un ajout.

## Aller plus loin

[Généralités sur les serveurs DNS](../generalites-serveurs-dns/){.external}.

[Ajouter un champ SPF à la configuration de son nom de domaine](../le-champ-spf/){.external}.

[Protégez votre domaine contre le Cache Poisoning avec le DNSSEC](https://www.ovh.com/fr/domaines/service_dnssec.xml){.external}.

Échangez avec notre communauté d'utilisateurs sur [https://community.ovh.com](https://community.ovh.com){.external}.
