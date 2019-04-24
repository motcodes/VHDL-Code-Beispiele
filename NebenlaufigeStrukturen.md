# Nebenläufige Kontrollstrukturen

## Unbedingte Signalzuweisung

Entspricht der Implementierung einer Funktionsgleichung

```vhdl
y_o <= (a_i and (not sel_i)) or (b_i and sel_i);
```

Die Reihenfolge der Zuweisungen ist egal



## Bedingte Signalzuweisung

Erzeugt (meist) eine MUX-Funktion und entspricht der “if-then-else”-Anweisung im sequentiellen Umgebungen. Führt aber zu eine sehr komplexen Schaltungsaufbau.

```vhdl
y_o <= 	a_i when (sel_i='1') else
  			b_i when (sel_i='0') else
        b_i;
```



## Selektive Signalzuweisung

Führt meist zu einer MUX-Struktur. Auswahl aus einer Reihe gleichberechtigter Möglichkeiten.

```vhdl
with sel_i select
y_o <=	a_i when '1',
				b_i when others;
```



## Beispiel

**siehe Fulladder**

