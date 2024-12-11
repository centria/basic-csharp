---
title: "Käyttöliittymät"
nav_order: 1
hidden: false
---

# Käyttöliittymän erottaminen ohjelman logiikasta


Tässä osassa katsotaan tarkemmin käyttöliittymää (englanniksi **User Interface** tai lyhennettynä **UI**) ja ohjelmalogiikkaa (englanniksi **program logic**.) Tarkastellaan prosessia jossa toteutamme ohjelman ja erotamme eri vastuualueet toisistaan. Ohjelma pyytää käyttäjää kirjoittamaan sanoja kunnes käyttäjä kirjoittaa saman sanan kahdesti.


```console
Write a word:
> carrot 
Write a word:
> turnip 
Write a word:
> potato 
Write a word:
> celery 
Write a word:
> potato 
You wrote the same word twice!
```

Rakennetaan ohjelma pala palalta. Yksi haasteista on se, että on vaikea päättää miten lähestyä ongelmaa, eli miten pilkkoa ongelma pienempiin osaongelmiin ja mistä osaongelmasta aloittaa. Yhtä oikeaa vastausta ei ole -- joskus on hyvä lähteä liikkeelle ongelma-alueesta ja sen käsitteistä ja niiden välisistä yhteyksistä, joskus taas on parempi lähteä liikkeelle käyttöliittymästä.

Voisimme aloittaa ohjelman toteuttamisen luomalla käyttöliittymän ja sille luokan UserInterface. 


```cpp
public class UserInterface
{
  public UserInterface() 
  {

  }

  public void Start()
  {
    // Tehdään jotain...
  }
}
```

Käyttöliittymän luominen ja käynnistäminen onnistuu seuraavasti.


```cpp
public static void Main(string[] args) 
{  
  UserInterface userinterface = new UserInterface();
  userinterface.Start();
}
```  

## Silmukka ja lopetus

Tässä ohjelmassa on (ainakin) kaksi "aliongelmaa". Ensimmäinen on sanojen lukeminen käyttäjältä niin kauan kuin jokin ehto toteutuu. Voimme hahmotella tämän seuraavasti.

```cpp
public class UserInterface
{
  public UserInterface() 
  {

  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();

      if(/*loppuehto tänne*/) {
        break;
      }
    }
    Console.WriteLine("You gave the same word twice!");
  }
}
```

Ohjelma jatkaa sanojen lukemista kunnes käyttäjä kirjoittaa sanan jonka hän on jo kirjoittanut aiemmin. Muokataan ohjelmaa niin, että se tarkistaa onko sana jo kirjoitettu. Emme vielä tiedä miten tämä toteutetaan, joten tehdään ensin luonnos.

```cpp
public class UserInterface
{
  public UserInterface() 
  {

  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();
      if(AlreadyEntered(word)) {
        break;
      }
    }
    Console.WriteLine("You gave the same word twice!");
  }
}

  public bool AlreadyEntered(string word) 
  {
    // Tehdään jotain täällä
    return false;
  }
}
```

On hyvä idea testata ohjelmaa jatkuvasti, joten tehdään testiversio metodista.


```cpp
public bool AlreadyEntered(string word) 
{
  if (word == "end") 
  {
    return true;
  }
  return false;
}
```

Nyt silmukka jatkuu kunnes käyttäjä kirjoittaa sanan "end".

```console
Write a word:
> carrot 
Write a word:
> turnip 
Write a word:
> potato 
Write a word:
> celery 
Write a word:
> end
You wrote the same word twice!
```

Ohjelma ei vielä toimi täysin, mutta ensimmäinen osaongelma -- silmukan lopettaminen kun tietty ehto on täyttynyt -- on ratkaistu.

## Oleellisen tiedon säilyttäminen

Toinen aliongelma on muistaa mitä sanoja on jo kirjoitettu. Tähän tarkoitukseen sopiva tietorakenne on lista.


