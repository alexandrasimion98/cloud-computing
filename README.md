# Proiect Cloud Computing


## Descriere proiect

Ideea de baza a proiectului consta in oferirea de informatii despre orice tara(capitala, cod telefonic, populatie, moneda, regiune, subregiune) si prezentarea celor mai recente statistici cu privire la virusul COVID19.

## Prezentare API-uri utilizate 

API-urile pe care le-am folosit sunt următoarele:
1. https://restcountries.eu/rest/v2/all - folosit pentru intoarcerea de informatii despre tara selectata.
2. https://covid-193.p.rapidapi.com/statistics - folosit pentru prezentarea celor mai recente statistici cu privire la COVID19 pentru tara selectata.

## Descriere arhitectura

* Aplicatia contine 2 functionalitati.
* Pe prima pagina, selectul pentru tara vine by default randomizat, putand fi aleasa ulterior orice tara.

* Metodele apelate cu API-urile integrate sunt:
1. initialize - pentru primul API care apeleaza metoda GET, request-ul din URL si ulterior populeaza selectul in momentul incarcarii paginii.

```
fetch("https://restcountries.eu/rest/v2/all")
.then(res => res.json())
.then(data => initialize(data))
.catch(err => console.log("Error:", err));

function initialize(countriesData){
    countries = countriesData;
    let options="";
    countries.forEach(country => options += `<option value="${country.alpha3Code}">${country.name}</option>`);
   countriesList.innerHTML = options;
   countriesList.selectedIndex = Math.floor(Math.random()*countriesList.length); 
   displayCountryInfo(countriesList[countriesList.selectedIndex].value);
}
```

2. getLatestCOVID19Data - pentru cel de-al doilea API care odata cu selectarea tarii, se va trimite raspunsul cu statisticile.

```
async function getLatestCOVID19Data()
{
    await fetch("https://covid-193.p.rapidapi.com/statistics", {
        "method": "GET",
        "headers": {
            "x-rapidapi-host": "covid-193.p.rapidapi.com",
            "x-rapidapi-key": MY_API_KEY
        }
    })
    .then(response => response.json())
    .then(response => {
        console.log("COVID 19 API object:");
        console.log(response);
        console.log("\n");

        // add all countries to select element
        response.response.forEach(c => {
            const option = document.createElement('option');
            option.innerHTML = c.country;
            document.getElementById('countriesCovid19').appendChild(option);
        })

        // save covid data to global variable
        covid19data = response.response;
    })
    .catch(err => {
        console.log(err);
    });
}
```
## Prezentare interfata

![image](https://user-images.githubusercontent.com/56071612/117853737-6fc33c00-b291-11eb-9d60-023f1f0415bf.png)

![image](https://user-images.githubusercontent.com/56071612/117853871-94b7af00-b291-11eb-9dd8-4c6d734f5dd6.png)


