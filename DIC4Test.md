# Dic 4 Test

### BSP1

```vhdl
entity mux2 is
	a_i, b_i, sel_i	:in std_logic;
	y_o						 :out std_logic
end entity;

architecture rtl of mux2 is
  begin
    y_o <= (a_i and sel_i) or (b_i and (not sel_i));
end rtl;
  
```

### BSP2

```vhdl
entity bu is
	a, b, c				:in std_logic;
	x    				  :out std_logic
end entity;

architecture rtl of bu is
  signal temp1, temp2, temp3 :std_logic;
  begin
    temp1 <= (not a) nor b;
    temp2 <= a nand (not c);
    temp3 <= b nand c;
	  x <= temp1 and temp2 and temp3;
end rtl;
```

