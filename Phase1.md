# Esivalmistelut

Kurssin tähänastiset Laboratorio ja Activity -harjoitukset olin tähän mennessä tehnyt Windows-koneella, joten päätin harjoituksen vuoksi työstää projektitehtävän alusta loppuun Linux -käyttöjärjestelmällä.

<img width="453" height="273" alt="image" src="https://github.com/user-attachments/assets/0824ab22-a264-44f7-8021-a54e54902723" />


## Pakettienhallinnan päivitys

Hyväksikoettu tapa työskentelyn alkuun, eli aloitin päivittämällä pakettivarastot:

    sudo apt update

<img width="586" height="216" alt="image" src="https://github.com/user-attachments/assets/5a14a5da-fd13-4163-87a8-56ea020a9fa2" />

## Python version tarkistus

Pythonia tarvitaan useammassakin vaiheessa, joten tarkistin että se on asennettuna:

    python3 --version

<img width="233" height="98" alt="image" src="https://github.com/user-attachments/assets/e8e4c918-4ed2-4bc5-bcd4-59d2b9eef3fb" />

## Unzip työkalun tarkistus

Pakattuja hakemistoja varten käytössä unzip-työkalu. Työkalu oli valmiiksi asennettuna koneelleni:

    unzip -v

<img width="647" height="131" alt="image" src="https://github.com/user-attachments/assets/798f7390-e472-4f7e-a385-92b7b0fb396a" />

## AWS CLI asennus

Ladataan pakattu hakemisto aws:n sivuilta. Puretaan paketti ja asennetaan AWS CLI:

    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install

<img width="771" height="114" alt="image" src="https://github.com/user-attachments/assets/454acd0c-449e-4381-a0f8-ed82c388a67f" />

## AWS version tarkistus

Tarkistin että asennus oli sujunut ongelmitta ja prompti palauttaa version:

    aws --version

<img width="635" height="72" alt="image" src="https://github.com/user-attachments/assets/517edcdd-2616-4b5b-a548-1db933cfb191" />

# AWS konfiguraatio

## aws credentiaalit

Access key, secret access key ja session token -tiedot hain aws academy -sivustolta Learner Lab osiosta (AWS Details > Cloud Access > AWS CLI)

<img width="608" height="384" alt="image" src="https://github.com/user-attachments/assets/f947e607-77d7-4ee7-8617-4050ed8f6c51" />

## AWS CLI konfigurointi

Konfiguroidaan AWS CLI, annetaan access key, secret access key ja session token. Regionina toimii labraympäristön oletus us-east-1 ja oletus output format:na toimii json:

    aws configure

<img width="647" height="224" alt="image" src="https://github.com/user-attachments/assets/0798455a-4b9f-44f0-808e-defdecc0ce60" />

## AWS CLI konfiguraation tarkistus

Komennolla `aws configure list` voidaan vielä silmäillä konfiguraatiota:

    aws configure list

<img width="641" height="160" alt="image" src="https://github.com/user-attachments/assets/31744c09-a7bd-46ba-be21-5cf079eafb63" />


# S3 Bucketit

## Bucketien luominen

Luodaan projektityötä varten kaksi erillistä bucketia, input-bucket (leppanen-input) ja output-bucket (leppanen-output). Komennolla `aws s3 ls` voidaan vielä listata ja tarkistaa bucketit:

    aws s3api create-bucket --bucket leppanen-input --region us-east-1
    aws s3api create-bucket --bucket leppanen-output --region us-east-1
    aws s3 ls

<img width="677" height="255" alt="image" src="https://github.com/user-attachments/assets/dbadfd7e-b605-46aa-93e1-ec1600de3412" />

## AWS Management Console -näkymä

Tarkistin vielä että löydän bucketit myös AWS Management Consolen kautta.

<img width="1048" height="348" alt="image" src="https://github.com/user-attachments/assets/5f055b0e-4881-4408-9dc9-ed41ef03900e" />

# Bucketien testaus

## Testitiedostot

Testasin että tiedostojen lataus bucketeihin onnistuu:

    echo "Hello input bucket!" > testinput.txt
    echo "Hello output bucket!" > testoutput.txt

    aws s3 cp testinput.txt s3://leppanen-input
    aws s3 cp testoutput.txt s3://leppanen-output


<img width="529" height="134" alt="image" src="https://github.com/user-attachments/assets/54360a94-5ee7-4a9c-8606-942ed048024d" />

## Bucketien sisällön tarkistus

Vielä vilkaisu, että tiedostot löytyvät kummastakin bucketista:

    aws s3 ls
    aws s3 ls s3://leppanen-input
    aws s3 ls s3://leppanen-output

<img width="418" height="163" alt="image" src="https://github.com/user-attachments/assets/feb6f35f-fbaf-4159-bf79-98ccea02f710" />

## Bucketien sisältö management consolessa

Vielä Management Consolen kautta vilkaisu bucketeihin, että tiedostot löytyvät.

<img width="1558" height="310" alt="image" src="https://github.com/user-attachments/assets/c376d169-9fd4-4d21-9e5c-c7f9e2351bf4" />

<img width="1560" height="314" alt="image" src="https://github.com/user-attachments/assets/800f2565-7b9b-443f-b5f0-4b84c63cd970" />

## Poistetaan testitiedostot ja katsotaan että ls-komennot palauttavat tyhjät promptit

Tiedostot palvelivat tehtävänsä, tyhjensin bucketit ennen seuraavaan vaiheeseen etenemistä:

    aws s3 rm s3://leppanen-input/testinput.txt
    aws s3 rm s3://leppanen-output/testoutput.txt
    aws s3 ls s3://leppanen-input
    aws s3 ls s3://leppanen-output
    
