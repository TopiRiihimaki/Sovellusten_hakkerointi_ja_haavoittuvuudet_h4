

# a)

Ghidra asennettiin Linuxissa seuraavilla komennoilla:

```
sudo apt update
sudo apt install openjdk-25-jdk unzip wget
```

Ghidran ZIP-tiedosto purettiin Downloads-hakemistossa komennolla:

```
unzip ghidra_*.zip
```

Tämän jälkeen Ghidra käynnistettiin komennolla:

```
./ghidraRun
```

Ghidra käynnistyi onnistuneesti ja oli valmis käytettäväksi.


# b)

<img width="490" height="327" alt="image" src="https://github.com/user-attachments/assets/911f95b5-d4b4-46f6-a08a-94e727051a71" />

Ohjelma pyytää käyttäjältä syötettä, jota sitten verrataan tuohon kovakoodattuun salasanaan. Jos käyttäjän syöte täsmää, antaa ohjelma tuon lipun, muuten se sanoo, että väärin meni.

Muutin nuo muuttujien nimet itse

# c)

Kokeilin ennen kun lähdin ghidraan, että ohjelma ei anna lippua jos salasana on väärin:

<img width="718" height="99" alt="image" src="https://github.com/user-attachments/assets/51d2f61d-24ab-4778-80d4-ead2f8c789d2" />

Avasin työn ghidrassa. Etsin pää funktion joka näytti decompilerissa tältä:

<img width="520" height="329" alt="image" src="https://github.com/user-attachments/assets/5461bc8a-db85-4cbe-9e69-5fce95bd778e" />

Klikkasin tuohon if lauseeseen, joka sitten heitti minun tähän kohtaan binäärissä:

<img width="993" height="156" alt="image" src="https://github.com/user-attachments/assets/e7b1348d-41fe-47d2-a8b5-7749a4aa1801" />

Vaihdoin tuon JNZ (Jump is Not Zero) JZ, eli poistin vaan tuon N-kirjaimen. (Tätä pääsee muokkaamaan kun painaa oikeeti hiiren näppäintä ja ottaa sieltä "Patch Instruction" tai painaa control+shift-G)

Decompiler koodi muuttui näin, kun poistin tuon N-kirjaimen:

<img width="505" height="310" alt="image" src="https://github.com/user-attachments/assets/e54c1a04-8501-40a6-9511-4ec4193380f5" />

Minä sitten tallensin tämän ja painoin "Export program" (se löytyy kun painaa "file"). Sieltä vaihdoin format "Original format" ja laitoin uuden luodun ohjelman kansiooni.


Koska ohjelmalla ei ole ajo-oikeuksia:
```
chmod +x <TIEDOSTON NIMI>
```

Minä tein näin ja ajoin tuon uuden ohjelman. Laitoin eka oikean salasanan, josta se herjasi. Laitoin sitten väärän, josta sain lipun:

<img width="765" height="173" alt="image" src="https://github.com/user-attachments/assets/4d977d8b-1592-48df-93e8-7c7d09da3194" />

# d)

## e1)

Aloitin tehtävän kopioimalla repositorin minun koneelle. Koska minulla on github asennettu tälle virtuaalikoneelle niin pystyin vaan laittamaan 
```
git clone <HTTPS linkki>
```
Tämän jälkeen toimin tiedoston antamien ohjeiden mukaan (Löytyvät README.md tiedostossa) Aloitiin siis komennolla:
```
make crackme01
```
Joka loi uuden tiedston nimeltä crackme01.64.

Ajoin tämän uuden tiedoston, josta sain tälläisen:

<img width="640" height="60" alt="image" src="https://github.com/user-attachments/assets/3b0c12db-3a05-4ca3-a5f2-916a9cb08709" />

Ohjelma odottaa siis jotakin syötettä. Kun annoin jonkin syötteen, sain tälläisen:

<img width="737" height="82" alt="image" src="https://github.com/user-attachments/assets/ecefd582-bbd8-4a30-805f-daf25b529c1e" />

**Ghidra**

Tein uuden projektin Ghidrassa ja lisäsin tuön crackme01.64 tiedoston siihen projektiin ja analysoin sen. Katsoin sitten decompilerista miltä koodi about näyttää.

<img width="353" height="436" alt="image" src="https://github.com/user-attachments/assets/e78d7d34-12df-4a9d-92d7-afb42936c637" />

Tuosta pystyy nähdä, että siellä on if lauseita, joiden perusteella palautetaan joko 0, 1 tai jotain muuta. Tuossa missä se ilmaisee antavansa 0 on se mikä me halutaan, koska 0 tarkoittaa, että ohjelma on ajettu onnistuneesti.

