# S3Pipe

**Opiskelija:** Ari-Pekka Leppänen  
**Päivämäärä:** 2026-03-31  

---

## Sisällysluettelo

1. Johdanto
2. Projektisuunnitelma
   - Vaihe 1 Suunnittelu
   - Vaihe 2 Suunnittelu
3. Toteutus
   - Vaihe 1 Toteutus ja validointi
   - Vaihe 2 Toteutus ja validointi
4. Parannukset
5. Parannuksen X toteutus 
6. Yhteenveto
7. Liitteet
8. Lähteet 

---

## Johdanto

Projektin aiheeksi valitsin yksinkertaisen datan käsittelyputken (Simple data processing pipeline), joka hyödyntää AWS S3-bucketeja ja Lambda-funktiota.

Projektin taustalla oli tarve varmuuskopioida omia tärkeitä tiedostoja, koulutehtäviä ja raportteja, jotka toistaiseksi olivat vain yhden raudan varassa (eli oman pöytäkoneeni). Tiedostojen varmuuskopiointi pilveen vaikutti kustannustehokkaalta ja luotettavalta ratkaisulta. Ulkoisten levyasemien tai RAID-asetusten kasaaminen voi nopeasti tulla kalliiksi, eikä silti takaa yhtä luotettavaa menetelmää varmuuskopiointiin. Lisäksi tämä projekti toimii hyvin realistisena harjoituksena, joten saadaan kaksi kärpästä yhdellä iskulla.

Projektin tavoitteet:
- AWS CLI:n asennus ja konfigurointi.
- S3 buckettien luonti, testaus ja hallinta.
- Lambda-funktion luonti, zippaus ja käyttöönotto.
- S3-event-triggerin määrittäminen Lambda-funktiota varten.
- Prosessin testaus ja lopputuloksen tarkastelu.
---

## Projektisuunnitelma

### Vaihe 1 Suunnittelu

Alustava arkkitehtuuri nojaa näihin AWS:n palveluihin:
- S3 - Tiedostojen tallennus ja varmuuskopiointi (input- ja output-bucketit)
- Lambda - Tiedostojen automaattinen käsittely ja kopiointi output-bucketiin.
- IAM - Lambda tarvitsee oikeudet S3 bucketeihin, mutta projektityön ympäristössä hyödynnetään valmiiksi luotua IAM-rolea, eikä käyttäjä voi luoda uusia.

Alustava arkkitehtuuri:

<img width="933" height="556" alt="image" src="https://github.com/user-attachments/assets/f0b0268c-75dd-4ca8-a144-3ecf7791c357" />

Perustelut valinnoille:
- S3: Skaalautuva, luotettava ja henkilökohtaisessa käytössä, jossa tiedonsiirto on verrattain pientä, erittäin edullinen vaihtoehto.
- Lambda: Serverless-ratkaisu säästää kustannuksissa ja automatisoi prosesseja.
- Event trigger: Nopea ja tehokas työkalu reaaliaikaiseen tiedostojen käsittelyyn.

### Vaihe 2 Suunnittelu

Include the cloud management and governance functions (like monitoring, scaling, logging, cost optimization etc.) planned on top of phase1 architecture.

---

## Toteutus

### Vaihe 1 Toteutus ja validointi

Vaihe 1 Toteutus ja validointi: [Linkki](https://github.com/arilep/Cloud_operations_aws/blob/main/Phase1.md)

### Vaihe 2 Toteutus ja validointi

In both phases follow the same guidelines used in lab/activity reports:
- the goal of the project work
- resources and configuration that you are creating/modifying
- step-by-step actions
- if something failed first, explain
  - what went wrong?
  - how did you diagnosed the issue?
  - what steps you took to fix it? 
- screenshots / command examples of critical steps and final results
- validation - how did you verify that the outcome is working as expected?
- what did you learn?
---

## Parannukset

How could you improve your current work?
  - Architectural enhancements?
  - Improvements to cloud governance?
  - Security related best practices? (IAM roles, firewall rules etc.)

Analyze different options (reasoning, why needed, implementation options etc.)


### Parannuksen X toteutus

If time allows implement one enhancement. 

---

## Yhteenveto

What was learned?

What would you improve or do differently?

Comparison to Alternatives (why this approach?)

---

## Liitteet

Omia muistiinpanoja projektityötä varten [Linkki](https://github.com/arilep/Cloud_operations_aws/blob/main/Commands.md)

---

## Lähteet

Documentation, tutorials, or other resources used.

AWS. AWS Academy Cloud Operations -opintojakson esitysmateriaali aws academy -alustalla. 

Heinonen, J. Cloud Operations AWS -opintojakson esitysmateriaali Moodlessa. Haaga-Helia ammattikorkeakoulu.
