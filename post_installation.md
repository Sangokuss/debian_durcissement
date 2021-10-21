# Configuration post installation

  * Configuration du parefeu en installant ufw sur Debian (idem sur Ubuntu),
  * Configuration du Umask passé en 077 (ou 027) plutôt que 022, en modifiant /etc/login.defs
  * Configuration IP du host dans /etc/hosts,
  * Configurer maunellement 1 ou 2 adresses 2 DNS (si possible des DNS internes, maîtrisés, ou, à défaut 1.1.1.1 et 9.9.9.9),
  * sudo nano /etc/host.conf

<code>order bind,hosts
nospoof on
</code>

  * Utilisation de 2 serveurs de nom de domaine,
  * Configuration du Sha maximum dans /etc/login.defs (à 5001 au lieu de 5000 par exemple),
  * Installation des utilitaires suivants : rkhunter, chkrootkit, usbguard
  * Installation de l'utilitaire Lynis pour contrôle du niveau de sécurité/durcissement,
  * Désactivation des ports USB pour les postes mobiles via BIOS,
  * Désactivation des ports firewire, …
  * Selon les besoin, désactiver les périphériques USB / Firewire (hdmi) / Thunderbolt via kernel :

<code>
su
echo 'install usb-storage /bin/true' >> /etc/modprobe.d/disable-usb-storage.conf
echo "blacklist firewire-core" >> /etc/modprobe.d/firewire.conf
echo "blacklist thunderbolt" >> /etc/modprobe.d/thunderbolt.conf
</code>

NB : il est possible de revenir en arrière sur ce réglage en éditant le fichier *.conf créé par les commandes ci-dessus, puis en commentant (càd en ajoutant un hashtag “#” devant la ligne, ce qui va changer la couleur de la ligne 😉 → un reboot est nécessaire pour prise en compte)

  * Configuration du message /etc/issue (et issue.net), avec une bannière officielle :

<code>WARNING:  Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your actions
may be monitored if unauthorized usage is suspected.
</code>

  * Dans la mesure du possible, déployer un outil de supervision. Je recommande l'outil open source Wazuh.
  * Cet outil facilite grandement la surveillance et la lecture des logs. sous réserve de configurer un serveur dédié de centralisation et de configurer l'agent Wazuh sur la machine.
  * Désactivation du mode recovery du Grub2 :

<code> ~ # nano /etc/default/grub
</code>

Mettre :

# Uncomment to disable generation of recovery mode menu entries
#GRUB_DISABLE_RECOVERY="true"

à 
<code> 
# Uncomment to disable generation of recovery mode menu entries
**GRUB_DISABLE_RECOVERY="true"**
</code> 

  * Il est également recommandé de forcer l'utilisation de la directive iommu=force dans le GRUB.
  * Durcissement kernel (éditer fichier /etc/sysctl.d/99-sysctl.conf):

<code> 
#kernel.sysrq=438
fs.suid_dumpable = 0
kernel.dmesg_restrict =	1
kernel.core_uses_pid = 1
# Restriction d'accès au buffer dmesg
kernel.kptr_restrict = 2
kernel.yama.ptrace_scope = 1
kernel.sysrq = 0
fs.protected_hardlinks  =1
fs.protected_symlinks = 1
kernel.ctrl-alt-del = 0
# Activation de l'ASLR
kernel.randomize_va_space = 2
# Interdiction de mapper de la mémoire dans les adresses basses (0)
vm.mmap_min_addr = 65536
# Espace de choix plus grand pour les valeurs de PID
kernel.pid_max = 65536
# Obfuscation des adresses mémoire kernel
# Restreint l'utilisation du sous système perf
kernel.perf_event_paranoid = 3
fs.protected_fifos = 2
kernel.perf_event_max_sample_rate = 1
kernel.perf_cpu_time_max_percent = 1
# désactivation de ipv6 pour toutes les interfaces
net.ipv6.conf.all.disable_ipv6 = 1
dev.tty.ldisc_autoload = 0
# désactivation de l’auto configuration pour toutes les interfaces
net.ipv6.conf.all.autoconf = 0
net.ipv4.conf.default.accept_source_route = 0
# désactivation de ipv6 pour les nouvelles interfaces (ex:si ajout de carte réseau)
net.ipv6.conf.default.disable_ipv6 = 1
# Désactiver le support des "router solicitations"
net.ipv6.conf.all.router_solicitations = 0
net.ipv6.conf.default.router_solicitations = 0
# Ne pas accepter les "router preferences" par "router advertisements"
net.ipv6.conf.all.accept_ra_rtr_pref = 0
net.ipv6.conf.default.accept_ra_rtr_pref = 0
# Pas de configuration auto des prefix par "router advertisements"
net.ipv6.conf.all.accept_ra_pinfo = 0
net.ipv6.conf.default.accept_ra_pinfo = 0
# Pas d'apprentissage du routeur par défaut par "router advertisements"
net.ipv6.conf.all.accept_ra_defrtr = 0
net.ipv6.conf.default.accept_ra_defrtr = 0
# Pas de configuration auto des adresses à partir des "router advertisements "
net.ipv6.conf.all.autoconf = 0
net.ipv6.conf.default.autoconf = 0
# Ne pas accepter les ICMP de type redirect
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
# Refuser les packets de source routing
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
# Nombre maximal d'adresses autoconfigurées par interface
net.ipv6.conf.all.max_addresses = 1
net.ipv6.conf.default.max_addresses = 1
# désactivation de l’auto configuration pour les nouvelles interfaces
net.ipv6.conf.default.autoconf = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
# Contrôle l'utilisation des syncookies TCP
# Activer les protections SYN-flood
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_synack_retries = 5
# Prévenir contre les 'attaques syn flood' courantes
#net.ipv4.tcp_syncookies = 1
# Ignore ICMP request:
net.ipv4.icmp_echo_ignore_all = 1
# Ignore Broadcast request:
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.default.log_martians = 1
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.forwarding = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.core.bpf_jit_harden = 2
kernel.unprivileged_bpf_disabled = 1
</code>

Faire ensuite un ~# sysctl -p # pour recharger 😉

  * Couper les services inutiles :

<code> 
~# echo "install tipc /bin/true" >> /etc/modprobe.d/CIS.conf
~# echo "install rds /bin/true" >> /etc/modprobe.d/CIS.conf
~# echo "install sctp /bin/true" >> /etc/modprobe.d/CIS.conf
~# echo "install dccp /bin/true" >> /etc/modprobe.d/CIS.conf
</code> 
