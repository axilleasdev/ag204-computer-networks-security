# AG204 — Computer Networks and Security
## Εργασία: Σχεδιασμός Δικτύου Νέου Κτιρίου

**Φοιτητής:** Αχιλλέας Καρατζάς  
**Μάθημα:** AG204 Computer Networks and Security  
**Πανεπιστήμιο:** University of Essex  
**Ημερομηνία:** Ιούνιος 2026

---

## Μέρος Α — Subnetting

### Α.Ι) Μάσκα Υποδικτύου (Subnet Mask)

#### Δεδομένα Προβλήματος

Η εταιρεία διαθέτει από τον ISP τη διεύθυνση **83.112.8.128/25**. Το νέο κτίριο έχει 3 ορόφους και θα φιλοξενήσει **5 τμήματα** με τουλάχιστον **12 υπολογιστές** ανά τμήμα.

#### Ανάλυση Αρχικού Δικτύου

Η διεύθυνση 83.112.8.128/25 σημαίνει:
- **Network prefix:** 25 bits
- **Host bits:** 32 - 25 = 7 bits
- **Συνολικές διευθύνσεις:** 2⁷ = 128 (126 χρησιμοποιήσιμες)
- **Εύρος:** 83.112.8.128 — 83.112.8.255
- **Subnet mask:** 255.255.255.128

#### Υπολογισμός Απαιτήσεων

Χρειαζόμαστε 5 υποδίκτυα (ένα ανά τμήμα) με τουλάχιστον 12 hosts ανά υποδίκτυο.

**Βήμα 1 — Bits για subnets:** Για 5 υποδίκτυα χρειαζόμαστε ⌈log₂(5)⌉ = 3 bits δανεικά από το host portion (2³ = 8 ≥ 5 ✓).

**Βήμα 2 — Bits για hosts:** Μένουν 7 - 3 = 4 bits → 2⁴ - 2 = **14 usable hosts** ≥ 12 ✓

**Βήμα 3 — Νέα μάσκα:** /25 + 3 = **/28**

```
/28 σε binary: 11111111.11111111.11111111.11110000
Σε decimal:    255.255.255.240
```

#### Αιτιολόγηση

**Γιατί /28 και όχι /27;** Η /27 θα έδινε μόνο 4 υποδίκτυα (2² = 4), ανεπαρκή για τα 5 τμήματα. Η /28 είναι η βέλτιστη λύση που ικανοποιεί ταυτόχρονα τις απαιτήσεις σε αριθμό υποδικτύων (8 ≥ 5) και hosts (14 ≥ 12).

**Η απαιτούμενη μάσκα υποδικτύου είναι: 255.255.255.240 (/28)**

---

### Α.ΙΙ) Διευθύνσεις Δικτύου των 5 Υποδικτύων

#### Μέθοδος Υπολογισμού

Κάθε υποδίκτυο /28 περιέχει 2⁴ = 16 διευθύνσεις (block size = 16). Τα υποδίκτυα ξεκινούν από 83.112.8.128 και αυξάνονται κατά 16. Η πρώτη διεύθυνση κάθε block είναι η network address και η τελευταία η broadcast address.

#### Αναλυτικοί Υπολογισμοί σε Binary

Στο τελευταίο octet, τα πρώτα 4 bits ανήκουν στο network portion (1 αρχικό + 3 δανεικά) και τα τελευταία 4 στο host portion:

```
Δομή: N|SSS|HHHH
N = αρχικό network bit (πάντα 1)
SSS = subnet bits (3)
HHHH = host bits (4)
```

**Subnet 1 (Sales):**
```
Subnet bits: 000 → Octet: 1|000|0000 = 128
Network: 83.112.8.128/28 | Hosts: .129—.142 | Broadcast: .143
```

**Subnet 2 (Service):**
```
Subnet bits: 001 → Octet: 1|001|0000 = 144
Network: 83.112.8.144/28 | Hosts: .145—.158 | Broadcast: .159
```

**Subnet 3 (Management):**
```
Subnet bits: 010 → Octet: 1|010|0000 = 160
Network: 83.112.8.160/28 | Hosts: .161—.174 | Broadcast: .175
```

