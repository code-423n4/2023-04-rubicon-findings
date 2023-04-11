[N-01] Order of Functions
Some external functions are between public, some public functions are between external, 
In the RubiconMarket.sol, BathHouseV2.sol file

According to Style Guide, ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.
Functions should be grouped according to their visibility and ordered:
1.	constructor
2.	receive function(if exists)
3. fallback function (if exists)
1.	external
2.	public
3.	internal
4.	private