```cpp
public class UserInterface
{
  private List<string> words;

  public UserInterface() 
  {
    this.words = new List<string>();
  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();

      if(AlreadyEntered(word)) {
        break;
      }
    }
    Console.WriteLine("You gave the same word twice!");
  }
  public bool AlreadyEntered(string word) 
  {
    if (word == "end") 
    {
      return true;
    }
    return false;
  }
}
```

Kun uusi sana annetaan, se täytyy lisätä listaan sanoja jotka on jo kirjoitettu. Tämä tehdään lisäämällä rivi joka päivittää listaa silmukan sisällä:

```cpp
while(true)
{
  Console.WriteLine("Enter a word:");
  string word = Console.ReadLine();

  if (AlreadyEntered(word))
  {
    break;
  }
  // lisätään sana listaan aiemmin kirjoitettujen sanojen joukkoon
  this.words.Add(word);
}
```

Koko käyttöliittymä näyttää nyt seuraavalta.



```cpp
public class UserInterface
{
  private List<string> words;

  public UserInterface() 
  {
    this.words = new List<string>();
  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();

      if (AlreadyEntered(word))
      {
        break;
      }
      // lisätään sana listaan aiemmin kirjoitettujen sanojen joukkoon
      this.words.Add(word);
    }
    Console.WriteLine("You gave the same word twice!");
  }

  public bool AlreadyEntered(string word) 
  {
    if (word == "end") 
    {
      return true;
    }
    return false;
  }
}
```

On jälleen hyvä idea testata että ohjelma toimii. Esimerkiksi voimme lisätä testitulostuksen Start-metodin loppuun, jotta varmistamme että syötetyt sanat ovat todella lisätty listaan.

```cpp
// testitulostus kokeilemaan että kaikki toimii
words.ForEach(Console.WriteLine);
```

## Aliongelmien ratkaisujen yhdistäminen

Muutetaan metodia **AlreadyEntered** niin, että se tarkistaa onko syötetty sana jo aiemmin syötettyjen sanojen listassa.


```cpp
public bool AlreadyEntered(string word) 
{
  return this.words.Contains(word);
}
```

Nyt sovellus toimii kuten pitääkin.


## Oliot luonnollisena osana ongelmanratkaisua

Loimme juuri ratkaisun ongelmaan, jossa ohjelma lukee käyttäjältä sanoja kunnes käyttäjä kirjoittaa sanan joka on jo kirjoitettu aiemmin. Esimerkkisyöte oli seuraava:

```console
Write a word:
> carrot 
Write a word:
> turnip 
Write a word:
> potato 
Write a word:
> celery 
Write a word:
> potato 
You wrote the same word twice!
```

Saimme aikaan seuraavan ratkaisun:


```cpp
public class UserInterface
{
  private List<string> words;

  public UserInterface() 
  {
    this.words = new List<string>();
  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();

      if (AlreadyEntered(word))
      {
        break;
      }
      // lisätään sana listaan aiemmin kirjoitettujen sanojen joukkoon
      this.words.Add(word);
    }
    Console.WriteLine("You gave the same word twice!");
  }

  public bool AlreadyEntered(string word) 
  {
    return this.words.Contains(word);
  }
}
```

Käyttöliittymän näkökulmasta, apumuuttuja **words** on vain yksi yksityiskohta. Tärkeintä on että käyttöliittymä muistaa mitä sanoja on syötetty aiemmin. Joukko sanoja on selkeä erillinen "konsepti" tai abstraktio. Tällaiset selkeät konseptit ovat kaikki mahdollisia olioita: kun huomaamme että koodissamme on tällainen abstraktio, voimme miettiä sen erottamista omaksi luokakseen.

## Sanajoukko

Luodaan luokka **WordSet**. Luokan toteutuksen jälkeen käyttöliittymän **Start**-metodi näyttää tältä:


```cpp
while(true)
{
  if (wordSet.Contains(word))
  {
    break;
  }
  wordSet.Add(word);
}
Console.WriteLine("You gave the same word twice!");
```

