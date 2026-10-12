# 80BPM Verfügbarkeits-Monitoring

Prüft alle 5 Minuten von außen (GitHub Actions) die Health-Endpunkte von Admin, Portal und Fragebogen. Fällt einer bei zwei Versuchen aus, geht eine Alarm-Mail mit der Entsperr-Anleitung an das Team, bei Rückkehr eine Entwarnung. Das Repo enthält keine Personendaten; Empfänger und Mail-Zugang liegen als Secrets.

## Einrichtung

1. Öffentliches Repo anlegen, diese Dateien hineinlegen (`.github/workflows/uptime.yml`, `README.md`).
2. Microsoft 365: eigene App „80BPM Monitoring“ mit Senderecht **nur** für `info@your80bpm.com` (Entra-App-Registrierung ohne API-Berechtigung, Einschränkung per Exchange RBAC for Applications). Schritt-für-Schritt-Anleitung für Lena: Claude Doc „80BPM Monitoring: Mailversand über info@ einrichten“.
3. Im Repo unter Settings → Secrets and variables → Actions anlegen:
   - `MONITOR_TENANT_ID`, `MONITOR_CLIENT_ID`, `MONITOR_CLIENT_SECRET`
   - `MONITOR_MAIL_FROM` (Absender-Postfach)
   - `MONITOR_MAIL_TO` (kommagetrennt: Kenan, Lena, Ann-Sophie)
4. Actions → „Verfügbarkeit 80BPM“ → Run workflow mit „Testmail“ anhaken. Die Mail muss bei allen ankommen.
5. Ernstfall-Test: Dev-URL testweise eintragen oder eine URL vorübergehend falsch schreiben, Alarm und Entwarnung abwarten, danach zurücksetzen.

## Hinweise

- GitHub startet geplante Läufe nicht auf die Minute genau, bei Last auch 10–15 Minuten später.
- Der monatliche Lebenszeichen-Commit verhindert, dass GitHub den Zeitplan nach 60 Tagen ohne Aktivität pausiert.
- Läuft das Client-Geheimnis ab, schlägt der Mailschritt fehl; GitHub mailt dann einen fehlgeschlagenen Lauf an den Repo-Inhaber.
