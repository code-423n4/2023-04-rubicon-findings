BATHHOUSEV2
1.	Use immutable variables instead of storage variables - Immutable variables are stored in the bytecode instead of in storage, which can save gas. The “admin” and “proxyAdmin” variables in the BathHouseV2 contract could be declared as immutable since they are never modified after initialization.

2.	Use “bytes32” instead of “string” - Using “bytes32” instead of “string” for small pieces of data can save gas. The “_bathify” function in the BathHouseV2 contract could use “bytes32” instead of “string” for the “_name” and “_symbol” variables. Just make sure the data won’t pass 32 characters.

V2 MIGRATOR CODE
1.	Use “safeApprove” instead of approve in V2 deposit
Using “safeApprove” from the SafeERC20 library instead of approve ensures that the approval is set to the minimum value required, which can save gas.

underlying.safeApprove(bathTokenV2, amountWithdrawn);

BATHBUDDY
1.	Use immutable variables instead of state variables wherever possible. Immutable variables are initialized at deployment time, and their values are hardcoded into the bytecode. Accessing them is generally cheaper than accessing state variables, which require a storage read operation. The following variables can be made immutable:
address immutable public owner;
address immutable public myBathTokenBuddy;
address immutable public bathHouse;

