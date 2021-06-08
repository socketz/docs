---
title: 'Skonfiguruj adres IP jako alias'
slug: ip-aliasing-vps
excerpt: 'Dowiedz się, jak dodać adresy IP Failover do konfiguracji VPS'
section: 'Sieć & IP'
---

> [!primary]
> Tłumaczenie zostało wygenerowane automatycznie przez system naszego partnera SYSTRAN. W niektórych przypadkach mogą wystąpić nieprecyzyjne sformułowania, na przykład w tłumaczeniu nazw przycisków lub szczegółów technicznych. W przypadku jakichkolwiek wątpliwości zalecamy zapoznanie się z angielską/francuską wersją przewodnika. Jeśli chcesz przyczynić się do ulepszenia tłumaczenia, kliknij przycisk „Zaproponuj zmianę” na tej stronie.
> 

**Ostatnia aktualizacja z dnia 27-04-2021**

## Wprowadzenie

Alias IP (*IP aliasing* w języku angielskim) to specjalna konfiguracja sieci dla serwerów OVHcloud, która pozwala na przypisanie kilku adresów IP do jednego interfejsu sieciowego.

**Niniejszy przewodnik wyjaśnia, jak dodawać adresy IP Failover do Twojej konfiguracji sieci.**

> [!warning]
>
> OVHcloud oddaje do Twojej dyspozycji usługi, za które przejmujesz odpowiedzialność. Firma OVH nie ma dostępu do Twoich serwerów, nie pełni funkcji administratora i w związku z tym nie będzie mogła udzielić Ci wsparcia. Zarządzanie oprogramowaniem i wdrażanie środków bezpieczeństwa należy do klienta.
>
> Oddajemy w Twoje ręce przewodnik, którego celem jest pomoc w jak najbardziej optymalnym wykonywaniu bieżących zadań. W przypadku problemów z administrowaniem, użytkowaniem czy zabezpieczeniem serwera rekomendujemy skorzystanie z usług wyspecjalizowanej firmy. Więcej informacji znajduje się w sekcji „Sprawdź również”.
>

## Wymagania początkowe