**Subnet 4 (E-commerce):**
```
Subnet bits: 011 → Octet: 1|011|0000 = 176
Network: 83.112.8.176/28 | Hosts: .177—.190 | Broadcast: .191
```

**Subnet 5 (Marketing):**
```
Subnet bits: 100 → Octet: 1|100|0000 = 192
Network: 83.112.8.192/28 | Hosts: .193—.206 | Broadcast: .207
```

#### Συγκεντρωτικός Πίνακας

| # | Τμήμα | Όροφος | Network Address | First Host | Last Host | Broadcast | Gateway |
|---|-------|--------|-----------------|------------|-----------|-----------|---------|
| 1 | Sales | 1ος | 83.112.8.128/28 | .129 | .142 | .143 | .129 |
| 2 | Service | 1ος | 83.112.8.144/28 | .145 | .158 | .159 | .145 |
| 3 | Management | 2ος | 83.112.8.160/28 | .161 | .174 | .175 | .161 |
| 4 | E-commerce | 2ος | 83.112.8.176/28 | .177 | .190 | .191 | .177 |
| 5 | Marketing | 3ος | 83.112.8.192/28 | .193 | .206 | .207 | .193 |

#### Κατανομή Ορόφων

- **1ος Όροφος:** Sales + Service (πελατοκεντρικά τμήματα)
- **2ος Όροφος:** Management + E-commerce (διοικητικά τμήματα)
- **3ος Όροφος:** Marketing

Τα τμήματα ομαδοποιούνται βάσει συνεργασίας: τα πελατοκεντρικά (Sales, Service) τοποθετούνται μαζί στον πρώτο όροφο καθώς χρειάζονται στενή επικοινωνία, ενώ τα διοικητικά (Management, E-commerce) στον δεύτερο. Η τοποθέτηση συνεργαζόμενων τμημάτων στον ίδιο όροφο μειώνει inter-floor traffic και απλοποιεί την καλωδίωση. Σε κάθε υποδίκτυο, η πρώτη χρησιμοποιήσιμη IP ανατίθεται στο default gateway (router sub-interface).

#### Εφεδρικά Υποδίκτυα

Η μάσκα /28 παρέχει 8 υποδίκτυα αλλά χρησιμοποιούμε 5, άρα μένουν 3 εφεδρικά (.208/28, .224/28, .240/28) για μελλοντική επέκταση (νέα τμήματα, server subnet, guest WiFi). Αυτό αποτελεί καλή πρακτική σχεδιασμού (scalability) και αποφεύγει πλήρη επανασχεδιασμό σε περίπτωση ανάπτυξης.

---

## Μέρος Β — Σχεδιασμός Δικτύου (Packet Tracer)

### Τοπολογία & Εξοπλισμός

Επιλέχθηκε τοπολογία αστέρα (star) με τεχνική Router-on-a-Stick. Η κατανομή του εξοπλισμού ακολουθεί τη φυσική δομή του κτιρίου, με ένα switch ανά όροφο:

- **1× Router (Cisco 2911):** Κεντρικός δρομολογητής με 3 GigabitEthernet θύρες και sub-interfaces (ένα ανά VLAN) για inter-VLAN routing.
- **3× Switch (Cisco 2960-24TT):** Ένα switch ανά όροφο. Κάθε switch διαθέτει 24 θύρες FastEthernet, επαρκείς για τα τμήματα του ορόφου του (έως 2×12 = 24 PCs), και 2 GigabitEthernet uplink θύρες για το trunk προς τον router.
- **60× PCs:** 12 υπολογιστές ανά τμήμα, κατανεμημένα στα αντίστοιχα VLANs.
- **Καλωδίωση:** Straight-through (PC↔Switch, Switch↔Router).

Η αντιστοίχιση ορόφων/switches είναι:

- **Όροφος 1 (Floor1-SW):** Sales (VLAN 10) + Service (VLAN 20) → Router Gig0/0
- **Όροφος 2 (Floor2-SW):** Management (VLAN 30) + E-commerce (VLAN 40) → Router Gig0/1
- **Όροφος 3 (Floor3-SW):** Marketing (VLAN 50) → Router Gig0/2