Käyttöliittymän näkökulmasta, luokka **WordSet** tarjoaa metodin **bool Contains(string word)**, joka tarkistaa onko annettu sana joukossa, ja metodin **void Add(word)**, joka lisää annetun sanan joukkoon.

Huomaamme että käyttöliittymän koodi on nyt paljon selkeämpää. 

Luokan WordSet luonnos näyttää tältä:


```cpp
public class WordSet 
{
  // oliomuuttuja(t)

  public WordSet() 
  {
    // konstruktori
  }

  public bool Contains(string word) 
  {
    // Contains-metodin toteutus
    return false;
  }

  public void Add(string word) 
  {
    // Add-metodin toteutus
  }
}
```

## Aiempi ratkaisu osana toteutusta

Voimme implementoida sanajoukon tekemällä aiemman ratkaisun, listan, oliomuuttujaksi:


```cpp
public class WordSet 
{
  private List<string> words;

  public WordSet() 
  {
    this.words = new List<string>();
  }

  public bool Contains(string word) 
  {
    return this.words.Contains(word);
  }

  public void Add(string word) 
  {
    this.words.Add(word);
  }
}
```

Nyt ratkaisumme on melko elegantti. Olemme erottaneet selkeän konseptin omaksi luokakseen, ja käyttöliittymämme näyttää siistiltä. Kaikki "likaiset yksityiskohdat" on encapsuloitu siististi olioon.

Muokataan nyt käyttöliittymää niin että se käyttää luokkaa WordSet. Luokka annetaan käyttöliittymälle parametrina.


```cpp
public class UserInterface
{
  private WordSet wordSet;

  public UserInterface(WordSet wordSet) 
  {
    this.wordSet = wordSet;
  }

  public void Start()
  {
    while(true)
    {
      Console.WriteLine("Enter a word:");
      string word = Console.ReadLine();

      if (this.wordSet.Contains(word))
      {
        break;
      }
      // lisätään sana listaan aiemmin kirjoitettujen sanojen joukkoon
      this.wordSet.Add(word);
    }
    Console.WriteLine("You gave the same word twice!");
  }
}
```

Ohjelma käynnistetään nyt seuraavasti:

```cpp
public static void Main(string[] args) 
{  
  WordSet set = new WordSet();
  UserInterface userinterface = new UserInterface(set);
  userinterface.Start();
}  
```

## Luokan toteutuksen muuttaminen

Olemme saavuttaneet nyt ratkaisun missä luokka **WordSet** kapsuloi listan. Onko tämä järkevää? Ehkä. Tämä johtuu siitä että voimme tehdä luokkaan muutoksia jos niin haluamme, ja pian saattaa olla että sanajoukon täytyy tallentaa sanat tiedostoon. Jos teemme kaikki muutokset luokkaan WordSet muuttamatta käyttöliittymän metodeja, emme joudu muuttamaan käyttöliittymän koodia ollenkaan.

Tärkein pointti on kuitenkin se, että muutokset jotka tehdään luokkaan WordSet eivät vaikuta käyttöliittymään. Tämä johtuu siitä että käyttöliittymä käyttää luokkaa WordSet vain sen tarjoamien metodien kautta -- näitä kutsutaan luokan julkisiksi rajapinnoiksi.

## Uuden toiminnallisuuden toteuttaminen: palindromit

Tulevaisuudessa voisimme haluta laajentaa ohjelmaa niin että luokka **WordSet** tarjoaa uusia toiminnallisuuksia. Jos esimerkiksi haluaisimme tietää kuinka monta syötetyistä sanoista oli palindromi, voisimme lisätä luokkaan **WordSet** metodin **Palindromes**.


```cpp
public void Start()
{
  while(true)
  {
    Console.WriteLine("Enter a word:");
    string word = Console.ReadLine();

    if (this.wordSet.Contains(word))
    {
      break;
    }
    this.wordSet.Add(word);
  }
  Console.WriteLine("You gave the same word twice!");
  Console.WriteLine(this.wordSet.Palindromes() + " of the words were palindromes.");
}
```

