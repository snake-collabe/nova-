NOVA AI — BUILD REAI / WLLAMA WEBGPU
======================================

CE QUE CONTIENT CETTE VERSION
- L'interface NOVA d'origine conservée.
- Un onglet « Moteur IA » pour diagnostiquer WebGPU et charger un GGUF local.
- Le chat appelle réellement Wllama quand un modèle a été chargé.
- Profil prudent : GGUF <= 500 Mio, contexte 512 par défaut (1024 possible),
  4 couches GPU par défaut (2 ou 6 réglables).
- Le modèle est choisi depuis le sélecteur de fichiers de l'iPhone et lu localement.
- Le prompt système est orienté cybersécurité défensive et reverse engineering autorisé.

IMPORTANT : RUNTIME NON EMBARQUÉ DANS CETTE ARCHIVE
Cette PWA initialise Wllama 3.13.0 depuis jsDelivr au premier chargement du moteur.
Le runtime WASM et le module JavaScript ne sont donc PAS copiés physiquement dans ce ZIP.
Internet est nécessaire pour cette initialisation. Après téléchargement, le cache HTTP du
navigateur peut réutiliser certains fichiers, mais ce n'est pas une garantie de fonctionnement
hors ligne. Le GGUF, lui, n'est pas envoyé à un serveur.

POURQUOI CE CHOIX
L'archive NOVA reçue ne contenait que cinq fichiers PWA : index.html, manifest, service worker,
icône et README. Aucun paquet Wllama, aucun fichier .wasm et aucun GGUF n'étaient fournis.
L'environnement de préparation ne pouvait pas récupérer les binaires externes, donc il serait
mensonger de prétendre que le vrai runtime WASM est déjà inclus dans ce ZIP.

WEBGPU / SAFARI
Wllama v3.x prend en charge WebGPU et le mode compatibilité Safari. Safari peut utiliser le
build compat (Asyncify sans MEMORY64), avec une performance inférieure au build standard.
La compatibilité dépend de la version iOS/Safari et du pilote. Le diagnostic teste navigator.gpu
et requestAdapter, mais seul un chargement et un test sur l'iPhone réel confirment le résultat.
Le paramètre n_gpu_layers demande à Wllama d'envoyer des couches au GPU ; il ne garantit pas
que 4 couches soient stables sur un iPhone SE 2020 de 3 Go de RAM.

UTILISATION
1. Publie les fichiers du dossier sur un hébergement HTTPS (GitHub Pages, par exemple).
2. Ouvre NOVA dans Safari.
3. Va dans « Moteur IA », vérifie WebGPU.
4. Sélectionne un GGUF <= 500 Mio depuis Fichiers.
5. Garde 512 tokens et 4 couches GPU pour le premier essai.
6. Une fois le modèle chargé, va dans « Discussion » et utilise ton assistant.

LIMITES IPHONE SE 2020
- Le GGUF est lu depuis un File sélectionné ; Safari peut le perdre après fermeture.
- Le modèle doit être re-sélectionné après rechargement si le navigateur ne conserve pas
  l'accès au fichier.
- Un onglet peut être fermé par iOS si la mémoire disponible devient insuffisante.
- Commencer avec un petit modèle quantifié ; aucun modèle n'est inclus.
- Si WebGPU n'est pas exposé, cette version refuse le chargement au lieu de prétendre utiliser GPU.

SOURCE TECHNIQUE
Wllama documentation officielle : https://github.com/ngxson/wllama
Compatibilité Safari : https://github.com/ngxson/wllama/blob/master/compat/README.md