> **[SCREENSHOT 1 — Τοπολογία Δικτύου]**
> *Εισάγετε εδώ στιγμιότυπο της πλήρους τοπολογίας από το Packet Tracer (Router + 3 Switches + PCs).*

### VLANs & Sub-interfaces

| Router Sub-interface | VLAN | IP (Gateway) | Τμήμα | Όροφος/Switch |
|----------------------|------|--------------|-------|---------------|
| Gig0/0.10 | 10 | 83.112.8.129/28 | Sales | 1 / Floor1-SW |
| Gig0/0.20 | 20 | 83.112.8.145/28 | Service | 1 / Floor1-SW |
| Gig0/1.30 | 30 | 83.112.8.161/28 | Management | 2 / Floor2-SW |
| Gig0/1.40 | 40 | 83.112.8.177/28 | E-commerce | 2 / Floor2-SW |
| Gig0/2.50 | 50 | 83.112.8.193/28 | Marketing | 3 / Floor3-SW |

### Αιτιολόγηση Σχεδιαστικών Αποφάσεων

1. **Ένα switch ανά όροφο:** Η φυσική κατανομή ακολουθεί τη δομή του κτιρίου. Κάθε όροφος έχει το δικό του switch, μειώνοντας την καλωδίωση μεταξύ ορόφων και διευκολύνοντας τη διαχείριση και το troubleshooting. Ένα 24-port switch καλύπτει άνετα τα έως 24 PCs κάθε ορόφου (2 τμήματα × 12).

2. **VLANs ανά τμήμα:** Εξασφαλίζουν λογική τμηματοποίηση ανεξάρτητη από φυσική θέση, μειώνουν το broadcast domain, και βελτιώνουν την ασφάλεια αποτρέποντας μη εξουσιοδοτημένη πρόσβαση μεταξύ τμημάτων. Σημειώνεται ότι σε ορόφους με 2 τμήματα, τα δύο τμήματα μοιράζονται φυσικά το switch αλλά παραμένουν λογικά απομονωμένα σε διαφορετικά VLANs.

3. **Router-on-a-Stick:** Οικονομική λύση inter-VLAN routing. Κάθε switch συνδέεται με τον router μέσω ενός trunk link, και ο router χρησιμοποιεί sub-interfaces με 802.1Q encapsulation. Οι 3 GigabitEthernet θύρες του 2911 αντιστοιχούν ακριβώς στα 3 switches.

4. **Αρίθμηση VLANs ανά δεκάδα (10, 20, 30...):** Αφήνει χώρο για μελλοντική προσθήκη VLANs χωρίς ανακατανομή.

### Configuration
### Configuration

Η υλοποίηση του σχεδιασμού έγινε στο Cisco Packet Tracer. Ο router λειτουργεί ως δρομολογητής μεταξύ των πέντε VLANs (inter-VLAN routing), με κάθε sub-interface να αποτελεί το default gateway του αντίστοιχου τμήματος. Τα τρία switches διαχειρίζονται την κίνηση εντός κάθε ορόφου: οι θύρες των υπολογιστών ανήκουν στο VLAN του τμήματός τους, ενώ η σύνδεση με τον router γίνεται μέσω trunk που μεταφέρει όλα τα VLANs με 802.1Q tagging.

Με τον τρόπο αυτό, υπολογιστές του ίδιου τμήματος επικοινωνούν απευθείας μέσω του switch (Layer 2), ενώ η επικοινωνία μεταξύ διαφορετικών τμημάτων περνά υποχρεωτικά από τον router (Layer 3), όπου μπορούν να εφαρμοστούν πολιτικές ασφαλείας (ACLs). Το πλήρες configuration των συσκευών περιλαμβάνεται στο συνοδευτικό αρχείο .pkt.

### Επαλήθευση Λειτουργίας

Για να επιβεβαιωθεί η σωστή λειτουργία του inter-VLAN routing, εκτελέστηκε ping από υπολογιστή ενός τμήματος προς υπολογιστή άλλου τμήματος (π.χ. Sales PC → Marketing PC).

> **[SCREENSHOT 2 — Επιτυχές Ping μεταξύ VLANs]**
> *Εισάγετε εδώ στιγμιότυπο επιτυχούς ping από PC ενός τμήματος σε PC άλλου τμήματος.*

