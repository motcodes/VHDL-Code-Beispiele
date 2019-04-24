# Füllstandsmessung mit Testbench

### Code

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity fuellstandsmessung is 
  port(
    a_i         : in  std_logic;
    b_i         : in  std_logic;
    c_i         : in  std_logic;
    led_o       : out std_logic_vector(2 downto 0)
  );
end entity;

architecture rtl_unbedingt of fuellstandsmessung is
  begin
    led_o(0) <= (a_i and not(b_i and c_i)) or (a_i and b_i and c_i);
    led_o(1) <= (a_i and b_i) or (a_i and b_i and c_i);
    led_o(2) <= ((not a_i) and c_i) or ((not a_i) and b_i) or ((not b_i) and c_i);
  end rtl_unbedingt;

architecture rtl_bedingt of fuellstandsmessung is
  signal in_vec : std_logic_vector(2 downto 0);
  begin
    in_vec <= (c_i, b_i, a_i);
    led_o <= "000" when in_vec = "000" else
          	 "001" when in_vec = "001" else
          	 "011" when in_vec = "011" else
          	 "011" when in_vec = "111" else
          	 "100";
  end rtl_bedingt;
               
architecture rtl_select of fuellstandsmessung is
	signal in_vec : std_logic_vector(2 downto 0);
  begin
    in_vec <= (c_i, b_i, a_i);
    with in_vec select
    led_o <= 	"000" when "000",
            	"001" when "001",
            	"011" when "011",
            	"011" when "111",
            	"100" when others;
  end rtl_select;
      
architecture rtl_seq of fuellstandsmessung is
  begin
  fuellstandsmessung: process(a_i, b_i, c_i)
  begin 

    if ((a_i = '1' and b_i ='0' and c_i = '0') 
       or (a_i = '1' and b_i = '1' and c_i = '1')) then
      	led_o <= "001";
    elsif ((a_i = '1' and b_i = '1') 
       or (a_i = '1' and b_i = '1' and c_i = '1')) then
      	led_o <= "011";
    elsif((a_i = '0' and c_i = '1') 
       or (a_i = '0' and b_i = '1') 
       or (b_i = '0' and c_i = '1')) then
      	led_o <= "100";
    end if;
  end process;
end rtl_seq;

configuration fuellstand_conf of fuellstandsmessung is 
  for rtl_unbedingt
  end for;
end fuellstand_conf; 
```


### Testbench

```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;
USE ieee.numeric_std.ALL;

ENTITY fuellstandsmessung_tb IS
END fuellstandsmessung_tb;

ARCHITECTURE behavior OF fuellstandsmessung_tb IS 
	COMPONENT fuellstandsmessung
	PORT(
	  a_i         : in  std_logic;
		b_i         : in  std_logic;
		c_i         : in  std_logic;
		led_o       : out std_logic_vector(2 downto 0)
		);
	END COMPONENT;
	signal a, b, c		: std_logic;
	signal y 					: std_logic_vector;
	
BEGIN
	uut: fuellstandsmessung 
  PORT MAP(
		a_i => a,
		b_i => b,
		c_i => c,
		led_o => y
	);
		
   tb : PROCESS
   BEGIN
		wait for 500 us;
		a <= '0';
		b <= '0';
		c <= '0';
		
		wait for 10 us;
		a <= '1';
		
		wait for 1 ms;
		b <= '1';
		
		wait for 1 ms;
		c <= '1';
		
		wait for 1 ms;
		a <= '0';
		b <= '0';
		
		wait for 1 ms;
		a <= '1';
		b <= '0';
		c <= '1';
		
		wait for 100 us;
		assert false report "Simulation ended" severity FAILURE;
   END PROCESS;
END behavior;
```