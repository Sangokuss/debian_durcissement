# Partitionnement

A l'installation, dans l'idéal, il est recommandé de choisir un partitionnement permettant un durcissement avec partitions /home /var /tmp séparées.
Puis d'y appliquer les droits restrictifs celles-ci via l'édition du fichier /etc/fstab :

~# nano /etc/fstab

Options à ajouter aux partitions : noexec, nosuid, nodev (attention, la partition /var ne doit pas être en noexec, au risque sinon de ne pas pouvoir appliquer les mises à jour ;-) )

# Chiffrement des disques

Durant l'installation, il faut activer le chiffrement des disques durs (LVM chiffré) et utiliser des mots de passe complexes.

# Proxy

Sur Debian, l'installeur propose de configurer le proxy. Si vous disposez d'un proxy local sur votre réseau, il est recommandé de le faire à cette étape.
Cela permet l'utilisation du proxy pour les mises à jour (ce qui veut dire que les mises à jour seront alors impossibles en dehors de ce proxy, ce qui est potentiellement un contrainte forte).

# Réduction de la surface d'attaque

Il est fortement recommandé de n'installer que le strict minimum sur le système afin de réduire la surface d'attaque.

# Mises à jour

Appliquer immédiatement les mises à jour.
