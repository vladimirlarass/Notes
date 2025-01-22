Ce shell est le shell WinRM ultime pour le piratage/pentesting.

WinRM (Windows Remote Management) est l'implémentation Microsoft du protocole WS-Management. Un protocole standard basé sur SOAP qui permet l'interopérabilité du matériel et des systèmes d'exploitation de différents fournisseurs. Microsoft l'a inclus dans ses systèmes d'exploitation afin de faciliter la vie des administrateurs système.

Ce programme peut être utilisé sur n'importe quel serveur Microsoft Windows avec cette fonctionnalité activée (généralement sur le port 5985), bien sûr uniquement si vous disposez des informations d'identification et des autorisations pour l'utiliser. Nous pouvons donc dire qu'il pourrait être utilisé dans une phase de piratage/pentesting post-exploitation. Le but de ce programme est de fournir des fonctionnalités agréables et faciles à utiliser pour le piratage. Il peut également être utilisé à des fins légitimes par les administrateurs système, mais la plupart de ses fonctionnalités sont axées sur le piratage/pentesting.

Il est principalement basé sur la bibliothèque WinRM Ruby qui a changé sa façon de fonctionner depuis sa version 2.0. Désormais, au lieu d'utiliser le protocole WinRM, il utilise PSRP (Powershell Remoting Protocol) pour initialiser les pools d'espaces d'exécution ainsi que pour créer et traiter les pipelines.



https://github.com/Hackplayers/evil-winrm