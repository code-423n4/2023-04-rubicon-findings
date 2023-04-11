1.

initialize functions can be front-run

The initialize function that initializes important contract state can be called by anyone.

Occurences:

NoteERC20.initialize
Router.initialize
The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract. In the best case for the victim, they notice it and have to redeploy their contract costing gas.

Recommend using the constructor to initialize non-proxied contracts. For initializing proxy contracts, recommend deploying contracts using a factory contract that immediately calls initialize after deployment, or make sure to call it immediately after deployment and verify the transaction succeeded.

2.

_name = string.concat("bath", ERC20(_underlying).symbol());
_symbol = string.concat(_name, "v2"); // @audit symbol is weird after concateanation

Maybe reverse

3.

Feetype maybe 0

4. 

Fee maybe 0

5.

No need safeMath

6.

Asset and quote can be the same

7.

spawnBuddy function can be frontrunned