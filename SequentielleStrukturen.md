# Sequentielle Kontrollstrukturen

## if - then - else

Wird in einem **Process** verwendet und führt zu einem MUX Aufbau. Ähnlich mit der bedingten Struktur

```vhdl
if (sel_i='01') then
  y_o <= a_i;
elsif (sel_i='10') then
  y_o <= b_i;
else
  y_o <= c_i;
end if;
```

## case is

Funktioniert nur innerhalb eines **Process**. Ist ähnlich mit der selektiven Struktur in der nebenläufigen Umgebung. Wird verwendet, wenn alle Inputausgaben die ‘gleiche’ Signallaufzeit vom **input** zu **y_o** haben sollen. Case/Is ist die Grundstruktur von state Maschinen.

```vhdl
case state is
  when init_state =>
  	next_state <= state1
  	y_o <= a_i;
	when state1 =>
  	next_state <= state2
  	y_o <= b_i;
end case;
```

## Beispiel

**siehe StateMachine und Füllstandsmessung**