---

## Μέρος Γ — Ασφάλεια Δικτύου

### Εισαγωγή

Η ασφάλεια δικτύου αποτελεί κρίσιμη πτυχή σε κάθε εταιρικό περιβάλλον. Παρακάτω αναλύονται οι κυριότεροι κίνδυνοι και τα μέτρα αντιμετώπισης για το δίκτυο του νέου κτιρίου.

### 1. Κακόβουλο Λογισμικό (Malware)

**Περιγραφή:** Το malware περιλαμβάνει ιούς (viruses) που εξαπλώνονται μέσω αρχείων, worms που αυτοαναπαράγονται μέσω δικτύου χωρίς ανθρώπινη παρέμβαση, trojans που μεταμφιέζονται σε νόμιμο λογισμικό, και ransomware που κρυπτογραφεί αρχεία απαιτώντας λύτρα. Τα worms είναι ιδιαίτερα επικίνδυνα σε εταιρικά δίκτυα.

**Αντιμετώπιση:**
- Antivirus/anti-malware σε κάθε σταθμό εργασίας με τακτική ενημέρωση
- Patch management για κλείσιμο γνωστών ευπαθειών
- Εκπαίδευση χρηστών για αναγνώριση ύποπτων αρχείων
- Τακτικά backups (κανόνας 3-2-1)
- Network segmentation: τα VLANs περιορίζουν την εξάπλωση worms εντός ενός τμήματος

### 2. Phishing & Social Engineering

**Περιγραφή:** Οι επιθέσεις phishing χρησιμοποιούν παραπλανητικά emails ή ψεύτικες ιστοσελίδες για απόσπαση credentials. Το social engineering εκμεταλλεύεται ανθρώπινη ψυχολογία (εμπιστοσύνη, φόβο, επείγον) παρακάμπτοντας πλήρως τα τεχνικά μέτρα. Πάνω από 90% των κυβερνοεπιθέσεων ξεκινούν με phishing.

**Αντιμετώπιση:**
- Security awareness training για το προσωπικό
- Email filtering (SPF, DKIM, DMARC)
- Multi-Factor Authentication (MFA): ακόμα κι αν κλαπούν credentials, ο δεύτερος παράγοντας αποτρέπει πρόσβαση

### 3. Επιθέσεις DDoS

**Περιγραφή:** Οι DDoS επιθέσεις κατακλύζουν δικτυακούς πόρους με traffic από botnets, καθιστώντας υπηρεσίες μη διαθέσιμες. Ιδιαίτερα κρίσιμο για το τμήμα E-commerce όπου η μη διαθεσιμότητα σημαίνει απώλεια εσόδων.

**Αντιμετώπιση:**
- Firewalls με rate limiting και traffic filtering
- Intrusion Detection/Prevention Systems (IDS/IPS)
- Redundant Internet connections για αποφυγή single point of failure

### 4. Man-in-the-Middle (MitM)

**Περιγραφή:** Ο επιτιθέμενος παρεμβάλλεται μεταξύ δύο επικοινωνούντων μερών μέσω ARP spoofing ή DNS spoofing, υποκλέπτοντας ή τροποποιώντας δεδομένα. Σε εταιρικό δίκτυο, ένας εσωτερικός επιτιθέμενος μπορεί εύκολα να εκτελέσει ARP spoofing στο ίδιο VLAN.

**Αντιμετώπιση:**
- Κρυπτογράφηση TLS/SSL σε κάθε υπηρεσία
- VPN για απομακρυσμένη πρόσβαση
- Port security και Dynamic ARP Inspection (DAI) στα Cisco switches

### 5. Μη Εξουσιοδοτημένη Πρόσβαση

**Περιγραφή:** Πρόσβαση χωρίς εξουσιοδότηση μέσω brute force attacks, κλεμμένων credentials, ή εκμετάλλευση ευπαθειών. Μη εξουσιοδοτημένη πρόσβαση στο Management VLAN μπορεί να δώσει πλήρη έλεγχο του δικτύου.

