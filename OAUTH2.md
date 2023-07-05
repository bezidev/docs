# OAUTH2 v BežiApp sistemu

BežiApp od 6. 7. 2023 dalje podpira OAUTH2 z uporabo BežiApp računa. Aplikacije tretjih podnudnikov lahko to izkoristijo, da preverijo, da se na njihovo aplikacijo povezujejo dejanski Bežigrajčani, prav tako pa lahko uporabljajo BežiApp API z uporabo dovoljenj.

## Ustvarjanje nove aplikacije v BežiApp sistemu

![image](https://github.com/bezidev/docs/assets/52399966/54728a8e-5c07-441a-8b22-6e22337af482)

Aplikacije lahko ustvarite v BežiApp-u pod zavihkom `BežiApp za razvijalce`. Kasneje lahko vse te podatke tudi spreminjate. Ko ste vse izpolnili, kliknete `Dodaj aplikacijo`, nakar bi se vam morala v spodnjem spisku vaših aplikacij pojaviti tudi ta. Za nadaljnje korake boste potrebovali enolični identifikator aplikacije (v spodnjem primeru `a106e326-eac5-4cc4-8ebf-24840bf3b85f`).

![image](https://github.com/bezidev/docs/assets/52399966/e9fb30b0-ce9d-4d25-98d5-395b023f98bf)

## Klic na avtorizacijski URL

Iz vaše aplikacije ob kliku na gumb za prijavo z BežiApp računom preusmerite uporabnika na naslednji naslov:

```
https://beziapp.si/oauth2/<vaš enolični identifikator aplikacije>?scope=<vsa zahtevana dovoljenja ločena z vejico>
```

Primer takega URL naslova je lahko nekaj takega:
```
https://beziapp.si/oauth2/a106e326-eac5-4cc4-8ebf-24840bf3b85f?scope=gimsis.timetable,gimsis.absences
```

Ob takem naslovu bi vaša aplikacija morala izgledati nekako tako:

![image](https://github.com/bezidev/docs/assets/52399966/df603709-8bcd-4b8c-8178-3b277e9a1e9b)

### Seznam dovoljenj
Aplikacija lahko z dovoljenjem `x` dela y stvari (`x` - y):

- `gimsis.timetable` - preveri vaš urnik
- `gimsis.absences` - preveri vaše odsotnosti
- `gimsis.gradings` - preveri vaša ocenjevanja
- `gimsis.grades` - preveri vaše ocene
- `gimsis.user.read.usernameemail` - bere vaš elektronski naslov in uporabniško ime za GimSIS/BežiApp račun
- `gimsis.user.read.namesurname` - bere vaše ime in priimek
- `gimsis.user.read.sex` - bere vaš spol
- `gimsis.user.read.role` - bere vaš tip uporabniškega računa v GimSIS-u (učenec, starš ipd.)
- `sharepoint.notifications.read` - bere vaša obvestila
- `sharepoint.notifications.write` - spreminja vaša obvestila (jih označi kot prebrana)
- `lopolis.meals.read` - preveri vaše menije v Lo.Polisu
- `lopolis.meals.write` - spreminja vaše menije v Lo.Polisu
- `lopolis.checkouts.read` - preveri vaše odjave v Lo.Polisu
- `lopolis.checkouts.write` - spreminja vaše odjave v Lo.Polisu
- `notes.read` - bere vse zapiske
- `notes.delete` - briše vaše zapiske
- `notes.write` - ustvarja nove zapiske
- `radio.suggestion.read` - preveri stanje predlogov na šolskem radiu
- `radio.suggestion.write` - ustvarja nove predloge za šolski radio
- `radio.suggestion.delete` - briše vaše predloge za šolski radio
- `radio.suggestion.status.change` - spreminja stanje predloga za šolski radio (na voljo samo administratorjem sistema)
- `tarot.read` - bere vse v tarok sistemu
- `tarot.game.write` - ustvarja igre v tarok sistemu
- `tarot.game.delete` - briše igre v tarok sistemu
- `tarot.contests.write` - ustvarja nova tekmovanja in spreminja obstoječa v tarok sistemu
- `tarot.contests.delete` - briše tekmovanja v tarok sistemu

## Uporabnik potrdi vašo zahtevo

V naslednjem koraku uporabnik potrdi vašo zahtevo, nakar ga stran avtomatično preusmeri na vaš nastavljen preusmeritveni URL (tega ste prej nastavili v BežiApp sistemu ob ustvaritvi aplikacije).

Sistem kot URL query parameter pošlje OAUTH2 avtorizacijski token, ki velja neomejeno - do naslednjega ponovnega zagona Docker kontejnerja BežiApp aplikacije (vse tokene shranjujemo v RAM-u). Takrat se vsi tokeni izničijo in je potrebno ponovno izdati nove. Avtorizacijski token velja samo za določene API-je, katere definirate z dovoljenji (glejte prejšnje poglavje).

API-ji so razvidni iz [izvorne kode backenda](https://github.com/bezidev/BeziAppBackend) za BežiApp sistem.

Torej na kratko - uporabnika strežnik ob prijavi preusmeri na približno tako povezavo:

```
https://mojazelofancyaplikacija.si/oauth2/callback?session=cafdfdgffhlperopwd...
```


## Naslednji koraki

Aplikacijo lahko tudi preverite. Prednost tega je, da bo aplikacija dobila simbol preverjenosti (kljukico poleg imena). Ob vsaki spremembi v sistemu izgubite simbol preverjenosti in morate še enkrat skozi proces verifikacije. V BežiApp sistemu imate navodila, kako doseči, da bo vaša aplikacija preverjena.
