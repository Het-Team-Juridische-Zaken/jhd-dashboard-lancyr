# Een dagrapport voor een nieuwe klant maken

Dit repo is het **template** (en tegelijk het dagrapport van Lancyr). Een nieuwe klant krijgt een eigen map en een eigen
GitHub-repo, met dezelfde code en alleen andere configuratie, logo en koppelingen.

## Nieuwe klant
```bash
cp tools/voorbeeld-klant.config.json ../klant.json        # vul id, naam, team, Zoom-lijn, Granola-map, ... in
python3 tools/new_client.py create --config ../klant.json --out ../dagrapport-klant --logo ../logo.svg
cd ../dagrapport-klant && cat CHECKLIST.md               # de stappen: GitHub, Supabase, secrets, cron-job.org
```
`create` zet de code klaar met een lege historie, plaatst het logo, embedt de config in `index.html` en maakt een lokale
git-repo. Wat nog moet worden ingevuld meldt het meteen (of later met `python3 tools/new_client.py check`).

## Wat verschilt per klant (alles staat in `client.config.json`)
Naam en logo, e-mailadressen met toegang (management, domeinen), het team en bijnamen, de Supabase-inlog, de
Granola-map, de Zoom-wachtrij, de Zoho-regio, hoe de doorverwijzing heet, en de bedrijfsregels voor de AI-analyse
(`prompt.bedrijfsregels`). Het dashboard en de scripts lezen dit bestand; niets klantspecifieks staat in de code.

## Nieuwe functies naar alle klanten brengen
Verbeter het dashboard hier, in het template. Daarna per klant:
```bash
python3 tools/new_client.py update ../dagrapport-klant
```
Dat ververst de code, en laat config, logo, data en de weekData-historie van die klant staan. Controleer daarna
`git diff` in de klantmap en push.

## Let op
- Elke klant een **eigen** Supabase-project en een eigen repo; deel geen inlog of data tussen klanten.
- Na een wijziging in `client.config.json`: `python3 tools/new_client.py sync-config`.
- Namen van workflows en bot komen bij `update` opnieuw uit de config; `sync-config` raakt alleen `index.html` aan.