**Αντιμετώπιση:**
- Strong password policies (≥12 χαρακτήρες, πολυπλοκότητα)
- MFA σε κρίσιμα συστήματα
- Access Control Lists (ACLs) στον router μεταξύ VLANs
- Αρχή Ελάχιστου Προνομίου (Least Privilege)
- Account lockout μετά από αποτυχημένες προσπάθειες

### 6. SQL Injection & XSS

**Περιγραφή:** Εισαγωγή κακόβουλου κώδικα μέσω web φορμών. Το SQL Injection στοχεύει βάσεις δεδομένων, ενώ το XSS εκτελεί scripts στους browsers χρηστών. Κρίσιμο για το τμήμα E-commerce που χειρίζεται δεδομένα πελατών και πληρωμών.

**Αντιμετώπιση:**
- Input validation & parameterized queries
- Web Application Firewall (WAF)
- Τακτικά penetration tests

### 7. Εσωτερικές Απειλές (Insider Threats)

**Περιγραφή:** Δυσαρεστημένοι ή αμελείς υπάλληλοι με νόμιμη πρόσβαση μπορούν να κλέψουν δεδομένα, να σαμποτάρουν συστήματα, ή να αποκαλύψουν εμπιστευτικές πληροφορίες. Δύσκολο να εντοπιστούν καθώς ενεργούν εντός νόμιμων ορίων.

**Αντιμετώπιση:**
- Role-Based Access Control (RBAC)
- Network monitoring μέσω SIEM systems
- Data Loss Prevention (DLP) solutions
- Τακτικοί audits δικαιωμάτων πρόσβασης

### 8. Φυσική Ασφάλεια

**Περιγραφή:** Φυσική πρόσβαση σε εξοπλισμό (switches, router, servers) μπορεί να οδηγήσει σε κλοπή δεδομένων, εγκατάσταση rogue devices ή καταστροφή υποδομής. Κάθε όροφος του κτιρίου μας έχει wiring closet που χρειάζεται προστασία.

**Αντιμετώπιση:**
- Κλειδωμένα server rooms & wiring closets
- Κάμερες παρακολούθησης (CCTV) και access control
- Disable αχρησιμοποίητων switch ports (`shutdown`)

### Συμπέρασμα

Η ασφάλεια δικτύου απαιτεί πολυεπίπεδη προσέγγιση (defense in depth): συνδυασμός τεχνικών μέτρων (firewalls, encryption, IDS/IPS, ACLs), διοικητικών πολιτικών (security policies, training, incident response plans) και φυσικής ασφάλειας (locks, cameras, access control). Τα VLANs που σχεδιάσαμε στο Μέρος Β αποτελούν ήδη ένα πρώτο επίπεδο segmentation — ακόμα κι αν παραβιαστεί ένα τμήμα, η εξάπλωση στα υπόλοιπα δυσχεραίνεται σημαντικά. Κανένα μεμονωμένο μέτρο δεν αρκεί, γι' αυτό η τακτική αξιολόγηση και ενημέρωση πρέπει να αποτελεί συνεχή διαδικασία.

---

## Μέρος Δ — Ανάλυση Wireshark

Στην ενότητα αυτή αναλύεται το αρχείο καταγραφής `FileTrace1.pcapng` με το εργαλείο Wireshark. Για κάθε ερώτηση παρατίθεται το φίλτρο που χρησιμοποιήθηκε, η απάντηση, στιγμιότυπο οθόνης του σχετικού πακέτου και η επεξήγησή του.

### Ερώτηση 1: Διεύθυνση IP του διακομιστή DNS

**Φίλτρο Wireshark:** `dns`

**Απάντηση:** Ο DNS server έχει διεύθυνση **192.168.2.1**.

> **[SCREENSHOT Δ.1]** — Frame 5 (query) ή 7 (response)

**Επεξήγηση:** Όλα τα DNS queries του client (192.168.2.9) αποστέλλονται προς τη διεύθυνση 192.168.2.1 στη θύρα 53, και οι απαντήσεις (responses) επιστρέφουν από την ίδια διεύθυνση. Συνεπώς το 192.168.2.1 είναι ο τοπικός DNS server του δικτύου (συνήθως ο router/gateway που λειτουργεί ως DNS resolver).

