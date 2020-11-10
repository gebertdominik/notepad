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

