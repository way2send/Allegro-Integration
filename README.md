<!-- Language-neutral identification -->
> **Application identification / Identyfikacja aplikacji:** `Way2Send - Integracja`

> **User-Agent:** `Way2Send-Integracja/<version> (+https://github.com/way2send/Allegro-Integration)`

**🇬🇧 English** · [🇵🇱 Polski](#polski)

---

<a id="english"></a>

# Way2Send — Allegro Integration

This repository is the public information page for the Way2Send integration with the
Allegro REST API. It exists so that Allegro administrators reviewing server logs can
identify the source of API traffic, understand the application's purpose, and reach the
application owner directly. It is the URL referenced in the application's `User-Agent`
header, as required by [Allegro REST API Terms & Conditions, §3.4(c)](https://developer.allegro.pl/rules).

## What this application does

Way2Send is an order management system (OMS) for e-commerce. This integration lets a
seller connect their Allegro account to Way2Send so that orders, offers, and shipments
are handled automatically across the seller's own warehouse (or fulfilment centre) and
their ERP/accounting system.

The integration is responsible for:

- **Order intake** — downloading Allegro orders so they can be fulfilled by the seller's
  own warehouse or chosen fulfilment centre, and so that accounting processes can run on
  them (for example, issuing sale documents in the seller's ERP system).
- **Order status updates** — pushing order status changes back to Allegro based on the
  fulfilment stage reached in the warehouse.
- **Stock synchronisation** — updating offer stock levels on Allegro to reflect the actual
  on-hand quantities in the seller's WMS.
- **Allegro Delivery (Wysyłam z Allegro / WzA)** — registering shipments for orders
  through Wysyłam z Allegro.
- **Shipment tracking** — tracking the status of shipments registered via Wysyłam z
  Allegro.
- **Tracking numbers** — uploading tracking numbers to Allegro for orders shipped through
  the seller's direct courier integrations (outside WzA).
- **Document upload** — uploading sale documents (e.g. invoices) issued in the seller's
  ERP system back to the corresponding Allegro orders.

All actions are performed on behalf of the seller, who explicitly authorises the
application through Allegro's OAuth consent flow.

## OAuth scopes used

The application requests only the scopes required to perform the functions above. Each
scope is listed below with the feature that depends on it.

| Scope | Allegro description | Why the application needs it |
| --- | --- | --- |
| `allegro:api:sale:offers:read` | Odczyt danych o ofertach | Read current offer data (incl. stock) to determine what needs to be synchronised from the WMS. |
| `allegro:api:sale:offers:write` | Tworzenie, edycja, łączenie i zamykanie ofert | Update offer stock levels to match actual quantities in the seller's WMS. |
| `allegro:api:orders:read` | Odczyt informacji o zamówieniach | Download orders for fulfilment and for downstream accounting processes. |
| `allegro:api:orders:write` | Zarządzanie zamówieniami | Update order statuses, attach tracking numbers from direct courier integrations, and upload ERP-issued sale documents. |
| `allegro:api:shipments:read` | Odczyt przesyłek, etykiet i protokołów | Track the status of shipments registered via Wysyłam z Allegro. |
| `allegro:api:shipments:write` | Zarządzanie przesyłkami | Register shipments for orders through Wysyłam z Allegro. |

## How it works (high level)

1. The seller authorises the application via the Allegro OAuth authorization-code flow.
2. The application periodically retrieves new and updated orders and imports them into the
   seller's Way2Send OMS.
3. As the warehouse processes each order, the integration updates the order status on
   Allegro and synchronises offer stock from the WMS.
4. For shipping, the integration either registers a shipment through Wysyłam z Allegro and
   tracks it, or attaches a tracking number obtained from the seller's direct courier
   integration.
5. Sale documents issued in the seller's ERP system are uploaded to the matching Allegro
   orders.

No Allegro data is used for any purpose other than fulfilling the seller's own orders and
keeping their offers, orders, shipments, and documents in sync with Way2Send.

## User-Agent

In line with [Allegro's User-Agent requirement](https://github.com/allegro/allegro-api/issues/13126),
every request this application sends to the Allegro REST API includes a custom
`User-Agent` header in the form:

```
Way2Send-Integracja/<version> (+https://github.com/way2send/allegro-integration)
```

This header is not modified between requests, as Allegro uses it as a whitelisting factor.
The application name matches the name of the registered application in the
[Allegro developer portal](https://apps.developer.allegro.pl/), without spaces.

## Application owner / contact

This application is operated by **Way2Send**.

- **Website:** https://way2send.pl
- **Technical contact:** integracje@way2send.pl
- **Abuse / monitoring contact:** support@way2send.pl

Allegro administrators who need to reach the application owner regarding API traffic can
use the contacts above.

---

<a id="polski"></a>

[🇬🇧 English](#english) · **🇵🇱 Polski**

# Way2Send — Integracja z Allegro

To repozytorium jest publiczną stroną informacyjną integracji Way2Send z interfejsem
Allegro REST API. Powstało po to, aby administratorzy Allegro analizujący logi serwerowe
mogli zidentyfikować źródło ruchu API, poznać cel działania aplikacji oraz bezpośrednio
skontaktować się z jej właścicielem. To adres URL wskazywany w nagłówku `User-Agent`
aplikacji, zgodnie z [Regulaminem REST API Allegro, §3.4(c)](https://developer.allegro.pl/rules).

## Co robi ta aplikacja

Way2Send to system zarządzania zamówieniami (OMS) dla e-commerce. Ta integracja umożliwia
sprzedającemu połączenie konta Allegro z systemem Way2Send, dzięki czemu zamówienia,
oferty i przesyłki są obsługiwane automatycznie — w magazynie własnym sprzedającego (lub
w wybranym centrum fulfilmentu) oraz w jego systemie ERP/księgowym.

Integracja odpowiada za:

- **Pobieranie zamówień** — pobieranie zamówień z Allegro w celu ich realizacji w magazynie
  własnym sprzedającego lub w wybranym centrum fulfilmentu, a także na potrzeby procesów
  księgowych (np. wystawienia dokumentów sprzedaży w systemie ERP sprzedającego).
- **Aktualizację statusów zamówień** — przekazywanie zmian statusów zamówień do Allegro na
  podstawie etapu realizacji osiągniętego w magazynie.
- **Synchronizację stanów magazynowych** — aktualizację stanów magazynowych ofert na
  Allegro tak, aby odzwierciedlały rzeczywiste stany w systemie WMS sprzedającego.
- **Wysyłam z Allegro (WzA)** — rejestrowanie przesyłek dla zamówień za pośrednictwem
  Wysyłam z Allegro.
- **Śledzenie przesyłek** — śledzenie statusu przesyłek zarejestrowanych przez Wysyłam
  z Allegro.
- **Numery przesyłek** — przekazywanie do Allegro numerów przesyłek (tracking) dla zamówień
  nadanych przez bezpośrednie integracje sprzedającego z kurierami (poza WzA).
- **Przekazywanie dokumentów** — przekazywanie dokumentów sprzedaży (np. faktur)
  wystawionych w systemie ERP sprzedającego do odpowiadających im zamówień na Allegro.

Wszystkie operacje są wykonywane w imieniu sprzedającego, który wyraźnie autoryzuje
aplikację w procesie zgody OAuth Allegro.

## Wykorzystywane zakresy OAuth

Aplikacja prosi wyłącznie o zakresy niezbędne do realizacji powyższych funkcji. Poniżej
każdy zakres opisano wraz z funkcją, która go wymaga.

| Zakres | Opis Allegro | Dlaczego aplikacja go potrzebuje |
| --- | --- | --- |
| `allegro:api:sale:offers:read` | Odczyt danych o ofertach | Odczyt aktualnych danych o ofertach (w tym stanów), aby ustalić, co należy zsynchronizować z systemem WMS. |
| `allegro:api:sale:offers:write` | Tworzenie, edycja, łączenie i zamykanie ofert | Aktualizacja stanów magazynowych ofert tak, aby odpowiadały rzeczywistym ilościom w WMS sprzedającego. |
| `allegro:api:orders:read` | Odczyt informacji o zamówieniach | Pobieranie zamówień do realizacji oraz na potrzeby dalszych procesów księgowych. |
| `allegro:api:orders:write` | Zarządzanie zamówieniami | Aktualizacja statusów zamówień, dołączanie numerów przesyłek z bezpośrednich integracji kurierskich oraz przekazywanie dokumentów sprzedaży wystawionych w ERP. |
| `allegro:api:shipments:read` | Odczyt przesyłek, etykiet i protokołów | Śledzenie statusu przesyłek zarejestrowanych przez Wysyłam z Allegro. |
| `allegro:api:shipments:write` | Zarządzanie przesyłkami | Rejestrowanie przesyłek dla zamówień za pośrednictwem Wysyłam z Allegro. |

## Jak to działa (ogólnie)

1. Sprzedający autoryzuje aplikację w przepływie Authorization Code OAuth Allegro.
2. Aplikacja cyklicznie pobiera nowe i zaktualizowane zamówienia oraz importuje je do
   systemu Way2Send sprzedającego.
3. W miarę realizacji każdego zamówienia w magazynie integracja aktualizuje status
   zamówienia na Allegro i synchronizuje stany ofert z systemem WMS.
4. Na etapie wysyłki integracja albo rejestruje przesyłkę przez Wysyłam z Allegro i ją
   śledzi, albo dołącza numer przesyłki uzyskany z bezpośredniej integracji kurierskiej
   sprzedającego.
5. Dokumenty sprzedaży wystawione w systemie ERP sprzedającego są przekazywane do
   odpowiadających im zamówień na Allegro.

Dane z Allegro nie są wykorzystywane do żadnych celów innych niż realizacja zamówień
sprzedającego oraz utrzymanie synchronizacji jego ofert, zamówień, przesyłek i dokumentów
z systemem Way2Send.

## User-Agent

Zgodnie z [wymogiem Allegro dotyczącym nagłówka User-Agent](https://github.com/allegro/allegro-api/issues/13126),
każde żądanie wysyłane przez aplikację do Allegro REST API zawiera niestandardowy nagłówek
`User-Agent` w postaci:

```
Way2Send-Integracja/<version> (+https://github.com/way2send/allegro-integration)
```

Nagłówek nie jest modyfikowany pomiędzy żądaniami, ponieważ Allegro wykorzystuje go jako
czynnik umieszczania na białej liście. Nazwa aplikacji jest zgodna z nazwą zarejestrowanej
aplikacji w [portalu deweloperskim Allegro](https://apps.developer.allegro.pl/), z pominięciem spacji.

## Właściciel aplikacji / kontakt

Aplikacja jest obsługiwana przez **Way2Send**.

- **Strona WWW:** https://way2send.pl
- **Kontakt techniczny:** integracje@way2send.pl
- **Kontakt ds. nadużyć / monitoringu:** support@way2send.pl

Administratorzy Allegro, którzy potrzebują skontaktować się z właścicielem aplikacji
w sprawie ruchu API, mogą skorzystać z powyższych kontaktów.
