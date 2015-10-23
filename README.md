# valgorama
En liten sak som hinter til hvilket parti du burde stemme på

##Beskrivelse
Valgorama samler først inn demogragiske data, og stiller deretter en rekke politiske spørsmål.

Hvert spørsmål har fem svar:

  - Helt enig
  - Litt enig
  - Nøytral
  - Litt uenig
  - Helt uenig

Hvert svar skal også vektes:

  - Veldig viktig
  - Viktig
  - Ikke viktig

Etter å ha svart på alle spørsmålene, får man opp en graf som viser deg hvilket politisk parti som passer deg best.

##Krav
- [x] Må kunne ta høyde for høy samtidig trafikk
- [x] Må fungere like bra på store og små skjermer
- [x] API'et må være generelt nok for flere platformer

##Server
Tatt utgangspunkt i Lumen (http://lumen.laravel.com/) i PHP.

###Modeller
- User (id, gender, ageGroup, partyLastElection, municipality)
- AgeGroup (id, startAge, endAge)
- Party (id, name, logo)
- County (id, name)
- Municipality (id, name, countyId)
- Topic (id, name)
- Statement (id, text, topicId)
- StatementAgreement (id, statementId, partyId, weight)
- StatementAnswer (id, statementWeight, importanceWeight,userId, statementId)

###Endpoints
**/api/start**
```json
  {
    "userId": 15
  }
```
**/api/statements**

*Input: userId {int}, gender {1, 2}, ageGroup [1 - 8], lastParty [1 - 11], municipality: [1 - 428]*
```json 
  {
    "userId": 15,
    "gender": 2,
    "ageGroup": 4,
    "lastParty": 9,
    "municipality": 208
  }
```

*Output*
```json
  {
    "statements": [
    	{
    	  "id": 12,
    	  "text": "Alle subsidier til norsk petroleumsvirksomhet bør stoppes nå, og hele næringen bør avvikles innen 20 år.",
    	  "category": "Økonomi"
    	},
    	{
    	  "id": 13,
    	  "text": "Lokalpolitikerne bør kunne bestemme mer over fordelene for el-biler.",
    	  "category": "Miljø"
    	}
    ]
  }
```

**/api/result**

*Input: Svar og vekt for hvert besvarte spørsmål*
```json
  {
    "userId": 15,
    "statements": [
      {
        "statement_id": 1,
        "answer": 1,
        "weight": 1
      },
      {
        "statement_id": 1,
        "answer": 5,
        "weight": 2
      },
      {
        "statement_id": 1,
        "answer": 3,
        "weight": 3
      }
    ]
  }
```
*Output*
```json
  {
    "winner": {
      "name" : "Venstre",
      "logo" : "/assets/logo/venstre.png"
    },
    "results": [
      {
        "name": "Venstre",
        "logo": "/assets/logo/venstre.png",
        "score": 61
      },
      {
        "name": "Høyre",
        "logo": "/assets/logo/hoyre.png",
        "score": 45
      },
      {
        "name": "Senterpartiet",
        "logo": "/assets/logo/sp.png",
        "score": 41
      }
    ]
  }
```

##Client
Tatt utgangspunkt i React.js (https://facebook.github.io/react/) i Javascript.

###React Components
Dette gir et grovt overblikk på hvilke React Components som er inkludert:
- App
  - TitleBar
  - Content
    - DemographicScreen
      - InfoScreen
        - GenderQuestion
      - BulletpointQuestion
        - Bulletpoint
    - PoliticalStatement
      - StatementTitle
      - StatementAnswers
        - StatementAnswer
      - ImportanceQuestion
        - ImportanceAnswer
    - ResultScreen
      - ResultInfo
      - ResultGraph
        - Result