Käyttöliittymä pysyy siistinä, koska palindromien laskeminen tapahtuu luokan **WordSet** sisällä. Seuraavassa on esimerkki metodin toteutuksesta.


```cpp
public int Palindromes() 
{
  int count = 0;

  foreach (string word in this.words)) 
  {
    if (IsPalindrome(word)) 
    {
      count++;
    }
  }

  return count;
}

public bool IsPalindrome(string word) 
{
  int length = word.Length;
  int end = word.Length - 1;
  for (int i = 0; i < length / 2; i++)
  {
      if (word[i] != word[end - i])
          return false;
  }
  return true;
}
```

Metodi **Palindromes** käyttää apunaan metodia **IsPalindrome**. Metodi **Palindromes** laskee kuinka monta sanoista on palindromi, ja metodi **IsPalindrome** tarkistaa onko sana palindromi vai ei.

Kun konseptit on erotettu toisistaan eri luokkiin koodissa, niiden kierrätys ja uudelleenkäyttö muissa projekteissa käy helposti. Esimerkiksi luokkaa **WordSet** voisi käyttää hyvin graafisessa käyttöliittymässä, tai se voisi olla osa mobiilisovellusta. Lisäksi ohjelman testaaminen on helpompaa kun se on jaettu useisiin konsepteihin, joista jokainen toimii itsenäisenä kokonaisuutena.

## Ohjelmointivinkkejä

Yllä olevassa isossa esimerkissä noudatimme seuraavia ohjelmoinnin periaatteita.

* Etene pienin askelin
  * Yritä jakaa ohjelma pienempiin aliongelmiin ja ratkaista ne yksi kerrallaan
  * Testaa aina että ohjelma toimii ennen kuin jatkat eteenpäin, toisin sanottuna: testaa että aliongelman ratkaisu on oikea
  * Tunnista tilanteet joissa ohjelman tulee toimia eri tavalla. Yllä olevassa esimerkissä, tarvitsimme toiminnallisuuden tarkistamaan onko sana jo kirjoitettu aiemmin.

* Kirjoita niin "puhdasta" koodia kuin mahdollista
  * Sisennä koodisi
  * Käytä kuvaavia metodi- ja muuttujanimiä
  * Älä tee metodeistasi liian pitkiä, ei edes Main-metodista
  * Tee vain yhtä asiaa yhdessä metodissa
  * **Poista kaikki copy-paste -koodi**
  * Korvaa "huono" ja epäsiisti puhtaalla koodilla
  

* Jos on tarve, ota askel taaksepäin ja tarkastele ohjelmaa kokonaisuutena. Jos se e toimi, voi olla hyvä idea palata edelliseen tilanteeseen jossa ohjelma toimi. Voisimmekin sanoa, että rikkinäistä ohjelmaa ei korjata lisäämällä siihen lisää koodia.

Ohjelmoijat seuraavat näitä periaatteita jotta ohjelmoinnista voisi tehdä helpompaa. Niitä seuraamalla koodista tulee myös helpompaa lukea, ylläpitää ja muokata tiimeissä.

## Yhdestä entiteetistä useaan osaan

Tarkastellaan ohjelmaa joka pyytää käyttäjää antamaan tenttipisteitä ja muuttaa ne arvosanoiksi. Lopuksi ohjelma tulostaa arvosanojen jakauman tähtinä. Ohjelma lopettaa syötteiden lukemisen kun käyttäjä antaa tyhjän syötteen. Esimerkki ohjelma näyttää seuraavalta:


```console
Points:
> 91 
Points:
> 98 
Points:
> 103 
Impossible number. 
Points:
> 90 
Points:
> 89 
Points:
> 89 
Points:
> 88 
Points:
> 72 
Points:
> 54 
Points:
> 55 
Points: 
51 
Points:
> 49 
Points:
> 48 
Points:
>

5: *** 
4: *** 
3: * 
2: 
1: *** 
0: **
```