Tuo __sl vaikuttaa olevan käyttäjän syöte, jota verrataan merkkijonoon password1. Kokeillaan laittaa tuo password1 ohjelmaan:

<img width="745" height="68" alt="image" src="https://github.com/user-attachments/assets/17b8104b-4eec-460e-a75d-975e82f83975" />

Kas saimme oikean. Voimme vielä tarkistaa, että saimme oikean komennolla:
```
echo $?
```
Tämä komento näyttää aikasemman ohjelman exit arvon. Koska saimme ohjelman oikein, sen pitäisi näyttää 0.

<img width="592" height="89" alt="image" src="https://github.com/user-attachments/assets/f79cbbf9-315e-49af-abf2-ad2f97dddfec" />

Jos laitamme jotakin väärää, sen pitäisi läyttää 1.

<img width="691" height="118" alt="image" src="https://github.com/user-attachments/assets/41bcc1f6-9113-4a40-bc43-050f5d172c49" />

Ja koska saimme tekstin "Need exactly one argument." kun emme laittaneet mitään, niin voidaan katsoa senkin exit status.

<img width="661" height="103" alt="image" src="https://github.com/user-attachments/assets/6217c02e-d2df-456a-b26a-74936eb83eb6" />

## e2)

Tein samat vaiheet kun ensinmmäisessä, eli tein make komennon ja ajoin ohjelman, josta sain samat vastaukset, kun jätin tyhjäksi/laitoin väärän vastauksen.

Tein uuden ghidra-projektin jonne ĺisäsin tuon ccrackme01e.64 ja analysoin sen. main ohjelma näytti tältä:

<img width="343" height="435" alt="image" src="https://github.com/user-attachments/assets/35473269-de40-4343-9ddf-949700afafcf" />

Tässä näemme tuon salasanan decompilerissa. Kokeilaan sitä siis.

<img width="754" height="80" alt="image" src="https://github.com/user-attachments/assets/0b7a2590-b03c-4fe9-b622-d66bde803c0a" />


Tässä tämä hermostuu tuosta huutomerkistä. Korjataksemme tämä, meidän pitää laittaa salasana ' merkkien sisälle

<img width="764" height="60" alt="image" src="https://github.com/user-attachments/assets/c3fdaffa-1f16-41e3-ba96-fe9409710a0b" />

Saimme näin oikena. Voimme vielä tarkistaa echo komennolla.

<img width="763" height="109" alt="image" src="https://github.com/user-attachments/assets/7816cbdc-35e5-4bbf-9d02-517d2ed88cc7" />

Käydään vielä kaikki läpi.

<img width="730" height="189" alt="image" src="https://github.com/user-attachments/assets/bf2a05b5-0781-4652-ab51-8680adbf735e" />


# f)

Tein ghidraan aivan samat vaiheet ku aikasemminkin. Tässä kuvakaappaus main funktion decompilerista. Muuuttujien nimet ovat jo muutettu seuraavasti:

lVar1 → index, koska sitä kasvatetaan +1 jokaisella kierroksella ja sitä käytetään merkkijonon indeksinä.
cVar1 → expected_char, koska siihen tallennetaan merkki, jota käyttäjän syötteeltä odotetaan.
cVar2 → input_char, koska siihen tallennetaan käyttäjän antamasta argumentista luettu merkki.

<img width="335" height="487" alt="image" src="https://github.com/user-attachments/assets/bf548945-1021-42b3-af6a-b99b16da547d" />

Ohjelman dekompiloidusta koodista selvisi, että salasanaa verrataan merkkijonoon "password1" vähentämällä jokaisesta merkistä yksi ASCII-arvo. Esimerkiksi p (112) → o (111). Tämän perusteella oikeaksi salasanaksi saatiin:
```
o`rrvnqc0
```
Jos kokeilemme tätä ohjelmassa:

<img width="761" height="61" alt="image" src="https://github.com/user-attachments/assets/465782d4-5047-4970-a5ba-cd6f170cb420" />

Niin saimme tuon vastauksen (tuo vastaus pitää laittaa ' merkkien sisälle, koska se muuten herjaa tuon ` merkin takia).

Ohjelmassa on pieni hiekkous. Jos annamme vain ensinmmäisen muuttuneen arvon, eli o, niin se silti käsittelee sen, kuin se olisi oikea vastaus.

<img width="694" height="66" alt="image" src="https://github.com/user-attachments/assets/f8dd80dd-8d2f-4a26-a224-2b273d7ef936" />

Korjaus olisi, että se tarkistaisi koko merkkijonon, eikä vain ensinmmäistä.

## Tekoälyn käyttö (ChatGPT 5.6 Luna)
- Auttanut selittämään
- Auttanut raportin sanottamisessa

## Lähteet
https://terokarvinen.com/application-hacking/

https://github.com/NoraCodes/crackmes
