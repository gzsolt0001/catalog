# catalog

Automatikusan generált katalógus repository.

## Repository célja

A(z) `catalog` repository az ÁNYK kivezetéséhez kapcsolódó új NAV M2M 4.0 bizonylatkezelési modell **űrlaptípus- és verziókatalógusát** tartalmazza.

Feladata:
- az NAV M2M 4.0 API-n keresztül benyújtható űrlaptípusok nyilvántartása,
- az egyes űrlaptípusok elérhető verzióinak közzététele,
- a verziók benyújthatósági és érvényességi időszakainak publikálása,
- valamint a tiltott/kivezetett verziók jelölése.

Az XML és JSON formátumban is elérhető katalógusállományok az NAV M2M 4.0 API-ján benyújtható űrlaptípusok és azok verzióinak technikai metaadatait írják le.

A katalógus meghatározza, hogy egy adott űrlaptípus mely verziói érhetők el, milyen időszakban tekinthetők érvényesnek és benyújthatónak, illetve mely verziók kerültek tiltásra vagy kivezetésre.



## Katalógus frissítése

A katalógusállományok tartalma az űrlapspecifikus repository-k publikációs állapotát követi.

A katalógus frissülhet:
- új űrlaptípus publikálásakor,
- új, még nem benyújtható űrlapverzió **előzetes** publikálásakor,
- meglévő űrlaptípus új verziójának közzétételekor,
- valamint egy már nyilvántartott űrlapverzió érvényességi vagy tiltási metaadatainak módosításakor.

A katalógus aktuális állapota az NAV M2M 4.0 rendszerben nyilvántartott űrlaptípusok és űrlapverziók legutóbb publikált metaadat-állapotát tükrözi.

### Mit lát a felhasználó az űrlapspecifikus repository-kban?

Az űrlapspecifikus repository-k célja, hogy egyszerre biztosítsák az aktuálisan használható verzió egyszerű elérését,
valamint a korábban — akár jövőbeli érvényességi kezdettel — publikált verziók teljes visszakereshetőségét.
Ennek érdekében a repository-k különböző szerepű brancheket és tageket használnak.

Az egyes elemek szerepe:

- main: technikai branch, amely a generálási és szinkronizálási folyamatok alap branch-e,
- `actual`: **nézeti** branch, amely az aktuálisan érvényes és nem tiltott verzió gyors elérését biztosítja,
- tags: az összes publikált verzió (release) visszakereshető jelölései.

Az `actual` branch mindig arra a verzióra mutat, amely az adott űrlaptípus esetén aktuálisan érvényes, nincs tiltva, és a legmagasabb verziónak számít.

### Mi számít aktuális verziónak?

Egy verzió akkor tekinthető aktuális (valid) verziónak, ha:
- az érvényesség kezdete nem a jövőben van,
- az érvényesség vége üres vagy még nem járt le,
- a verzió nincs tiltva (`disabled=false`),
- és ezek közül a legmagasabb verziószámú.

Megjegyzés: az aktuális verzió meghatározásának forrása a `catalog` repository-ban lévő metaadat.

---

## Kontextus

Az ÁNYK kivezetéséhez kapcsolódó NAV M2M 4.0 fejlesztések célja, hogy az ügyviteli szoftverek számára szabványosabb, automatizálhatóbb és könnyebben feldolgozható űrlapkezelési megoldás álljon rendelkezésre.

Az új NAV M2M bizonylat API-ban az űrlapok adatszerkezete XML alapon, űrlaponként saját XSD állományban kerülnek leírásra. Ezek az űrlapspecifikus XSD-k egységes szerkezeti elveket követnek, és közös elemekre, típusokra, enumerációkra, valamint metaséma-elemekre támaszkodhatnak.

A `catalog` repository az űrlapspecifikus repository-khoz kapcsolódó verzió- és validitási metaadatokat biztosítja.


### A `catalog` repository szerepe az architektúrában

Az új generációs bizonylatkezelési architektúra három fő repository-típusra épül:

| Repository | Szerep |
| --- | --- |
| `common` | közös sémák, metasémák, enumerációk |
| űrlapspecifikus repository-k | konkrét űrlapleírók |
| `catalog` | verzió- és érvényességi katalógus |

