# UE4SS Linux Native (build Arkya)

Build automatisé du portage Linux natif d'[UE4SS](https://github.com/UE4SS-RE/RE-UE4SS)
pour serveurs dédiés Palworld, depuis la branche `linux-native` de
[BlackBookOfficial/ue4ss-linux-palworld](https://github.com/BlackBookOfficial/ue4ss-linux-palworld).

Ce dépôt ne contient pas de code : il compile l'amont et publie l'archive correspondante.

## Pourquoi

La dernière release publiée en amont (`v1.0.2-palworld-linux`) est cassée : le serveur
démarre puis s'arrête en `SIGABRT`. Une douzaine de correctifs ont été fusionnés dans
`linux-native` sans qu'une nouvelle release soit publiée (issues
[#11](https://github.com/BlackBookOfficial/ue4ss-linux-palworld/issues/11) et
[#13](https://github.com/BlackBookOfficial/ue4ss-linux-palworld/issues/13)).

Le build est produit avec GCC 13 sur Debian 12 et un `libstdc++` statique. La release
amont, compilée sur Ubuntu 24.04, réclame `GLIBC_2.38` et ne se charge pas sur une base
Debian 12 (issue
[#16](https://github.com/BlackBookOfficial/ue4ss-linux-palworld/issues/16)).

## Build

La CI compare chaque jour le HEAD de `linux-native` au dernier tag publié ici et ne
reconstruit que s'il a bougé, un tag par commit amont. Elle échoue si le binaire dépasse
le plancher `GLIBC_2.36` ou relie `libstdc++` dynamiquement, pour que la régression ne
passe pas inaperçue.

L'archive reprend la disposition amont : la bibliothèque, ses réglages, les offsets
propres à Palworld et les mods intégrés. La console de debug y est coupée et les mods
orientés client désactivés, la GUI n'ayant ni display ni GPU sur un serveur dédié.

## Limites

Les mods Lua fonctionnent tels quels. Les mods C++ doivent être des `.so` Linux : les
`.dll` Windows ne se chargeront jamais, aucune couche de compatibilité n'existant ici.

Après une mise à jour de Palworld, `UE4SS.log` signale par des lignes `REFUSED` les hooks
désactivés parce que les adresses ont bougé.

## Licence

Le code compilé appartient à ses auteurs respectifs ; voir le dépôt amont.
