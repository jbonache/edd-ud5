# Casos pràctics

## Sistema de reserves esportives

Anem a realitzar un **sistema de reserves en línia per a un centre esportiu**, que ofereix serveis com la reserva de pistes, la gestió d’usuaris i la generació de factures. Per a això, realitzarem tres, els quals faran evolucionar els diferents diagrames UML utilitzats.

### **Objectiu general del projecte:**  
7.Exercicis
Desenvolupar una aplicació que permeta als usuaris reservar pistes esportives, gestionar les seues reserves i generar factures.


## **Sprint 1: Sistema bàsic de reserves**

**Objectiu del sprint:**  

Permetre als usuaris reservar una pista esportiva.

### **Requeriments identificats:**

- Els usuaris han de poder veure les pistes disponibles.
- Han de poder reservar una pista concreta.
- El sistema ha de guardar la reserva.

!!!question "Diagrames de casos d'ús"
    




### 🖼️ **Diagrama de casos d'ús (PlantUML)**  
```plantuml
@startuml
actor Usuari
actor Admin

usecase "Consultar pistes disponibles" as U1
usecase "Reservar pista" as U2
usecase "Gestionar pistes" as U3

Usuari --> U1
Usuari --> U2
Admin --> U3

U2 .> U1 : <<include>>
@enduml
```

---

### 🛠️ **Diagrama de classes inicial (PlantUML)**
```plantuml
@startuml
class Usuari {
    -nom: String
    -email: String
    +consultarPistes(): void
    +reservarPista(): void
}

class Reserva {
    -data: Date
    -hora: String
    -pista: String
    +confirmarReserva(): void
}

class Pista {
    -nom: String
    -tipus: String
    -disponible: boolean
    +consultarDisponibilitat(): boolean
}

Usuari --> Reserva : fa
Reserva --> Pista : reserva
@enduml
```

---

### 🔄 **Diagrama de seqüència (PlantUML)**  
```plantuml
@startuml
actor Usuari
participant Sistema
participant BaseDeDades

Usuari -> Sistema : Consultar pistes
Sistema -> BaseDeDades : Obtenir llista de pistes
BaseDeDades -->> Sistema : Retorna pistes
Sistema -->> Usuari : Mostra pistes disponibles

Usuari -> Sistema : Reservar pista
Sistema -> BaseDeDades : Guardar reserva
BaseDeDades -->> Sistema : Confirmació
Sistema -->> Usuari : Reserva confirmada
@enduml
```

---

## 📈 **Sprint 2: Afegim gestió d'usuaris**

**Objectiu del sprint:**  
Permetre que els administradors puguen gestionar els usuaris i diferenciar entre tipus d'usuari.

### 🔹 **Requeriments identificats:**
- Els administradors han de poder afegir i eliminar usuaris.  
- Cal distingir entre usuaris **normals** i **administradors**.  

---

### 🖼️ **Diagrama de casos d'ús actualitzat (PlantUML)**  
```plantuml
@startuml
actor Usuari
actor Admin

usecase "Consultar pistes disponibles" as U1
usecase "Reservar pista" as U2
usecase "Gestionar pistes" as U3
usecase "Gestionar usuaris" as U4

Usuari --> U1
Usuari --> U2
Admin --> U3
Admin --> U4

U2 .> U1 : <<include>>
@enduml
```

---

### 🛠️ **Diagrama de classes actualitzat (PlantUML)**
```plantuml
@startuml
abstract class Persona {
    -nom: String
    -email: String
    +consultarPistes(): void
}

class Usuari extends Persona {
    +reservarPista(): void
}

class Admin extends Persona {
    +gestionarUsuaris(): void
    +gestionarPistes(): void
}

class Reserva {
    -data: Date
    -hora: String
    -pista: String
    +confirmarReserva(): void
}

class Pista {
    -nom: String
    -tipus: String
    -disponible: boolean
    +consultarDisponibilitat(): boolean
}

class GestorUsuaris {
    +afegirUsuari(): void
    +eliminarUsuari(): void
}

Usuari --> Reserva : fa
Reserva --> Pista : reserva
Admin --> GestorUsuaris : gestiona
@enduml
```