---

### Ερώτηση 2: IPs των έγκυρων name servers του www.berkeley.edu

**Φίλτρο Wireshark:** `dns.qry.name contains "berkeley.edu"`

**Απάντηση:** Οι authoritative name servers του www.berkeley.edu (domain w3.berkeley.edu) είναι:
- **adns1.berkeley.edu** → 128.32.136.3
- **adns2.berkeley.edu** → 128.32.136.14
- **aodns1.berkeley.edu** → 192.35.225.133
- **aodns2.berkeley.edu** → 128.253.35.148

> **[SCREENSHOT Δ.2]** — Frame 473 (DNS response, ενότητες Authoritative nameservers + Additional records)

**Επεξήγηση:** Στη DNS response, η ενότητα "Authoritative nameservers" περιέχει τα NS records που δηλώνουν τα ονόματα των name servers. Η ενότητα "Additional records" περιέχει τα αντίστοιχα A records που δίνουν τις IP διευθύνσεις κάθε name server.

---

### Ερώτηση 3: IP του web server που φιλοξενεί το www.uoa.gr

**Φίλτρο Wireshark:** `dns.qry.name contains "uoa.gr"`

**Απάντηση:** Το www.uoa.gr είναι CNAME (alias) του sites.uoa.gr, που αντιστοιχεί στη διεύθυνση **195.134.71.229** (επιστρέφονται και οι 195.134.99.120, 195.134.99.121, 83.212.5.22).

> **[SCREENSHOT Δ.3]** — Frame 891 (DNS response με τα A records)

**Επεξήγηση:** Η DNS response δείχνει ότι το www.uoa.gr είναι CNAME του sites.uoa.gr, του οποίου το A record δίνει την IP του web server (195.134.71.229). Οι πολλαπλές IP υποδηλώνουν load balancing / πολλαπλούς servers.

---

### Ερώτηση 4: Θύρα επικοινωνίας με τον DNS server για το www.uoa.gr

**Φίλτρο Wireshark:** `dns.qry.name contains "uoa.gr"`

**Απάντηση:** Η επικοινωνία γίνεται στη θύρα **53** (destination port του DNS server). Ο client χρησιμοποιεί εφήμερη θύρα 58502.

> **[SCREENSHOT Δ.4]** — Frame 889 (δείξτε το UDP layer: src port 58502 → dst port 53)

**Επεξήγηση:** Το DNS λειτουργεί πάνω από UDP στη well-known θύρα 53. Στο πακέτο, η destination port του query είναι 53 (η θύρα στην οποία ακούει ο DNS server), ενώ η source port (58502) είναι εφήμερη θύρα που επιλέγει ο client.

---

### Ερώτηση 5: Username σύνδεσης με τον host 172.16.137.53

**Φίλτρο Wireshark:** `ip.addr == 172.16.137.53 && ftp`

**Απάντηση:**
- Username: **network**
- Πρωτόκολλο: **FTP** (θύρα 21)
- Επιτυχής σύνδεση: **ΟΧΙ** — απέτυχε (κωδικός απάντησης 530 "Login authentication failed")

> **[SCREENSHOT Δ.5]** — Frames 1411 (USER network) + 1433 (530 Login failed)

**Επεξήγηση:** Στο πακέτο 1411 φαίνεται η εντολή `USER network`. Ο client έδωσε password "12345" (frame 1418), αλλά ο server απάντησε με κωδικό **530** (frame 1433) που σημαίνει αποτυχία ταυτοποίησης. Το FTP μεταδίδει credentials σε καθαρό κείμενο (plaintext), γι' αυτό είναι ορατά στο Wireshark.

---

### Ερώτηση 6: Password σύνδεσης με τον host 172.16.137.59

**Φίλτρο Wireshark:** `ip.addr == 172.16.137.59 && ftp`

**Απάντηση:**
- Password: **Diktya**
- Πρωτόκολλο: **FTP** (θύρα 21)
- Επιτυχής σύνδεση: **ΟΧΙ** — απέτυχε (κωδικός απάντησης 530 "Login incorrect")

> **[SCREENSHOT Δ.6]** — Frames 1630 (PASS Diktya) + 1631 (530 Login incorrect)