- jednego [VPS](https://www.ovhcloud.com/pl/vps/) na koncie OVHcloud
- adresu [IP Failover](https://www.ovhcloud.com/pl/bare-metal/ip/) lub bloku IP Failover
- dostęp administratora (root) przez SSH lub GUI do serwera
- podstawowa wiedza o sieciach i ich administrowaniu

## W praktyce

Niniejszy przewodnik zawiera najpopularniejsze konfiguracje dystrybucji/systemów operacyjnych. Pierwszy etap polega zawsze na logowaniu się do serwera przez SSH lub przez sesję logowania do interfejsu graficznego użytkownika (RDP dla serwera VPS Windows). Poniższe przykłady zakładają, że jesteś zalogowany jako użytkownik z dużymi uprawnieniami (Administrator/sudo).

> [!primary]
>
Jeśli chodzi o różne wersje dystrybucji, należy pamiętać, że można zmodyfikować odpowiednią procedurę konfiguracji Twojego interfejsu sieciowego oraz nazw plików. W przypadku trudności zalecamy zapoznanie się z dokumentacją dotyczącą systemu operacyjnego.
>

**Należy wziąć pod uwagę następującą terminologię, która zostanie użyta w przykładach kodu i instrukcjach zawartych w tym przewodniku:**

|Nazwa|Opis|Przykłady|
|---|---|---|
|IP_FAILOVER|Adres IP Failover przypisany do Twojej usługi|169.254.10.254|
|NETWORK_INTERFACE|Nazwa interfejsu sieciowego|*eth0*, *ens3*|
|ID|ID aliasu IP, zaczynające się od *0* (w zależności od liczby dodatkowych adresów IP do skonfigurowania)|*0*, *1*|

### Debian 10

#### Etap 1: wyłącz automatyczną konfigurację sieci

Otwórz ścieżkę dostępu do następującego pliku z edytorem tekstu:

```bash
sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

Wprowadź następującą linię, następnie zapisz i wyjdź z edytora.

```bash
network: {config: disabled}
```

Utworzenie tego pliku konfiguracyjnego zapobiega automatycznemu wprowadzaniu zmian w konfiguracji Twojej sieci.

#### Etap 2: zmień plik konfiguracyjny sieci

Nazwy interfejsu sieciowego możesz sprawdzić za pomocą polecenia:

```bash
ip a
```

Otwórz plik konfiguracyjny sieci, aby go zmienić za pomocą następującego polecenia:

```bash
sudo nano /etc/network/interfaces.d/50-cloud-init
```

Następnie dodaj następujące wiersze:

```bash
auto NETWORK_INTERFACE:ID
iface NETWORK_INTERFACE:ID inet static
address IP_FAILOVER
netmask 255.255.255.255
```

#### Etap 3: uruchom ponownie interfejs

Zastosuj zmiany za pomocą polecenia:

```bash
sudo systemctl restart networking
```

### Ubuntu 20.04

Plik konfiguracyjny adresów IP Failover znajduje się w katalogu `/etc/netplan/`. W tym przykładzie nosi nazwę "50-cloud-init.yaml". Zanim wprowadzisz zmiany, sprawdź w tym folderze nazwę rzeczywistego pliku. Każdy adres IP Failover wymaga własnej linii w pliku.

#### Etap 1: wyłącz automatyczną konfigurację sieci

Otwórz ścieżkę dostępu do następującego pliku z edytorem tekstu:

```bash
sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

Wprowadź następującą linię, następnie zapisz i wyjdź z edytora.

```bash
network: {config: disabled}
```

Utworzenie tego pliku konfiguracyjnego zapobiega automatycznemu wprowadzaniu zmian w konfiguracji Twojej sieci.

#### Etap 2: zmień plik konfiguracyjny

Nazwy interfejsu sieciowego możesz sprawdzić za pomocą polecenia:

```bash
ip a
```

Otwórz plik konfiguracyjny sieci, aby go zmienić za pomocą następującego polecenia:

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Nie zmieniaj istniejących linii w pliku. Dodaj adres IP Failover, postępując zgodnie z poniższym przykładem:

```yaml
network:
    version: 2
    ethernets:
        NETWORK_INTERFACE:
            dhcp4: true
            match:
                macaddress: fa:xx:xx:xx:xx:63
            set-name: NETWORK_INTERFACE
            addresses:
            - IP_FAILOVER/32
```

> [!warning]
>
> Ważne jest przestrzeganie wyrównania każdego elementu tego pliku, jak pokazano w powyższym przykładzie. Nie używaj przycisku tabulacji do tworzenia odstępów.
>

Zapisz i zamknij plik.

#### Etap 3: zastosować nową konfigurację sieci

Możesz przetestować konfigurację za pomocą polecenia:

```bash
sudo netplan try
```

Jeśli jest poprawna, zastosuj ją za pomocą następującego polecenia:

```bash
sudo netplan apply
```

Powtórz tę procedurę dla każdego adresu IP Failover.

### Windows Server 2016

#### Etap 1: sprawdź konfigurację sieci

Kliknij prawym przyciskiem myszy przycisk `Menu Start`{.action} i otwórz `Uruchom`{.action}.

Wpisz `cmd` i kliknij `OK`{.action}, aby otworzyć aplikację wiersza poleceń.

![cmdprompt](images/vps_win07.png){.thumbnail}

Aby pobrać aktualną konfigurację IP, wprowadź `ipconfig` w wierszu poleceń.

![sprawdź główną konfigurację IP](images/image1-1.png){.thumbnail}

#### Etap 2: zmień właściwości IPv4

Teraz zmodyfikuj właściwości IP w konfigurację statyczną.

Otwórz parametry adaptera w Panelu konfiguracyjnym Windows, a następnie otwórz `Właściwości`{.action} protokołu `Internet Protocol Version 4 (TCP/IPv4)`{.action}.

![zmień konfigurację IP](images/image2.png){.thumbnail}

W oknie Właściwości IPv4 wybierz `Użyj następującego`{.action} adresu IP. Wpisz adres IP, który otrzymałeś w pierwszym etapie, po czym kliknij `Zaawansowane`{.action}.

#### Etap 3: dodać adres IP Failover do zaawansowanych ustawień TCP/IP

W nowym oknie kliknij `Dodaj...`{.action} pod "Adresy IP". Wpisz adres IP Failover i maskę podsieci (255.255.255.255).

![sekcja konfiguracji zaawansowanej](images/image4-4.png){.thumbnail}

Potwierdź klikając `Dodaj`{.action}.

![Konfiguracja migracji IP](images/image5-5.png){.thumbnail}

#### Etap 4: uruchom ponownie interfejs sieciowy

Wróć do panelu konfiguracyjnego (`Połączenia sieciowe`{.action}), kliknij prawym przyciskiem myszy interfejs sieciowy, a następnie wybierz `Wyłącz`{.action}.

![dezaktywacja sieci](images/image6.png){.thumbnail}

Aby go ponownie uruchomić, kliknij prawym przyciskiem myszy, a następnie wybierz `Aktywuj`{.action}.

![aktywacja sieci](images/image7.png){.thumbnail}

#### Etap 5: sprawdź nową konfigurację sieci

Otwórz wiersz poleceń (cmd) i wprowadź `ipconfig`. Konfiguracja musi teraz zawierać nowy adres IP Failover.

![sprawdź aktualną konfigurację sieci](images/image8-8.png){.thumbnail}

### cPanel (CentOS 7) / pochodne Red Hat

#### Etap 1: zmień plik konfiguracyjny sieci

Nazwy interfejsu sieciowego możesz sprawdzić za pomocą polecenia:

```bash
ip a
```

Otwórz plik konfiguracyjny sieci, aby go zmienić:

```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-NETWORK_INTERFACE:ID
```

Dodaj te linie:

```bash
DEVICE=NETWORK_INTERFACE:ID
BOOTPROTO=static
IPADDR=IP_FAILOVER
NETMASK=255.255.255.255
BROADCAST=IP_FAILOVER
ONBOOT=yes
```

#### Etap 2: uruchom ponownie interfejs

Zastosuj zmiany za pomocą polecenia:

```bash
sudo systemctl restart networking
```

### Plesk

#### Etap 1: dostęp do interfejsu zarządzania adresami IP Plesk

W panelu konfiguracyjnym Plesk wybierz `Tools & Settings`{.action} na pasku bocznym po lewej stronie.

![dostęp do zarządzania adresami IP](images/pleskip1.png){.thumbnail}

Kliknij `IP Addresses`{.action} w **Tools & Settings**.

#### Etap 2: dodaj dodatkowe informacje IP

W tej sekcji kliknij przycisk `Add IP Address`{.action}.

![dodaj informacje IP](images/pleskip2-2.png){.thumbnail}

Wprowadź adres IP Failover w formie `xxx.xxx.xxx.xxx/32` w polu "IP address and subnet mask", a następnie kliknij `OK`{.action}.

![dodaj informacje IP](images/pleskip3-3.png){.thumbnail}

#### Etap 3: sprawdź aktualną konfigurację IP

W sekcji "IP Addresses" sprawdź, czy adres IP Failover został poprawnie dodany.

![aktualna konfiguracja IP](images/pleskip4-4.png){.thumbnail}

### Diagnostyka

Po pierwsze, zrestartuj serwer za pomocą wiersza poleceń lub interfejsu użytkownika. Jeśli nadal nie udaje Ci się nawiązać połączenia między siecią publiczną a adresem IP aliasu i podejrzewasz problem z siecią, zrestartuj serwer w [trybie rescue]((../rescue/). Następnie możesz skonfigurować adres IP Failover bezpośrednio na serwerze.

Po zalogowaniu się do serwera przez SSH wprowadź następującą komendę:

```bash
ifconfig ens3:0 IP_FAILOVER netmask 255.255.255.255 broadcast IP_FAILOVER up
```

Aby przetestować połączenie, wystarczy wysłać ping na adres IP Failover z zewnątrz. Jeśli odpowiada w trybie Rescue, prawdopodobnie oznacza to, że wystąpił błąd w konfiguracji. Jeśli jednak adres IP nadal nie działa, poinformuj o tym zespół pomocy technicznej OVHcloud, wysyłając zgłoszenie serwisowe w [Panelu client OVHcloud](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.pl/&ovhSubsidiary=pl).
 
## Sprawdź również

[Włącz tryb Rescue na serwerze VPS](../rescue/)

Przyłącz się do społeczności naszych użytkowników na stronie <https://community.ovh.com/en/>.