A `catalog` repository az űrlaptípusok verzió- és validitási metaadatainak központi nyilvántartásaként működik.


### Kapcsolat az űrlapspecifikus repository-kkal

Az űrlapspecifikus repository-k tartalmazzák a konkrét űrlapleírókat és technikai kiegészítő állományokat.

A `catalog` repository ezekhez biztosít:

- verziónyilvántartást,
- érvényességi információkat,
- verziók tiltási állapotát.

---

## Repository felépítése, állományai

A repository fő tartalmai a `content/` könyvtárban találhatók.

```text
content/
├── artifact-catalog.json
├── artifact-catalog.xml
├── artifact-catalog.xsd
├── artifact-catalog.schema.json
└── ...
```

## A főbb fájlok szerepe

| Fájlnév | Szerep |
| --- | --- |
| `artifact-catalog.json` | Az űrlapkatalógus JSON reprezentációja. |
| `artifact-catalog.xml` | Az űrlapkatalógus XML reprezentációja. |
| `artifact-catalog.xsd` | Az XML katalógus XSD sémája. |
| `artifact-catalog.schema.json` | A JSON katalógus JSON séma definíciója. |



## Katalógus mezők jelentése

| Mező | Jelentés |
| --- | --- |
| `baseInformation` | A katalógus egészére vonatkozó technikai metaadatokat tartalmazó blokk. |
| `catalogSchemaVersion` | A katalógusállomány sémaverziója. |
| `generatedAt` | A katalógus generálásának időpontja. |
| `formsInformation` | Az űrlaptípusokat tartalmazó lista. |
| `formId` | Az űrlaptípus technikai azonosítója. |
| `formName` | Az űrlaptípus megnevezése. |
| `versions` | Az adott űrlaptípushoz tartozó verziók listája. Egy űrlaptípushoz több verzió is tartozhat, eltérő érvényességi időszakokkal és tiltási állapottal. |
| `formVersion` | Az adott űrlaptípus egy konkrét verziójának azonosítója. |
| `validFrom` | Az adott verzió érvényességének kezdete. |
| `validTo` | Az adott verzió érvényességének vége. |
| `disabled` | Tiltási jelző. `true` esetén a verzió kivezetésre került. |

A `versions` tömb teszi lehetővé, hogy ugyanazon űrlaptípus több párhuzamosan nyilvántartott verzióval rendelkezzen.

Az egyes verziók önálló érvényességi időszakkal és tiltási állapottal rendelkeznek, ezért a benyújthatósági döntés mindig egy konkrét `formId` + `formVersion` kombinációra értelmezendő.


### Időkezelési elvek

Az időkezelés célja, hogy az űrlapverziók felhasználhatósága determinisztikus módon, egyértelmű szabályok alapján legyen meghatározható.

Alapelvek:

* minden időadat ISO-8601 formátumban, Europe/Budapest időzóna szerinti dinamikus offsettel (`+01:00` / `+02:00`) kerül publikálásra,
* az érvényességi időadatok perc pontosságúak,
* az érvényesség kezdete *inkluzív* (`validFrom` időpontja már része az érvényes időszaknak),
* az érvényesség vége *exkluzív* (`validTo` időpontja már nem része az érvényes időszaknak),
* hiányzó `validFrom` esetén a verzió azonnal érvényesnek tekintendő,
* hiányzó `validTo` esetén a verzió határozatlan ideig érvényesnek tekintendő,
* az érvényesség vége nem lehet korábbi, mint az érvényesség kezdete,
* ugyanazon űrlaptípus azonos verziója csak egyszer szerepelhet a katalógusban.

#### Egy űrlapverzió időbeli érvényességi feltétele

Egy űrlapverzió időbeli szempontból akkor tekinthető érvényesnek, ha teljesül a következő feltétel:
- ha mindkét dátumadat meg van adva:

```sql
validFrom <= now < validTo
```

- ha valamelyik dátumadat hiányzik:

```sql
(validFrom IS NULL OR validFrom <= now)
AND
(validTo IS NULL OR now < validTo)
```


---

### Űrlapverzió tiltása

