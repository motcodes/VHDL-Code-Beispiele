# DIC 5 Test

### BSP1

```vhdl
architecture rtlSel of LUT is
begin
  with sel_i is
  out_o <= "1001" when "00",
  				"1100" when "01",
  				"0010" when "10",
  				"0110" when "11",
  				"0000" when others;
end rtlSel;
  
architecture rtlBed of LUT is
begin
  out_o <= "1001" when (sel_i = "00") else
  				"1100" when (sel_i = "01") else
  				"0010" when (sel_i = "10") else
  				"0110" when (sel_i = "11") else
  				"0000";
end rtlBed
```

### BSP2

```vhdl
architecture rtl of LUT is
	signal clk_i, res_n, ena_i	: std_logic;
	signal ser_in_i						 : std_logic;
	signal par_out_o					 : std_logic_vector(3 downto 0);
begin
  shift_reg:process(res_n, clk_i, ena_i)
    begin
      if(res_n = '1')then
        par_out_o <= (others => '0');
      elsif(clk_i'event and clk_i = '1')then
        if(ena_i = '1')then
          par_out_o <= ser_in_i & par_out_o(2 downto 0)
        end if;
      end if
    end process
end rtl;
  
```

### BSP3

```vhdl
architecture beh of my_tb is
  signal clk_i					: std_logic;
	signal clk_period			: time := 1us;
	signal clk_signal			: std_logic;
	signal res_n					: std_logic := 1;
	signal ctrl1, ctrl2		: std_logic;
begin
  clkgen_p:process
    begin
      clk_signal <= '1';
    	wait for clk_period * 0.5;
    	clk_signal <= '0';
		  wait for clk_period * 0.5;
  end process;
	clk <= clk_signal;

  resgen:process
    begin
      wait for clk_period * 0.25;
      res_n <= '0';
    	wait for clk_period * 1.5;
    	res_n <= '1';
  end process;

  ctrl_p:process
    begin
      wait for clk_period * 2.25;
      ctrl2 <= '1';
    	
    	wait for clk_period * 1.75;
    	ctrl1 <= '1';

      wait for clk_period * 1.5;
      ctrl2 <= '0';

	    wait for clk_period * 2.75;
      ctrl1 <+ '1';
	end process;
end beh;
	
```











