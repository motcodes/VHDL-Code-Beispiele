# LZK Verbesserung

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity lzk_verbesserung is
port (
	a_i       : in  std_logic_vector(3 downto 0);
	b_i       : in  std_logic_vector(3 downto 0);
	c_i       : in  std_logic_vector(3 downto 0);
	sel_i			: in  std_logic_vector(1 downto 0);
	load_i		: in  std_logic;
	res_n     : in  std_logic;
	clk_i     : in  std_logic;
	enable_i  : in  std_logic;
	ser_out   : out std_logic
  );
end entity;

architecture rtl of lzk_verbesserung is
	signal mux_out : std_logic_vector(3 downto 0);
	signal shiftreg : std_logic_vector(3 downto 0);
begin
	with sel_i select 
	mux_out <= 	a_i when "00",
							b_i when "01",
							c_i when "10",
							a_i when others;
							
  lzk_verbesserung: process(res_n, clk_i)
  begin
    if (res_n = '0') then
     shiftreg <= "0000";
    elsif (clk_i'event and clk_i = '1') then
      if (enable_i = '1') then
        if (load_i = '1') then
          shiftreg <= mux_out;
        else 
          shiftreg <= '0' & shiftreg(3 downto 1);
				end if;
      end if;
    end if;
    ser_out <= shiftreg(0);
  end process;
end rtl;
```