Ha egy űrlapverzió expliciten tiltott (`disabled = true`), akkor az adott verzió nem tekinthető beadhatónak:
az adott verzió benyújtása akkor is visszautasításra kerül, ha az érvényességi időintervalluma alapján egyébként még használható lenne.

---

## Benyújthatósági feltételek (validitás)

Egy adott időpillanatban ugyanazon űrlaptípus több időben érvényes verziója is létezhet, azonban benyújtás szempontjából a legmagasabb **valid** verzió tekintendő elsődlegesnek.

Egy űrlaptípus egy adott verziója akkor tekinthető validnak (benyújtás szempontjából használhatónak), ha:

1. a verzió nincs tiltva (`disabled = false`),
2. a verzió időbeli érvényességi feltétele teljesül,
3. az adott időpillanatban ez az űrlaptípus legmagasabb valid verziója.

Ez a modell lehetővé teszi, hogy:

* időben előre publikált, de még nem aktív űrlapverziók is elérhetők legyenek,
* a lejárt verziók automatikusan kikerüljenek a használatból,
* hibás verziók explicit tiltással kizárhatók legyenek.

### Kompatibilitási megjegyzés

A NAV M2M 4.0 API technikailag nem gátolja az olyan űrlapverziók benyújtását is, amelyek explicit módon nem tiltottak és még nem jártak le, azonban normál működés során az adott időpillanatban validnak tekintett legmagasabb verzió használata javasolt.


A katalógus validitási szabályainak célja annak biztosítása, hogy az ügyviteli rendszerek egységesen és determinisztikus módon tudják kiválasztani az aktuálisan használható űrlapverziót.


### Validitási példák

| Verzió | validFrom | validTo | disabled | Állapot |
| --- | --- | --- | --- | --- |
| `0.20` | múltbeli | jövőbeli | `false` | valid |
| `0.21` | jövőbeli | üres | `false` | még nem valid |
| `0.18` | múltbeli | múltbeli | `false` | lejárt |
| `0.19` | múltbeli | jövőbeli | `true` | tiltott |


Az alábbi példák egy tipikus katalógusbejegyzés JSON és XML reprezentációját mutatják be.

#### JSON példa

```json
{
  "formId": "NAV_PMT25",
  "formName": "PMT25 - Bejelentés ...",
  "versions": [
    {
      "formVersion": "0.20",
      "validFrom": "2025-06-20T00:00:00+02:00",
      "validTo": "2026-01-01T00:00:00+01:00",
      "disabled": false
    },
    {
      "formVersion": "0.21",
      "validFrom": "2026-01-01T00:00:00+01:00",
      "validTo": "",
      "disabled": false
    }
  ]
}
```


#### XML példa

```xml
<form>
    <formId>NAV_PMT25</formId>
    <formName>PMT25 - Bejelentés ...</formName>
    <versions>
        <version>
            <formVersion>0.20</formVersion>
          <validFrom>2025-06-20T00:00:00+02:00</validFrom>
          <validTo>2026-01-01T00:00:00+01:00</validTo>
            <disabled>false</disabled>
        </version>
    </versions>
</form>
```



## Séma validáció

A repository-ban található:

- JSON állományok JSON Schema alapján,
- XML állományok XSD alapján

validálhatók.

---

## Kapcsolódó dokumentációk

Az új M2M bizonylatkezelési modellhez és az ÁNYK kivezetéséhez kapcsolódó háttéranyagok:

- [ÁNYK kivezetéshez kapcsolódó M2M fejlesztések](https://github.com/nav-gov-hu/M2M/wiki/%C3%81NYK-kivezet%C3%A9shez-kapcsol%C3%B3d%C3%B3-M2M-fejleszt%C3%A9sek)
- [Új bizonylat API](https://github.com/nav-gov-hu/M2M/wiki/%C3%9Aj-bizonylat-API)
- [Új bizonylat API: XML formátum](https://github.com/nav-gov-hu/M2M/wiki/%C3%9Aj-bizonylat-API:-XML-form%C3%A1tum)
- [Új bizonylat API: XML validáció](https://github.com/nav-gov-hu/M2M/wiki/%C3%9Aj-bizonylat-API:-XML-valid%C3%A1ci%C3%B3)


