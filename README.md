# 🚀 TP Virtualisation - Creation automatique de VMs Proxmox avec Ansible (API REST)

## 👨‍💻 Projet
Automatiser la creation de machines virtuelles sur un serveur Proxmox à l'aide d'Ansible en exploitant **l'API REST Proxmox** (et non les modules classiques).  
Ce projet permet de provisionner dynamiquement des VMs sur un noeud Proxmox via des appels HTTP `uri`.

---

## 📦 Fonctionnalites

- Recuperation de la liste des VMs existantes via API
- Calcul automatique du prochain `VMID` disponible
- Creation d'une nouvelle VM avec ISO, reseau et disque
- Demarrage automatique de la VM juste apres sa creation
- Utilisation de tokens d'API Proxmox (securite sans mot de passe)
- Structure Ansible modulaire et reutilisable

---

## 📁 Structure du projet

```
proxmox-with-ansible-atom/
├── inventories/
│   └── dev/
│       ├── hosts
│       └── group_vars/
│           └── all.yml
├── playbooks/
│   └── create_vm.yml
└── README.md
```

---

## ✅ Prerequis

### Machine de controle
- Ubuntu/Debian avec `Ansible` installé :
  ```bash
  sudo apt update && sudo apt install ansible -y
  ```
- Acces Internet ou installation offline de la collection si besoin

### Serveur cible
- Proxmox VE 7.x ou 8.x fonctionnel avec accès à l'interface Web (API ouverte sur port 8006)
- ISO `lubuntu.iso` préalablement uploadé dans le stockage Proxmox (`local` ou `local-lvm`)
- Un token d’API généré pour l’utilisateur Proxmox :
  - Utilisateur : `root@pam`
  - Token : `ansible`
  - Permissions : accès API pour `VM.Allocate`, `VM.Config`, `VM.PowerMgmt`

---



## ⚙️ Lancement du playbook

### Commande d’execution :
```bash
ansible-playbook -i inventories/dev/hosts playbooks/create_vm.yml
```

---

## ✅ Resultat attendu

- Une nouvelle VM apparait dans l'interface Web Proxmox
- Elle porte un nom auto-genere : `vm-lubuntu-<vmid>`
- Elle est demarree automatiquement
- L’`vmid` est attribue automatiquement en se basant sur les VMs deja existantes

---

## 🛡️ Bonnes pratiques

- Ne pas exposer les tokens dans les fichiers `group_vars` non chiffrés → utiliser `ansible-vault`
- Toujours tester la commande `curl https://IP_PROXMOX:8006/api2/json` pour valider que l’API Proxmox est joignable
- Utiliser des noms de stockage et noms de noeuds **identiques** à ceux affichés dans l’interface web Proxmox

---

## 🔗 Ressources

- [Documentation officielle Proxmox API](https://pve.proxmox.com/pve-docs/api-viewer/index.html)
- [Ansible URI module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html)

---

## 👥 Auteurs

Projet realise dans le cadre d’un TP de virtualisation - Groupe **ATOM**
