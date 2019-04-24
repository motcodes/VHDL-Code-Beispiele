# Registerbank

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity regbank is
port (
  res_n            : in  std_logic;
  clk_i            : in  std_logic;
  ena_i         	 : in  std_logic;
  in_i        	 	 : in  std_logic_vector(7 downto 0);
  out_o          	 : out std_logic_vector(7 downto 0)
  );
end regbank;


architecture rtl of regbank is
begin
regbank: process(res_n, clk_i)
  begin
    if (res_n = '0') then
     out_o <= (others => '0');
    elsif (clk_i'event and clk_i = '1') then
      if (ena_i = '1') then
        out_o <= in_i;
      end if;
    end if;
  end process;
end rtl;

configuration regbank_conf of regbank is 
  for rtl
  end for;
end regbank_conf;
```