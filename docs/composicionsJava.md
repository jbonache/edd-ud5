# Composicions en Java

Anem a veure com fariem l'exemple de la partida en Java. Com representaríeu que *una partida pot tindre més d'un jugador*? Anem a veure alguna forma de representar-ho amb estructures de dades que ens proporciona Java.

En primer lloc, definiriem la classe `Jugador`:

```java
public class Jugador {

  private String Nom;  
  
  public Jugador () { };
  
  public void setNom (String newVar) {
    Nom = newVar;
  }

  public String getNom () {
    return Nom;
  }
}
```

Com veiem, és una classe senzilla, on definim l'atribut *Nom* i els mètodes accessors necessaris. Anem a veure ara com fer la classe *Partida*:

```java

import java.Util.ArrayList;

public class Partida {

  private int idPartida;
  
  // Definim Jugadors com a una llista 
  // implementada com un vector (ArrayList)
  // d'objectes de tipus *Jugador*
  private ArrayList<Jugador> Jugadors;
  
  public Partida () {
      // En el constructor de la partida, creem
      // aquest ArrayList.
      this.Jugadors = new ArrayList<Jugador>();
  };
  
  public void setIdPartida (int newVar) {
    idPartida = newVar;
  }

  public int getIdPartida () {
    return idPartida;
  }

  public void setJugadors (String Nom) {
    // Per afegir un nou jugador donat el nom
    // el crearem i l'afegirem a l'ArrayList:

    Jugador nouJugador=new Jugador();
    nouJugador.setNom(Nom);
    // Per afegir-lo, utilitzem el mètode add
    // de l'arraylist
    this.Jugadors.add(nouJugador);
  }

  public Jugador getJugadors () {
    return this.Jugadors;
  }

  public void iniciarPartida()
  {
  }
  public void finalitzarPartida()
  {
  }

}

```

Analitzem un poc l'exemple. Hem introduït el tipus de dada `ArrayList`, definit a la llibreria `java.Utils`, i hem definit dins la classe `Partida` un `ArrayList` d'objectes de tipus `Jugador`:

```java
public class Partida {
...
  private ArrayList<Jugador> Jugadors;
  ...
```

La forma de declarar un `ArrayList` en general té la següent sintaxi:

```java
ArrayList<TipusBase> NomDelArrayList;
```

Recordem que un `ArrayList` és una llista, que internament s'implementa com un vector, però la qual ens ofereix mètodes com `add`, `remove`, `set` o `get` per crear, eliminar, modificar o consultar elements, sense preocupar-nos de la gestió dels índex interna del vector.

Quan definim un `ArrayList`, hem d'indicar el tipus de dades que aquest contindrà (el seu *TipusBase*). Aquest podrà ser enter, cadenes de caràcters... o altra classe, com és el nostre cas, que es tracta d'objectes de tipus `Jugador`.

Amb això, hem definit l'`ArrayList`, però aquest no té encara espai reservat en memòria per a ell. Per a això, haurem de crear-lo dins el constructor de la classe partida. És a dir, quan creem una partida, haurem de crear l'`ArrayList` que contindrà la llista (buïda de moment) de jugadors.

Per tal de fer això, al constructor, farem:

```java
  public Partida () {
      this.Jugadors = new ArrayList<Jugador>();
  };
```

Simplement, hem hagut de crear l'`ArrayList` de `Jugadors` i assignar-lo a `this.Jugadors`.

Ara ens queda per vore com afegim jugadors a la partida. Per a això, anem a veure el mètode `setJugadors`:

```java
  public void setJugadors (String Nom) {
    // Per afegir un nou jugador donat el nom
    // el crearem i l'afegirem a l'ArrayList:

    Jugador nouJugador=new Jugador();
    nouJugador.setNom(Nom);
    // Per afegir-lo, utilitzem el mètode add
    // de l'arraylist
    this.Jugadors.add(nouJugador);
  }
```

Com veiem, al mètode ens passen el nom del jugador, i és el propi mètode `setJugadors`, qui crea un nou objecte (`nouJugador`) de tipus `Jugador`, i l'afig a l'`ArrayList` mitjançant el mètode `add` que aquest ens proporciona.

**Fixeu-vos que quan destruïm la partida, l'ArrayList també es destruïrà, i amb ell, tots els jugadors que s'han creat (a través del recol·lector de fem).** Aquest és el comportament corresponent a una **composició**.

Si en aquest cas haverem volgut representar una **associació** en lloc de la composició, podriem haver definit el mètode `setJugador` de la següent forma:

```java
public void setJugadors (Jugador jugador) {
    this.Jugadors.add(jugador);
  }
```

Fixeu-vos que en aquest cas, en lloc de rebre com a argument el nom del jugador, rebem ja una instància de jugador, el que vol dir que aquest objecte s'ha creat fora, i el que rebem, realment és una referència a aquest. En aqest cas, quan s'elimine la `Partida`, s'eliminarà l'ArrayList de jugadors, junt amb les instàncies a aquest, però no s'eliminaran els objectes de tipus `Jugador`, ja que segueixen existint fora d'aquesta classe (estan referenciats en algun lloc, i el recol·lector de fem no els elimina).