**Επεξήγηση:** Ο client συνδέθηκε με username "hello" (frame 1626) και password `Diktya` (frame 1630). Ο server απάντησε με κωδικό **530** (frame 1631) → η σύνδεση απέτυχε. Όπως και πριν, το FTP εκθέτει το password σε plaintext.

---

### Ερώτηση 7: Θύρες και bytes επικοινωνίας με τον host 172.16.137.59

**Φίλτρο Wireshark:** `ip.addr == 172.16.137.59` (+ Statistics → Conversations → TCP)

**Απάντηση:**
- Θύρα πλευράς client: **54289**
- Θύρα πλευράς server: **21**
- Σύνολο bytes: **889 bytes** (14 frames)

> **[SCREENSHOT Δ.7]** — Statistics → Conversations → TCP, γραμμή 172.16.137.59:21

**Επεξήγηση:** Από την καρτέλα Conversations φαίνεται η σύνδεση 192.168.2.9:54289 ↔ 172.16.137.59:21. Ο client χρησιμοποιεί εφήμερη θύρα 54289, ο FTP server τη θύρα 21. Συνολικά μεταφέρθηκαν 889 bytes (414 bytes client→server + 475 bytes server→client).

---

### Ερώτηση 8: Transport Layer protocol για τις συνδέσεις FTP

**Φίλτρο Wireshark:** `ftp`

**Απάντηση:** **TCP** (Transmission Control Protocol).

> **[SCREENSHOT Δ.8]** — Οποιοδήποτε FTP frame, με ανοιχτό το Transport Layer (TCP, port 21)

**Επεξήγηση:** Το FTP χρησιμοποιεί TCP (ip.proto = 6) στη θύρα 21 για το control connection. Επιλέγεται TCP γιατί είναι connection-oriented και αξιόπιστο (reliable): εγγυάται την παράδοση δεδομένων με μηχανισμούς acknowledgment και retransmission — απαραίτητο για τη μεταφορά αρχείων χωρίς απώλειες.

---

### Ερώτηση 9: Bytes που ελήφθησαν από τον FTP server 172.16.137.53

**Φίλτρο Wireshark:** `ip.src == 172.16.137.53 && tcp.srcport == 21` (ή Statistics → Conversations)

**Απάντηση:** **1012 bytes** (10 frames) ελήφθησαν από τον FTP server.

> **[SCREENSHOT Δ.9]** — Statistics → Conversations → TCP, γραμμή 172.16.137.53:21 (στήλη "←" = 1012 bytes)

**Επεξήγηση:** Στην καρτέλα Conversations, η σύνδεση 192.168.2.9:54287 ↔ 172.16.137.53:21 δείχνει 1012 bytes στην κατεύθυνση από τον server προς τον client (στήλη "←"). Αυτά είναι τα bytes που ελήφθησαν από τον FTP server (banner, μηνύματα, response codes).

---

### Ερώτηση 10: Άλλο Application Layer protocol (εκτός SMTP) για email

**Φίλτρο Wireshark:** `pop`

**Απάντηση:** **POP3** (Post Office Protocol v3), στη θύρα 110.

> **[SCREENSHOT Δ.10]** — Frame 1850 (POP3 command, port 110)

**Επεξήγηση:** Εκτός από το SMTP (που χρησιμοποιείται για αποστολή email), στην καταγραφή εντοπίστηκε και το πρωτόκολλο **POP3** στη θύρα 110 (60 frames). Το POP3 χρησιμοποιείται για την ανάκτηση/λήψη email από τον mail server προς τον client. Δεν εντοπίστηκε IMAP (0 frames).

---

## Βιβλιογραφία

1. Kurose, J. F., & Ross, K. W. (2021). *Computer Networking: A Top-Down Approach* (8th ed.). Pearson.
2. Cisco Networking Academy. (2023). *CCNA: Introduction to Networks*. Cisco Press.
3. Stallings, W. (2017). *Network Security Essentials: Applications and Standards* (6th ed.). Pearson.
4. Wireshark Foundation. (2024). *Wireshark User's Guide*. Διαθέσιμο στο: https://www.wireshark.org/docs/