---

### 🔄 **Diagrama de seqüència actualitzat (PlantUML)**  
```plantuml
@startuml
actor Admin
participant Sistema
participant BaseDeDades

Admin -> Sistema : Afegir usuari
Sistema -> BaseDeDades : Insereix nou usuari
BaseDeDades -->> Sistema : Confirmació
Sistema -->> Admin : Usuari afegit correctament

Admin -> Sistema : Eliminar usuari
Sistema -> BaseDeDades : Esborra usuari
BaseDeDades -->> Sistema : Confirmació
Sistema -->> Admin : Usuari eliminat
@enduml
```

---

## 🔍 **Sprint 3: Afegim facturació**

**Objectiu del sprint:**  
Implementar la generació i gestió de factures per a les reserves realitzades.

### 🔹 **Requeriments identificats:**
- El sistema ha de generar una factura després de fer una reserva.  
- Els administradors han de poder consultar les factures.  

---

### 🖼️ **Diagrama de casos d'ús final (PlantUML)**  
```plantuml
@startuml
actor Usuari
actor Admin

usecase "Consultar pistes disponibles" as U1
usecase "Reservar pista" as U2
usecase "Gestionar pistes" as U3
usecase "Gestionar usuaris" as U4
usecase "Generar factura" as U5
usecase "Consultar factures" as U6

Usuari --> U1
Usuari --> U2
Admin --> U3
Admin --> U4
Admin --> U6
U2 .> U1 : <<include>>
U2 .> U5 : <<include>>
@enduml
```

---

### 🛠️ **Diagrama de classes final (PlantUML)**
```plantuml
@startuml
abstract class Persona {
    -nom: String
    -email: String
    +consultarPistes(): void
}

class Usuari extends Persona {
    +reservarPista(): void
}

class Admin extends Persona {
    +gestionarUsuaris(): void
    +gestionarPistes(): void
    +consultarFactures(): void
}

class Reserva {
    -data: Date
    -hora: String
    -pista: String
    +confirmarReserva(): void
}

class Pista {
    -nom: String
    -tipus: String
    -disponible: boolean
    +consultarDisponibilitat(): boolean
}

class GestorUsuaris {
    +afegirUsuari(): void
    +eliminarUsuari(): void
}

class Factura {
    -id: int
    -data: Date
    -import: double
    +generarFactura(): void
}

Usuari --> Reserva : fa
Reserva --> Pista : reserva
Admin --> GestorUsuaris : gestiona
Reserva --> Factura : genera
Admin --> Factura : consulta
@enduml
```

---

### 🔄 **Diagrama de seqüència final (PlantUML)**  
```plantuml
@startuml
actor Usuari
participant Sistema
participant BaseDeDades
participant Factura

Usuari -> Sistema : Reservar pista
Sistema -> BaseDeDades : Guarda reserva
BaseDeDades -->> Sistema : Confirmació

Sistema -> Factura : Generar factura
Factura -> BaseDeDades : Guarda factura
BaseDeDades -->> Factura : Confirmació
Factura -->> Sistema : Factura generada

Sistema -->> Usuari : Reserva i factura confirmades
@enduml
```

---

## ✅ **Reflexió del procés**

Aquest cas pràctic mostra com:

1. **El disseny UML es pot desenvolupar iterativament.**  
2. Cada **sprint amplia i actualitza els diagrames** per incorporar les noves funcionalitats.  
3. **PlantUML permet representar fàcilment els canvis** amb diagrames de casos d'ús, classes i seqüències.  

Amb aquesta metodologia, **UML s'integra de manera natural amb SCRUM** i altres metodologies àgils, adaptant-se a les necessitats del projecte a cada iteració.