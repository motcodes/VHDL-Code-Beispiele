# Drehkreuz State Machine

### Code

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;

entity Drehkreuz is
    port (
      clk_i      : in  std_logic;
      res_n      : in  std_logic;
      enable_i   : in  std_logic;
      card_i		 : in  std_logic;
      turn_i		 : in  std_logic;
      a_o				 : out std_logic
      );
end entity;

architecture rtl of Drehkreuz is  

   type state_def is (closed, opened); 
	attribute enum_decoding: string;
	attribute enum_decoding of state_def: type is "001 011 100 010"; -- Gray code

	signal state, next_state : state_def;
    begin --rtl

    -- implementation of the state register
    state_reg : process (res_n, clk_i) 
    begin
    if (res_n = '0') then
        state <= closed;
    elsif (clk_i'event and clk_i = '1') then
      if enable_i = '1' then
        state <= next_state;
      end if;
    end if;
    end process;
	 
    -- implementation of the state_machine function
    state_func : process(state, card_i, turn_i)
    begin
      case state is
           when closed => 
            if(card_i='1') then
              next_state <= opened;
            else next_state <= closed;
            end if;
            a_o <= '0';

           when opened => 
            if(turn_i = '0') then
              next_state <= closed;
            else next_state <= opened;
            end if;
            a_o <= '1';

            when others => 
						next_state <= closed;
             a_o <= '0';
        end case;
    end process; -- state function
end rtl; -- architecture

configuration Drehkreuz_conf of Drehkreuz is 
  for rtl
  end for;
end Drehkreuz_conf;
```

