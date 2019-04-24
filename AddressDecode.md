# Address Decoder mit Testbench

### Code

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity addrDec is
  port (
    addr_i		 : in  std_logic_vector(7 downto 0);
    cs_o			 : out  std_logic_vector(2 downto 0);
    addr0_o		 : out  std_logic_vector(5 downto 0);
    addr1_o		 : out  std_logic_vector(5 downto 0);
    addr2_o		 : out  std_logic_vector(6 downto 0)
    );
end entity;
  
architecture rtl of addrDec is  
begin
	cs_o <= "001" when addr_i(7 downto 6) = "00" else
			  	"010" when addr_i(7 downto 6) = "01" else
			  	"100" when addr_i(7) = '1' else
			  	"000";
	addr0_o <= addr_i(5 downto 0);
	addr1_o <= addr_i(5 downto 0);
	addr2_o <= addr_i(6 downto 0);
end rtl;
  
architecture rtl2 of addrDex is
  begin
    with addr_i(7 downto 6) select
    	CS_O <=	"001" when "00",
    					"010" when "01",
    					"100" when '1',
    					"000" when others;
  addr0_o <= addr_i(5 downto 0);
  addr1_o <= addr_i(5 downto 0);
  addr2_o <= addr_i(6 downto 0);
end rtl2;

configuration addrDec_conf of addrDec is 
  for rtl
  end for;
end addrDec_conf;
```



### Testbench

```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;
USE ieee.numeric_std.ALL;

ENTITY addrDec_tb IS
END addrDec_tb;

ARCHITECTURE behavior OF addrDec_tb IS 
  COMPONENT addrDec
  PORT(
    addr_i			 : in std_logic_vector(7 downto 0);
    cs_o			 : out std_logic_vector(2 downto 0);
    addr0_o		 : out std_logic_vector(5 downto 0);
    addr1_o		 : out std_logic_vector(5 downto 0);
    addr2_o		 : out std_logic_vector(6 downto 0)
  );
  END COMPONENT;

  SIGNAL res_n    :std_logic := '1';
  SIGNAL clk      :std_logic;
  signal addr			:std_logic_vector(7 downto 0);
   
BEGIN
  uut: addrDec 
  PORT MAP(
	 addr_i => addr,
	 cs_o => open,
	 addr0_o => open,
	 addr1_o => open,
	 addr2_o => open
  );
    
  control_sig : PROCESS
  BEGIN
    wait for 500 us;
    addr <= "00111111";
    wait for 500 us;
    addr <= "01111111";
    wait for 500 us;
    addr <= "11111111";

    wait for 150 ms;
    assert false report "End of simulation" severity FAILURE;
   END PROCESS;
END behavior;
```