Kuten melkein kaikki ohjelmat, tämä ohjelma voidaan kirjoittaa yhtenä kokonaisuutena Main-metodiin. Tässä on yksi mahdollisuus.


```cpp
public class Program 
{
  public static void Main(string[] args) 
  {
    List<int> grades = new List<int>();

    while (true) {
      Console.WriteLine("Points:");
      string input = Console.ReadLine();
      if (input =0 "") 
      {
          break;
      }
      int score = Conver.ToInt32(input);

      if (score < 0 || score > 100) 
      {
        Console.WriteLine("Impossible number.");
        continue;
      }

      int grade = 0;
      if (score < 50) 
      {
        grade = 0;
      } 
      else if (score < 60) 
      {
        grade = 1;
      } 
      else if (score < 70) 
      {
        grade = 2;
      } 
      else if (score < 80) 
      {
        grade = 3;
      } 
      else if (score < 90) 
      {
        grade = 4;
      } 
      else 
      {
        grade = 5;
      }

      grades.Add(grade);
    }

    Console.WriteLine("");
    int grade = 5;
    while (grade >= 0) 
    {
      int stars = 0;
      foreach(int received in grades) 
      {
        if (received == grade) 
        {
          stars++;
        }
      }

      Console.Write(grade + ": ");
      while (stars > 0) 
      {
        Console.Write("*");
        stars--;
      }
      Console.WriteLine("");
      grade = grade - 1;
    }
  }
}
```

Erotellaan ohjelman eri osat pienemmiksi paloiksi. Tämä voidaan tehdä tunnistamalla ohjelmasta eri vastuualueet. Arvosanojen seuranta, mukaanluettuna pisteiden muuttaminen arvosanoiksi, voidaan tehdä omassa luokassaan. Lisäksi voimme luoda uuden luokan käyttöliittymälle.

## Ohjelmalogiikka

Ohjelmalogiikka sisältää ohjelman toiminnan kannalta kriittiset osat, kuten toiminnot jotka säilyttävät tietoa. Edellisestä esimerkistä pystymme erottamaan osat jotka tallentavat arvosanatiedot. Näistä voimme tehdä luolan **GradeRegister**, joka on vastuussa eri arvosanojen määrän laskemisesta. Rekisteriin voidaan lisätä arvosanoja pisteiden perusteella. Lisäksi rekisteristä voidaan kysyä kuinka monta tiettyä arvosanaa on annettu.

Esimerkki luokasta alla.


```cpp
public class GradeRegister 
{
  private List<int> grades;
  
  public GradeRegister() 
  {
    this.grades = new List<int>();
  }

  public void AddGradeBasedOnPoints(int points) 
  {
    this.grades.Add(PointsToGrades(points));
  }

  public int NumberOfGrades(int grade) 
  {
    int count = 0;
    foreach(int received in this.grades) 
    {
      if (received == grade) 
      {
          count++;
      }
    }
    return count;
  }
  
  public static int PointsToGrades(int points) 
  {
    int grade = 0;
    if (points < 50) 
    {
      grade = 0;
    } 
    else if (points < 60) 
    {
      grade = 1;
    } 
    else if (points < 70) 
    {
      grade = 2;
    } 
    else if (points < 80) 
    {
      grade = 3;
    } 
    else if (points < 90) 
    {
      grade = 4;
    } 
    else 
    {
      grade = 5;
    }
    return grade;
    }
  }
}
```

Kun luokka on erotettu omaksi luokakseen, voimme käyttää sitä käyttöliittymässä. Main näyttää nyt tältä.


```cpp
public class Program 
{
  public static void Main(string[] args) 
  {
    GradeRegister register = new GradeRegister();

    while (true) {
      Console.WriteLine("Points:");
      string input = Console.ReadLine();
      if (input == "") 
      {
          break;
      }
      int score = Conver.ToInt32(input);

      if (score < 0 || score > 100) 
      {
        Console.WriteLine("Impossible number.");
        continue;
      }
      register.AddGradeBasedOnPoints(score);
    }

    Console.WriteLine("");
    int grade = 5;
    while (grade >= 0) 
    {
      int stars = register.NumberOfGrades(grade);
      Console.Write(grade + ": ");
      while (stars > 0) 
      {
        Console.Write("*");
        stars--;
      }
      Console.WriteLine("");

      grade = grade - 1;
    }
  }
}
```

