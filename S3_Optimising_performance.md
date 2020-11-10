URL: https://docs.aws.amazon.com/whitepapers/latest/s3-optimizing-performance-best-practices/welcome.html?did=wp_card&trk=wp_card

# Wstęp
- S3 zapewnia przepustowość przynajmniej 3500 PUT/COPY/POST/DELETE i 5500 GET/HEAD requests/second/prefix in a bucket
- nie ma limitów na prefixy w buckecie
- można poprawić wydajność poprzez równoległe wykonywanie operacji odczytu, np. kiedy utworzymy 10 prefixów  można osiągnąć przepustowowść 55 000 requestów GET/HEAD na sekundę
- niektóre aplikacjie wykorzystujące S3 skanują miliony lub miliardy obiektów, co przekłada się na petabajty danych. Aplikacje te osiągają transfer na poziomi 100 Gb/s na pojedynczej instancji
- inne aplikacje wrażliwe na opóźnienia jak np. czaty osiagają stałe opóxnienie(lub opóźnienie dla pierwszego bajtu dla dużych obiektów) rzędu 100-200 milisekund.
- dla opóźnień poniżej 10ms możliwe jest zastosowanie mechanizmu cachowania takiego jak `Amazon CloudFront` lub `Amazon ElastiCache`.
- w przypadku konieczności przesyłania danych na duże odległości należy użyć `Amazon S3 Transfer Acceleration`. Używa on rozmieszczonych po świecie lokalizacji brzeghowych należących do `Cloud Front` do przyspieszenia transportu danych na duże odległości.
- Jeśli używamy szyfrowania po stronie serwera z wykorzystaniem `AWS Key Management Service (SSE-KMS)` to należy zweryfikować limity przepustowości dla wysyłanych żądań w dokumentacji `AWS KMS`

# Wytyczne dotyczące wydajności Amazon S3
## Pomiar wydajności
Chcąc zoptymalizować wydajność należy spojrzeć na wymagania przepustowości sieci, CPU oraz DRAM. W zależności od kombinacji zapotrzebowania na wymienione zasoby warto zdecydować się na odpowiedni typ instancji `Amazon EC2`.
Czasami warto także spojrzeć na czas wyszukiwania na poziomie DNS, opóźnienia i szybkość przesyłania danych za pomocą narzędzi do analizy ruchu HTTP.
## Horyzontalne skalowanie połączeń
Rozłożenie requestów pomiędzy wiele połączeń jest typowym wzorcem projektowym w przypadku horyzontalnego skalowania wydajności. S3 należy traktowa jako bardzo duży system rozproszony, nie jak pojedynczy endpoint w sieci. Najlepszą wydajność można osiągnąć w przypadku wykonywania wielu równoległych żądań, wykorzystując oddzielne połaczenia. S3 nie ma limitu połaczeń do pojedynczego bucketu.
## Wykorzystanie pobierania zakresu bajtów
Wykorzystując nagłówek Range HTTP w zapytaniach typu GET Object można pobrać zakres bajtów z pojedynczego obiektu. Ponadto zapytania takie możemy wykonywać równolegle. Pozwala to osiągnąć wyższą łączną przepustowość w porównaniu do pojdedynczego żądania o cały obiekt. Dodatkowo pobieranie częściowe pozwala także na osiągnięcie znacznie niższego czasu potrzebnego na ponowienie requestu w przypadku awarii - ponawiamy tylko request dla danego zakresu.
Typowe żądanie o zakres bajtów pobiera od 8MB do 16MB danych. Jeżeli wykorzystujemy multipart upload, to rekomendowane jest pobieranie obiektu w takich samych partiach jak był on uploadowany. GET może pobierać dowolną partię obiektu za pomocą GET ?partNumber=N
## Ponawianie żądań dla aplikacji wrażliwych na opóźnenia
Agresywne limity czasu i ponownych prób pozwalają uzyskać stałe czasy opóźnienia. Wykorzystując skalę S3, jeżeli pierwszy request był powolny, to ponowna próba z dużym prawodopodobieństwem zostanie wykonana inną ścieżką i żądanie szybko zostanie zakończone sukcesem. W AWS SDK możemy skonfigurować timeout i limit prób tak by dopasować się do specyfiki naszej aplikacji.