<img width="501" height="146" alt="image" src="https://github.com/user-attachments/assets/cb0580a2-bd6d-4813-a6e8-89e19ac0ed68" />

# AWS Lambda

## Lambda funktio

Luodaan ensin tyhjä tiedosto, johon funktio tallennetaan:

    touch lambda_function.py

Käytin tässä micro editoria (`sudo apt-get install -y micro`). Lisäsin funktion lambda_function.py -tiedostoon:

```
import boto3
from datetime import datetime
import os


s3 = boto3.client('s3')


OUTPUT_BUCKET = 'leppanen-output'

def lambda_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        
        timestamp = datetime.now().strftime("%Y%m%d-%H%M%S")
        new_key = f"{timestamp}-{key}"
        
        copy_source = {'Bucket': bucket, 'Key': key}
        s3.copy_object(CopySource=copy_source, Bucket=OUTPUT_BUCKET, Key=new_key)
        
 
        print(f"Processed {key} -> {OUTPUT_BUCKET}/{new_key}")
```

## Lambda funktion zippaus

Lambda vaatii, että tiedosto on nimenomaan zip -muodossa, joten pakkasin tiedoston komennolla:

    zip function.zip lambda_function.py

## Lambda funktion luominen

Ajoin seuraavan komennon, jolla lambda funktio luotiin. Python versiona käytin python 3.12, jonka version tarkistin aiemmin:

        aws lambda create-function \
            --function-name leppanen-in-out \
            --runtime python3.12 \
            --role arn:aws:iam::905418040354:role/LabRole \
            --handler lambda_function.lambda_handler \
            --zip-file fileb://function.zip \
            --region us-east-1

## Event notification konfiguraatio

Loin ensin tyhjän tiedoston, johon konfiguraatio tallennetaan:

        touch s3-notification.json

Seuraava json käynnistää Lambda funktion, kun uusi tiedosto lisätään:

    {
        "LambdaFunctionConfigurations": [
            {
                "Id": "NewFileTrigger",
                "LambdaFunctionArn": "arn:aws:lambda:us-east-1:905418040354:function:leppanen-in-out",
                "Events": ["s3:ObjectCreated:*"]
            }
        ]
    }

## S3 permission

Annetaan S3:lle lupa kutsua Lambdaa:

     aws lambda add-permission \
        --function-name leppanen-in-out \
        --principal s3.amazonaws.com \
        --statement-id s3invoke \
        --action "lambda:InvokeFunction" \
        --source-arn arn:aws:s3:::leppanen-input

## Liitetään Lambda funktio S3 triggeriin

        aws s3api put-bucket-notification-configuration \
            --bucket leppanen-input \
            --notification-configuration file://s3-notification.json

## Versioinnin käyttöönotto

Lisätään versiointi kumpaankin bucketiin myöhempiä vaiheita varten:

    aws s3api put-bucket-versioning \
    --bucket leppanen-input \
    --versioning-configuration Status=Enabled

    aws s3api put-bucket-versioning \
    --bucket leppanen-output \
    --versioning-configuration Status=Enabled

# Lifecycle rule

Eräs tapa säästää kustannuksissa on poistaa S3 Bucketeihin tallennettujen tiedostojen vanhoja versioita tietyn ajanjakson jälkeen (tai vaihtoehtoisesti siirtää ne pitkäaikaisarkistoon kuten glacier). Kyseisessä projektissa vanhat tiedostoversiot poistetaan automaattisesti kuukauden ajanjakson jälkeen.

Luodaan tiedosto lifecycle.json:

    touch lifecycle.json

Lisätään lifecycle -sääntö:

    {
      "Rules": [
        {
          "ID": "DeleteOldVersions",
          "Status": "Enabled",
          "Filter": {
            "Prefix": ""
          },
          "NoncurrentVersionExpiration": {
            "NoncurrentDays": 30
          }
        }
      ]
    }


## Käyttöönotto

Lifecycle -sääntö täytyy ottaa käyttöön kumpaankin bucketiin erikseen, otin ne käyttöön ajamalla seuraavat komennot:

    aws s3api put-bucket-lifecycle-configuration \
    --bucket leppanen-input \
    --lifecycle-configuration file://lifecycle.json

    aws s3api put-bucket-lifecycle-configuration \
    --bucket leppanen-output \
    --lifecycle-configuration file://lifecycle.json

Seuraavilla komennoilla voidaan vielä varmistua siitä että sääntö on voimassa:

    aws s3api get-bucket-lifecycle-configuration \
    --bucket leppanen-input

    aws s3api get-bucket-lifecycle-configuration \
    --bucket leppanen-output

## Testaus

Luodaan testitiedosto:

    echo "Hello Projekti" > projektitesti.txt

Ajetaan seuraava komento. Jokaisella ajolla input-bucketiin tallentuu tiedosto omalla versio numerollaan:

    aws s3 cp projektitesti.txt s3://leppanen-input
    aws s3 cp projektitesti.txt s3://leppanen-input
    aws s3 cp projektitesti.txt s3://leppanen-input

Ja tarkistetaan vielä että kyseisestä tiedostosta todella löytyy kaivatut kolme eri versiota.

Input-bucket:

    aws s3api list-object-versions --bucket leppanen-input --prefix projektitesti.txt

Lambda luo tiedostoista uudet nimet timestampilla, output-bucketista löytyy 3 eri tiedostoa:

    aws s3 ls s3://leppanen-output
