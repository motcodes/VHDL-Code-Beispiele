#  Shift-Register 1

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity shiftreg is
  port (
    res_n            : in  std_logic;
    clk_i            : in  std_logic;
    ena_i         	 : in  std_logic;
    ser_in_i         : in  std_logic;
    par_out_o        : out std_logic_vector(7 downto 0)
    );
end shiftreg;

architecture rtl of shiftreg is
	signal shiftreg : std_logic_vector(7 downto 0);
begin  
shift_p: process(res_n, clk_i)
  begin
    if (res_n = '0') then
     shiftreg <= (others => '0');
    elsif (clk_i'event and clk_i = '1') then
      if (ena_i = '1') then
        shiftreg <= ser_in_i & shiftreg(7 downto 1);
      end if;
    end if;
end process;
par_out_o <= shiftreg;
end rtl;

configuration shiftreg_conf of shiftreg is 
  for rtl
  end for;
end shiftreg_conf;
```