### Überprüfen der Vollständigkeit der Netzwerkdeklaration

**Rule ID:** S-DC-SubnetMissing

**Beschreibung:**
Das Ziel ist es sicherzustellen, dass der Mindestsatz an Subnetzen in der Domäne konfiguriert wurde.

**Technische Erklärung:**
Wenn mehrere Standorte in einer Domäne erstellt werden, sollten Netzwerke in der Domäne deklariert werden, um Prozesse wie die Zuordnung von Domaincontrollern zu optimieren. Darüber hinaus kann PingCastle die Informationen sammeln, um eine Netzwerkkarte zu erstellen. Diese Regel wurde ausgelöst, weil mindestens ein Domaincontroller eine IP-Adresse hat, die nicht in der Subnetzdeklaration gefunden wurde. Diese IP-Adressen wurden durch Abfragen der DC-FQDN-IP-Adresse sowohl im IPv6- als auch im IPv4-Format gesammelt.

**Empfohlene Lösung:**
1. Lokalisieren Sie die IP-Adresse, die nicht Teil der deklarierten Subnetze ist.
2. Fügen Sie dieses Subnetz dem Tool "Active Directory Sites" hinzu.
3. Wenn Sie IPv6-Adressen gefunden haben und diese nicht erwartet wurden, sollten Sie das IPv6-Protokoll auf der Netzwerkkarte deaktivieren.


$Subnet = "192.168.30.0/24"
$SiteName = "Default-First-Site-Name"  # Der Name Ihres Standorts in Active Directory

New-ADReplicationSubnet -Name $Subnet -Site $SiteName