Ohjelmalogiikan erottaminen omakseen on erittäin hyödyllistä ohjelman ylläpidon kannalta. Kun ohjelmalogiikka -- tässä tapauksessa GradeRegister -- on omana luokkanaan, sitä voidaan myös testta muista osista erikseen. Jos haluat, voit kopioida luokan GradeRegister ja käyttää sitä muissa ohjelmissa. Alla on esimerkki yksinkertaisesta manuaalisesta testistä -- tässä testissä keskitytään vain osaan rekisterin toiminnallisuudesta.

```cpp
GradeRegister register = new GradeRegister();
register.AddGradeBasedOnPoints(51);
register.AddGradeBasedOnPoints(50);
register.AddGradeBasedOnPoints(49);

Console.WriteLine("Number of students with grade 0 (should be 1): " + register.NumberOfGrades(0));
Console.WriteLine("Number of students with grade 1 (should be 2): " + register.NumberOfGrades(1));
```

## Käyttöliittymä

Tyypillisesti jokaisella ohjelmalla on oma käyttöliittymänsä. Luomme luokan **UserInterface** ja erotamme sen Main-metodista. Käyttöliittymä saa parametrina GradeRegister-olion, jotta se voi käyttää sitä.

Nyt kun meillä on erillinen käyttöliittymä, pääohjelma jää hyvin yksinkertaiseksi.

```cpp
public class Program 
{
  public static void Main(String[] args) 
  {
    GradeRegister register = new GradeRegister();

    UserInterface userInterface = new UserInterface(register);
    userInterface.Start();
  }
}
```

Katsotaan tarkemmin toteuttamaamme käyttöliittymää. Käyttöliittymässä on kaksi keskeistä osaa: pisteiden lukeminen ja arvosanojen tulostaminen.


```cpp
public class UserInterface 
{
  private GradeRegister register;

  public UserInterface(GradeRegister register) 
  {
    this.register = register;
  }

  public void Start() 
  {
    ReadPoints();
    Console.WriteLine("");
    PrintGradeDistribution();
  }

  public void ReadPoints() 
  {
  }

  public void PrintGradeDistribution() 
  {
  }
}
```

Voimme kopioida koodin pisteiden lukemiseen ja arvosanojen tulostamiseen melko suoraan edellisestä main-metodista. Alla olevassa ohjelmassa on kopioitu koodia vanhasta Main-metodista, ja on luotu uusi metodi tähtien tulostamiseen -- tämä selkeyttää arvosanajakauman tulostamiseen käytetyn metodin koodia.

```cpp
public class UserInterface 
{
  private GradeRegister register;

  public UserInterface(GradeRegister register) 
  {
    this.register = register;
  }

  public void Start() 
  {
    ReadPoints();
    Console.WriteLine("");
    PrintGradeDistribution();
  }

  public void ReadPoints() 
  {
    while (true) 
    {
      Console.WriteLine("Points:");
      string input = Console.ReadLine();
      if (input =0 "") 
      {
        break;
      }
      int score = Conver.ToInt32(input);

      if (score < 0 || score > 100) 
      {
        Console.WriteLine("Impossible number.");
        continue;
      }
      register.AddGradeBasedOnPoints(score);
    }
  }

  public void PrintGradeDistribution() 
  {
    int grade = 5;
    while (grade >= 0) 
    {
      int stars = register.NumberOfGrades(grade);
      Console.Write(grade + ": ");
      PrintStars(stars);
      Console.WriteLine();

      grade = grade - 1;
    }

  public static void PrintStars(int stars) 
  {
    while (stars > 0) 
    {
      Console.Write("*");
      stars--;
    }
  }
  }
}
```

# Tehtävät

<Exercise title={'001 Grade register'}>

