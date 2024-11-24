# Instrukcja obsługi
## Instalacja
### Zainstaluj dockera
Zainstaluj najnowszą wersję dockera zgodnie z twoją wersją systemu operacyjnego.

**UWAGA**  Na linuxie musisz dodać grupę do swojego użytkownika komendą
`sudo usermod -aG docker $USER ` i potem `newgrp docker`. 

### Sklonuj repozytoria
Wczytaj folder, w którym chcesz pracować. Używając `git clone` sklonuj 
wszystkie repozytoria.** Repozytoria muszą być równloegle do siebie**, 
oznacza to taką strukturę folderów:
```
- mój folder
	- Infra
	- Frontend
	- Backend
```
Na tym etapie możesz zrobić test działania stawiając środowisko z plików 
dockera, wejdź do katalogu `Infra` i użyj komendy `docker-compose -f docker.compose.init.yml up`.

### Zaloguj się do ghcr
Zalogowanie do ghcr jest konieczne aby móc pobierać obrazy kontenerów z
rejestru.

#### Utwórz token github
Wejdź na swoje konto github. Następnie klinkij na swoje zdjęcie profilowe -> 
ustawienia -> ustawienia programisty/dewelopera -> personalny token dostępu
-> token klasyczny.
W prawym górnym rogu wciśnij wygeneruj nowy token -> nowy token klasyczny. Tam zaznacz opcje repo oraz te dotyczące czytania, usuwania i dodawania paczek.
Wygeneruj token i zapisz go.

Teraz zaloguj się do ghcr w dockerze za pomocą komendy `echo "<token>" | docker login ghcr.io -u <nazwa użytkownika>  --password-stdin` (ta komenda działa na linuxie być moze trzeba znaleźć ekwiwalent na windowsie).

Powinno działać, spróbuj postawić wersję deweloperską dla frontendu wchodząc
do katalogu Infra i używając komendy `docker-compose up`.

## Korzystanie
### Frontend
Korzystanie z tego systemu budowania właściwie nie wymaga od ciebie instalacji
repozytoriów innych niż frontend. Warto pilnować aby repozytorium Infra było 
cały czas aktualne na twoim komputerze.

Wchodzimy do katalogu Infra i używamy komendy `docker-compose up`. 
Frontend odpali sie z kodu na twoim komputerze w trybie hot reload -- zmiany w kodzie są natychmiastowo odzwierciedlane. Hot reload ma pewne limity, jeżeli
na nie natraficie wyłączcie kontener i włączcie go znowu z opcją `docker-compose up --build`.
Ponieważ coś sie psuje i u mnie nie mogłem odpalić frontendu na porcie 3000, macie do niego dostęp na porcie 3001 (localhost:3001).

Dostęp do api z frontendu macie za pomocą stałej:
`process.env.REACT_APP_API_URL`.
Polecam zrobić sobie po prostu `const apiUrl = process.env.REACT_APP_API_URL;`. Na razie nie przewidziałem żebyście mieli dostęp do api w jakiś inny sposób, je uznacie to za potrzebne mogę to zrobić.

#### UWAGA
Jeżeli zdecydujecie się dodać do waszego projektu coś co ma swój własny system budowania np. tailwind, POWIEDZCIE MI BO TRZEBA BĘDZIE ZMENIĆ DOCKERFILE, nie ruszajcie plików dockera oraz katalogu `.github`.

### Backend
Ogólnie to samo (chyba) z tym, że odpalamy `docker-compose -f 
dockerfile.api.yml up`, tutaj też działa hot reload i jeżeli się popsuje to robimy 
`docker-compose -f dockerfile.api.yml up --build`.  Nie planuje podpinania 
serwisów do portów hosta a komunikowanie się znimi bezpośrednio przez API, 
jeżeli okaże się to niewystarczające to dodam kolejne pliki dockera. Api działa na 
porcie 5000 (localhost:5000), swagger ze zmapowanymi endpointami znajduje 
sie pod http://localhost:5000/swagger/index.html.