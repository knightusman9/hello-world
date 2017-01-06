library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;
use ieee.std_logic_unsigned.all;

entity encoder is
port( error:in std_logic;
		datain: in std_logic_vector (3 downto 0);
		clk:in std_logic;
		rst:in std_logic;
		
		dataout:out std_logic_vector(3 downto 0);
		codef:out std_logic_vector(6 downto 0));
		
		
end encoder;

		
architecture system of encoder is
type state_type is (A,B,C,DD,E);
signal state: state_type;
signal h: std_LOGIC_VECTOR(7 downto 1);
signal d: std_LOGIC_VECTOR (3 downto 0);
signal y: std_LOGIC_VECTOR (7 downto 0);
signal code: std_LOGIC_VECTOR (7 downto 1);
signal p1 : std_LOGIC;
signal p2: std_LOGIC;
signal p4 : std_LOGIC;
signal c1: std_LOGIC;
signal c2: std_LOGIC;
signal c4: std_LOGIC;


type arr is array (0 to 11) of integer;
constant sumarr: arr:= (2,7,4,3,2,5,2,6,1,4,3,1);



begin

process(clk,error,datain,state,rst,p1,p2,p4,h,d,y)

variable i: integer range 0 to 11;

begin

if rst='1' then
state<=A;
elsif rising_edge(clk) then
		
		case state is
		
		when A=>
		
		y(0)<='0';
		y(1)<='0';
		y(2)<='0';
		y(3)<='0';
		y(4)<='0';
		y(5)<='0';
		y(6)<='0';
		y(7)<='0';
		
		dataout(0)<='0';
		dataout(1)<='0';
		dataout(2)<='0';
		dataout(3)<='0';
		state<=B;
		
		when B=>
		
		d(0)<= datain(0);
		d(1)<= datain(1);
		d(2)<= datain(2);
		d(3)<= datain(3);
		state<=C;
		
		when C=>
		
		
		-- Parity bits are found here. 
		if 	d(0)='0' and d(1)='0' and d(3)='0' then
		p1<='0';
		elsif d(0)='0' and d(1)='0' and d(3)='1' then
		p1<='1';
		elsif d(0)='0' and d(1)='1' and d(3)='0' then
		p1<='1';
		elsif d(0)='0' and d(1)='1' and d(3)='1' then
		p1<='0';
		elsif d(0)='1' and d(1)='0' and d(3)='0' then
		p1<='1';
		elsif d(0)='1' and d(1)='0' and d(3)='1' then
		p1<='0';
		elsif d(0)='1' and d(1)='1' and d(3)='0' then
		p1<='0';
		elsif d(0)='1' and d(1)='1' and d(3)='1' then
		p1<='1';
		end if;
		
		
		if 	d(0)='0' and d(2)='0' and d(3)='0' then
		p2<='0';
		elsif d(0)='0' and d(2)='0' and d(3)='1' then
		p2<='1';
		elsif d(0)='0' and d(2)='1' and d(3)='0' then
		p2<='1';
		elsif d(0)='0' and d(2)='1' and d(3)='1' then
		p2<='0';
		elsif d(0)='1' and d(2)='0' and d(3)='0' then
		p2<='1';
		elsif d(0)='1' and d(2)='0' and d(3)='1' then
		p2<='0';
		elsif d(0)='1' and d(2)='1' and d(3)='0' then
		p2<='0';
		elsif d(0)='1' and d(2)='1' and d(3)='1' then
		p2<='1';
		end if;
		
		
		if 	d(1)='0' and d(2)='0' and d(3)='0' then
		p4<='0';
		elsif d(1)='0' and d(2)='0' and d(3)='1' then
		p4<='1';
		elsif d(1)='0' and d(2)='1' and d(3)='0' then
		p4<='1';
		elsif d(1)='0' and d(2)='1' and d(3)='1' then
		p4<='0';
		elsif d(1)='1' and d(2)='0' and d(3)='0' then
		p4<='1';
		elsif d(1)='1' and d(2)='0' and d(3)='1' then
		p4<='0';
		elsif d(1)='1' and d(2)='1' and d(3)='0' then
		p4<='0';
		elsif d(1)='1' and d(2)='1' and d(3)='1' then
		p4<='1';
		end if;
		
		
		
		h(1)<=p1; --The parity bits found out in the above process block
	   h(2)<=p2; -- are put in a register 'h'(declared as signal). 
		h(3)<=d(0);
		h(4)<=p4;
		h(5)<=d(1);
		h(6)<=d(2);
		h(7)<=d(3);
	
		if error='1' then
		
		state<=DD;
		
		elsif error='0' then
		
	
		state<=E;
		end if;
		
		
		when DD=>
		
		if error='1' then
		i:=i+1;
		state<= DD;
		elsif error='0' then
		
		state<=E;
		end if;
		
		
		
		when E=>
		
		
		h(1)<=p1; --The parity bits found out in the above process block
	   h(2)<=p2; -- are put in a register 'h'(declared as signal). 
		h(3)<=d(0);
		h(4)<=p4;
		h(5)<=d(1);
		h(6)<=d(2);
		h(7)<=d(3);
		
		
		
		
		
		if i = 0 then
		h(sumarr(i))<= h(sumarr(i));
		else 
		h(sumarr(i))<= not h(sumarr(i));
		end if;
		
		
		 -- Parity bits are found here. 
		if 	h(1)='0' and h(3)='0' and h(5)='0' and h(7)='0'  then
		c1<='0';
		elsif h(1)='0' and h(3)='0' and h(5)='0' and h(7)='1'  then
		c1<='1';
		elsif h(1)='0' and h(3)='0' and h(5)='1' and h(7)='0'  then
		c1<='1';
		elsif h(1)='0' and h(3)='0' and h(5)='1' and h(7)='1'  then
		c1<='0';
		elsif h(1)='0' and h(3)='1' and h(5)='0' and h(7)='0'  then
		c1<='1';
		elsif h(1)='0' and h(3)='1' and h(5)='0' and h(7)='1'  then
		c1<='0';
		elsif h(1)='0' and h(3)='1' and h(5)='1' and h(7)='0'  then
		c1<='0';
		elsif h(1)='0' and h(3)='1' and h(5)='1' and h(7)='1'  then
		c1<='1';
		elsif h(1)='1' and h(3)='0' and h(5)='0' and h(7)='0'  then
		c1<='1';
		elsif h(1)='1' and h(3)='0' and h(5)='0' and h(7)='1'  then
		c1<='0';
		elsif h(1)='1' and h(3)='0' and h(5)='1' and h(7)='0'  then
		c1<='0';
		elsif h(1)='1' and h(3)='0' and h(5)='1' and h(7)='1'  then
		c1<='1';
		elsif h(1)='1' and h(3)='1' and h(5)='0' and h(7)='0'  then
		c1<='0';
		elsif h(1)='1' and h(3)='1' and h(5)='0' and h(7)='1'  then
		c1<='1';
		elsif h(1)='1' and h(3)='1' and h(5)='1' and h(7)='0'  then
		c1<='1';
		elsif h(1)='1' and h(3)='1' and h(5)='1' and h(7)='1'  then
		c1<='0';
		end if;
		
		
		if 	h(2)='0' and h(3)='0' and h(6)='0' and h(7)='0'  then
		c2<='0';
		elsif h(2)='0' and h(3)='0' and h(6)='0' and h(7)='1'  then
		c2<='1';
		elsif h(2)='0' and h(3)='0' and h(6)='1' and h(7)='0'  then
		c2<='1';
		elsif h(2)='0' and h(3)='0' and h(6)='1' and h(7)='1'  then
		c2<='0';
		elsif h(2)='0' and h(3)='1' and h(6)='0' and h(7)='0'  then
		c2<='1';
		elsif h(2)='0' and h(3)='1' and h(6)='0' and h(7)='1'  then
		c2<='0';
		elsif h(2)='0' and h(3)='1' and h(6)='1' and h(7)='0'  then
		c2<='0';
		elsif h(2)='0' and h(3)='1' and h(6)='1' and h(7)='1'  then
		c2<='1';
		elsif h(2)='1' and h(3)='0' and h(6)='0' and h(7)='0'  then
		c2<='1';
		elsif h(2)='1' and h(3)='0' and h(6)='0' and h(7)='1'  then
		c2<='0';
		elsif h(2)='1' and h(3)='0' and h(6)='1' and h(7)='0'  then
		c2<='0';
		elsif h(2)='1' and h(3)='0' and h(6)='1' and h(7)='1'  then
		c2<='1';
		elsif h(2)='1' and h(3)='1' and h(6)='0' and h(7)='0'  then
		c2<='0';
		elsif h(2)='1' and h(3)='1' and h(6)='0' and h(7)='1'  then
		c2<='1';
		elsif h(2)='1' and h(3)='1' and h(6)='1' and h(7)='0'  then
		c2<='1';
		elsif h(2)='1' and h(3)='1' and h(6)='1' and h(7)='1'  then
		c2<='0';
		end if;
		
		if 	h(4)='0' and h(5)='0' and h(6)='0' and h(7)='0'  then
		c4<='0';
		elsif h(4)='0' and h(5)='0' and h(6)='0' and h(7)='1'  then
		c4<='1';
		elsif h(4)='0' and h(5)='0' and h(6)='1' and h(7)='0'  then
		c4<='1';
		elsif h(4)='0' and h(5)='0' and h(6)='1' and h(7)='1'  then
		c4<='0';
		elsif h(4)='0' and h(5)='1' and h(6)='0' and h(7)='0'  then
		c4<='1';
		elsif h(4)='0' and h(5)='1' and h(6)='0' and h(7)='1'  then
		c4<='0';
		elsif h(4)='0' and h(5)='1' and h(6)='1' and h(7)='0'  then
		c4<='0';
		elsif h(4)='0' and h(5)='1' and h(6)='1' and h(7)='1'  then
		c4<='1';
		elsif h(4)='1' and h(5)='0' and h(6)='0' and h(7)='0'  then
		c4<='1';
		elsif h(4)='1' and h(5)='0' and h(6)='0' and h(7)='1'  then
		c4<='0';
		elsif h(4)='1' and h(5)='0' and h(6)='1' and h(7)='0'  then
		c4<='0';
		elsif h(4)='1' and h(5)='0' and h(6)='1' and h(7)='1'  then
		c4<='1';
		elsif h(4)='1' and h(5)='1' and h(6)='0' and h(7)='0'  then
		c4<='0';
		elsif h(4)='1' and h(5)='1' and h(6)='0' and h(7)='1'  then
		c4<='1';
		elsif h(4)='1' and h(5)='1' and h(6)='1' and h(7)='0'  then
		c4<='1';
		elsif h(4)='1' and h(5)='1' and h(6)='1' and h(7)='1'  then
		c4<='0';
		end if;
		
	
		if 	c4='0' and c2='0' and c1='0' then
		y(0)<='1';
		elsif c4='0' and c2='0' and c1='1' then
		y(1)<='1';
		elsif c4='0' and c2='1' and c1='0' then
		y(2)<='1';
		elsif c4='0' and c2='1' and c1='1' then
		y(3)<='1';
		elsif c4='1' and c2='0' and c1='0' then
		y(4)<='1';
		elsif c4='1' and c2='0' and c1='1' then
		y(5)<='1';
		elsif c4='1' and c2='1' and c1='0' then
		y(6)<='1';
		elsif c4='1' and c2='1' and c1='1' then
		y(7)<='1';
		end if;
		
	
		
		if y(0)='1' then
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		elsif y(1)= '1' then
		
		code(1)<= not h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		elsif y(2)= '1' then
		
		code(1)<= h(1);
		code(2)<=  not h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		elsif y(3)= '1' then
		
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= not h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		elsif y(4)= '1' then
		
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= not h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		elsif y(5)= '1' then
		
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= not h(5);
		code(6)<= h(6);
		code(7)<= h(7);
		
		
		elsif y(6)= '1' then
		
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= not h(6);
		code(7)<= h(7);
		
		
		elsif y(7)= '1' then
		
		code(1)<= h(1);
		code(2)<= h(2);
		code(3)<= h(3);
		code(4)<= h(4);
		code(5)<= h(5);
		code(6)<= h(6);
		code(7)<= not h(7);
		
		end if;
				
		dataout(0)<= code(3);
		dataout(1)<= code(5);
		dataout(2)<= code(6);
		dataout(3)<= code(7);
		
		codef(0)<=code(1);
		codef(1)<=code(2);
		codef(2)<=code(3);
		codef(3)<=code(4);
		codef(4)<=code(5);
		codef(5)<=code(6);
		codef(6)<=code(7);
				
	end case;
	end if;	
end process;
end system;		
		

		

		
		
		
		
		
		
		
		
		