Tehtäväpohjassa on materiaalista tuttu luokka `GradeRegister`. Tässä tehtävässä kehitämme ohjelmaa eteenpäin, jonka avulla voidaan laskea arvosanojen keskiarvoja ja tenttituloksia.

* Osa 1 - Arvosanojen keskiarvo

Luo metodi `public double AverageOfGrades()` luokkaan `GradeRegister`. Sen tulee palauttaa arvosanojen keskiarvo pyöristettynä kahden desimaalin tarkkuuteen. Jos rekisterissä ei ole arvosanoja, metodin tulee palauttaa `-1`. Käytä keskiarvon laskemiseen listaa `grades`. Esimerkki:


```cpp
GradeRegister register = new GradeRegister();
register.AddGradeBasedOnPoints(93);
register.AddGradeBasedOnPoints(91);
register.AddGradeBasedOnPoints(92);
register.AddGradeBasedOnPoints(88);

Console.WriteLine(register.AverageOfGrades());
```

```console
4.75
```

* Osa 2 - Pisteiden keskiarvo

Anna luokalle `GradeRegister` uusi oliomuuttuja: lista johon tallennetaan tenttipisteet aina kun metodia `AddGradeBasedOnPoints` kutsutaan. Tämän lisäyksen jälkeen luo metodi `public double AverageOfPoints()` joka laskee ja palauttaa tenttipisteiden keskiarvon pyöristettynä kahden desimaalin tarkkuuteen. Jos rekisterissä ei ole pisteitä, metodin tulee palauttaa `-1`. Esimerkki:

```cpp
GradeRegister register = new GradeRegister();
register.AddGradeBasedOnPoints(93);
register.AddGradeBasedOnPoints(91);
register.AddGradeBasedOnPoints(92);

Console.WriteLine(register.AverageOfPoints());
```

```console
92
```

* Osa 3 - Tulostaminen käyttöliittymässä

Viimeisenä osana, lisää yllä toteutetut metodit käyttöliittymään. Kun ohjelma tulostaa arvosanojen jakauman, se tulostaa myös pisteiden ja arvosanojen keskiarvot. 

```console
Points:
> 82 
Points:
> 83
Points:
> 96 
Points: 
> 51 
Points:
> 48 
Points:
> 56 
Points:
> 61 
Points:
>

5: * 
4: ** 
3: 
2: * 
1: ** 
0: * 
The average of points: 68.14
The average of grades: 2.43
```

</Exercise>

<Exercise title={'002 Joke manager'}>

<Note>
Tässä tehtävässä EI OLE TESTEJÄ. Sinun tulee itse päättää, milloin tehtävä on valmis. TARKASTAN TEHTÄVÄN, JOTEN ÄLÄ HUIJAA.
</Note>

<Note>Tästä saa tuplapisteet, eli 4 yhteensä (2 per osio).</Note>

Tehtäväpohjassa on seuraava ohjelma joka on kirjoitettu kokonaan Main-luokkaan.

```cpp
using System;
using System.Collections.Generic;

namespace Exercise002
{
  class Program
  {
    public static void Main(string[] args)
    {
      List<string> jokes = new List<string>();
      Console.WriteLine("What a joke!");

      while (true)
      {
        Console.WriteLine("Commands:");
        Console.WriteLine(" 1 - add a joke");
        Console.WriteLine(" 2 - draw a joke");
        Console.WriteLine(" 3 - list jokes");
        Console.WriteLine(" X - stop");

        string command = Console.ReadLine();

        if (command == "X")
        {
          break;
        }

        if (command == "1")
        {
          Console.WriteLine("Write the joke to be added:");
          string joke = Console.ReadLine();
          jokes.Add(joke);
        }
        else if (command == "2")
        {
          Console.WriteLine("Drawing a joke.");

          if (jokes.Count == 0)
          {
            Console.WriteLine("Jokes are in short supply.");
          }
          else
          {
            Random draw = new Random();
            int index = draw.Next(0, jokes.Count);
            Console.WriteLine(jokes[index]);
          }

        }
        else if (command == "3")
        {
          Console.WriteLine("Printing the jokes.");
          foreach (string joke in jokes)
          {
            Console.WriteLine(joke);
          }
        }
      }
    }
  }
}
```

