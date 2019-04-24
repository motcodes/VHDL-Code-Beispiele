# MUX Variation

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity mux_var is
port (
  a_i				: in std_logic;
  b_i				: in std_logic;
  c_i				: in std_logic;
  sel_i			: in std_logic_vector(1 downto 0);
  y_o       : out std_logic
  );
end entity;

architecture rtl of mux_var is
begin
mux_var: process(a_i, b_i, c_i, sel_i)
  begin
    if(sel_i = "00")then
      y_o <= a_i;
    end if;
    if(sel_i = "01")then
      y_o <= b_i;
    end if;
    if(sel_i = "10")then
      y_o <= c_i;
    end if;
  end process;
end rtl;
```