Ohjelma on käytännössä vitsivarasto. Voit lisätä vitsejä, hakea satunnaisen vitsin, ja tallennetut vitsit voi tulostaa. Tässä tehtävässä ohjelma jaetaan osiin ohjelmoinnin periaatteiden mukaisesti.

* Osa 1 - Joke manager (vitsivarasto)

Luo luokka `JokeManager` ja siirrä vitsien hallintaan liittyvä toiminnallisuus siihen. Luokan tulee sisältää parametriton konstruktori, sekä seuraavat metodit:

* `public void AddJoke(string joke)` - lisää vitsi vitsivarastoon.
* `public string DrawJoke()` - valitse yksi satunnainen vitsi ja palauta se. Jos vitsejä ei ole, palauta merkkijono "Jokes are in short supply.".
* `public void PrintJokes()` - tulostaa kaikki vitsit jotka on talletettu vitsivarastoon.

Esimerkki luokan käytöstä:

```cpp
JokeManager manager = new JokeManager();
manager.AddJoke("What is red and smells of blue paint? - Red paint.");
manager.AddJoke("What is blue and smells of red paint? - Blue paint.");

Console.WriteLine("Drawing jokes:");
for (int i = 0; i < 5; i++)
{
  Console.WriteLine(manager.DrawJoke());
}

Console.WriteLine("");
Console.WriteLine("Printing jokes:");
manager.PrintJokes();
```

Alla on mahdollinen tuloste ohjelmasta. Huomaa että vitsejä ei välttämättä tulosteta samassa järjestyksessä kuin ne on lisätty.


```console
Drawing jokes: 
What is blue and smells of red paint? - Blue paint. 
What is red and smells of blue paint? - Red paint. 
What is blue and smells of red paint? - Blue paint. 
What is blue and smells of red paint? - Blue paint. 
What is blue and smells of red paint? - Blue paint.

Printing jokes: 
What is red and smells of blue paint? - Red paint. 
What is blue and smells of red paint? - Blue paint.
```

* Osa 2 - User Interface (käyttöliittymä)

Luo luokka `UserInterface` ja siirrä käyttöliittymän toiminnallisuus siihen. Luokan tulee sisältää konstruktori jossa on yksi parametri: `JokeManager`-luokan olio. Lisäksi luokalla tulee olla metodi `public void Start()` jota voidaan käyttää käyttöliittymän käynnistämiseen.

Käyttöliittymän tulee tarjota seuraavat komennot käyttäjälle:

* X - Lopetus: Poistuu metodista Start.
* 1 - Lisääminen: Kysyy käyttäjältä vitsin ja lisää sen vitsivarastoon. 
* 2 - Arpominen: Valitsee satunnaisen vitsin vitsivarastosta ja tulostaa sen. Jos vitsejä ei ole, tulostetaan merkkijono "Jokes are in short supply.".
* 3 - Tulostaminen: Tulostaa kaikki vitsit jotka on talletettu vitsivarastoon.

Esimerkki käyttöliittymän käytöstä:



```cpp
JokeManager manager = new JokeManager();
UserInterface ui = new UserInterface(manager);
ui.Start();
```

```console
Commands: 
 1 - add a joke 
 2 - draw a joke 
 3 - list jokes
  X - stop 
> 1 
Write the joke to be added:
> Did you hear about the claustrophobic astronaut? -- He just needed a little space. 
Commands:
 1 - add a joke
 2 - draw a joke
 3 - list jokes 
 X - stop 
> 3 
Printing the jokes. 
Did you hear about the claustrophobic astronaut? -- He just needed a little space. 
Commands:
 1 - add a joke
 2 - draw a joke
 3 - list jokes 
 X - stop 
> X
```

</Exercise>
