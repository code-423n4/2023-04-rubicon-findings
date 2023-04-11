# [L-1] Use Ownable OpenZeppelin contract

## Comments
The Ownable contract in OpenZeppelin provides several advantages over creating an access control system from scratch, including:

**Security**: OpenZeppelin is a well-audited and tested library of smart contracts, which reduces the risk of vulnerabilities and errors in the code. The Ownable contract provides a secure way to manage access control in your smart contract.

**Time-saving**: The Ownable contract can save a lot of time and effort in writing and testing access control functionalities. Instead of writing your own code, you can use the pre-built contract provided by OpenZeppelin and focus on other aspects of your smart contract.

**Standardization**: OpenZeppelin provides a standard interface for access control, which makes it easier for other developers to understand and interact with your smart contract. By using the Ownable contract, you can benefit from the well-established conventions and best practices in the Ethereum community.

**Flexibility**: The Ownable contract can be customized to fit your specific needs. You can modify the contract to add or remove access control functionalities as per your requirements.

Overall, using the Ownable contract from OpenZeppelin can provide several benefits over creating an access control system from scratch, including security, time-saving, standardization, and flexibility.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L26
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L30

# [L-2] Change function visibility to external

## Comments

External functions are more restrictive, but they are cheaper to call since they don't need to update the contract's state. Public functions are more flexible but can be more expensive to call, especially when they modify the contract's state.

The function is not being called within the contract, so the external visibility flag can be used instead of public.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L45

# [L-3] Use OpenZeppelin ReentrancyGuard

## Comments

Using the OpenZeppelin ReentrancyGuard instead of creating the code from scratch can provide several benefits:

**Security**: The OpenZeppelin ReentrancyGuard has been extensively tested and reviewed by the community, making it a more secure implementation of a reentrancy guard than one that you create yourself. This can help reduce the risk of reentrancy attacks in your smart contract.

**Time-saving**: Using a pre-built implementation of a reentrancy guard can save you time, as you don't have to spend time writing, testing, and debugging your own implementation. This can help you focus on other parts of your smart contract that require more attention.

**Consistency**: By using a well-known and widely-used implementation of a reentrancy guard like OpenZeppelin's, you can ensure that your code is consistent with best practices and industry standards. This can make it easier for other developers to understand and work with your code.

**Future-proofing**: The OpenZeppelin ReentrancyGuard is designed to be upgradable and adaptable to changes in the Solidity language and the Ethereum ecosystem. This can help ensure that your smart contract remains secure and functional as the ecosystem evolves.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L264

# [L-4] Constants should be defined rather than using magic numbers

## Comments

Using magic numbers (hardcoded values) in the code makes it harder to understand and maintain. When a magic number is used, it is not immediately clear what the value represents or why it is being used. If the value needs to be changed in the future, you would have to search for all instances of the magic number in the code, which can be time-consuming and error-prone.

By using constants, you can avoid these issues and make your code more robust, readable, and maintainable. Additionally, Solidity provides built-in support for constants, which allows you to define them in a way that optimizes gas usage and makes them easier to work with in the context of smart contract development.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586

# [L-5] Duplicated requires should be refactored to modifiers

## Comments

By defining a modifier that encapsulates a set of common requirements, you can reduce the amount of code duplication and improve the readability of your smart contracts. This makes it easier for other developers to understand and maintain your code, reducing the likelihood of introducing errors or vulnerabilities.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L255
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L615
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L725

# [L-6] Function may run out of gas

# Comments

Once the number of ids grow to a large number, the array allocated will be large, and the number of iterations calling external functions on cancel will also be large, leading to the function running out of gas.

# Issues

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L910
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917

# [N-1] Event is missing indexed fields

## Comments

If you're creating an event function with more than two parameters, it's a good idea to index the ones that you'll be searching for later. This will help you filter through the logs and find the data you need more easily. Just remember that indexing too many fields can increase the gas cost, so be mindful of that as well.

## Instances

`event BathTokenCreated(address bathToken, address underlying);`
`event BuddySpawned(address bathToken, address bathBuddy);`
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L23
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L24

# [N-2] Inadequate NatSpec

## Comments

Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

## Instances

- Functions without description
- Wrong comments
- Missing parts of the comments

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L16
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L51
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L487
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L491

# [N-3] Use a fixed version number in Solidity

## Comments

It is generally recommended to use a fixed version number in Solidity rather than a version range that includes minor updates.

The reason for this is that minor updates to a Solidity version can introduce changes that affect the behavior of your smart contract. These changes may not necessarily be backward-compatible, which means that your smart contract could break or behave unexpectedly when run on a newer minor version.

By specifying a fixed version number, you ensure that your smart contract is only compiled and deployed with a specific version of Solidity, which reduces the risk of compatibility issues and unexpected behavior.

Additionally, using a fixed version number makes your code more explicit and easier to understand, as it removes any ambiguity around which version of Solidity is being used.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2

# [N-4] Use the latest Solidity version

## Comments

**Security**: Newer versions of Solidity often include security improvements and bug fixes that make the language more secure and less vulnerable to attacks. Using an older version may expose your smart contract to security vulnerabilities that have been addressed in newer versions.

**Compatibility**: Using the latest version of Solidity ensures that your smart contract is compatible with other contracts and tools in the Ethereum ecosystem that are also using the latest version. If you use an older version, you may run into compatibility issues with newer contracts and tools.

**Functionality**: Newer versions of Solidity often introduce new features and functionality that can make your smart contract more powerful and flexible. By using the latest version, you can take advantage of these new features and improve the functionality of your smart contract.

**Support**: Solidity is an open-source project that is actively maintained by a community of developers. By using the latest version, you can access the latest support and resources from the community, which can help you develop and debug your smart contract more effectively.

Overall, using the latest version of Solidity is a good practice to ensure the security, compatibility, functionality, and support of your smart contract. However, it is also important to carefully test and verify any changes that you make to your code to ensure that they work as intended.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2

# [N-5] Remove hardhat console import

## Comments
It is not necessary to leave the hardhat console import if it will not be used.
It helps to keep the code clean.

## Instances
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L5

# [N-6] Order of layout

## Comments

According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), ordering helps readers identify which variable they can use.

Variable should be grouped and ordered by:
- Type declarations
- State variables
- Events
- Modifiers
- Functions

You can move to the top the following instances.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L75
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L261

# [N-7] Remove commented out code 

## Comments

Leaving code commented out in Solidity is generally not considered to be a good practice for several reasons:

**Cluttered Code**: Commented code can clutter up your codebase and make it harder to read and maintain. Over time, the number of commented-out lines may increase, making it harder to distinguish between active and inactive code.

**Misleading Code**: Commented code may mislead other developers about the intended behavior of the contract. They may think that the commented-out code is still active and be confused about the actual behavior of the contract.

**Security Risks**: Leaving commented-out code in the contract can pose security risks. For example, if the commented-out code contains a vulnerability that was discovered during testing but never fixed, an attacker could potentially exploit it.

**Version Control**: Leaving commented-out code in the contract can make version control more complicated, especially if different versions of the code have different commented-out sections.

It is good practice to keep a copy of the commented-out code in a separate file or folder for future reference if needed.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L99
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L107
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L128
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L140
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L163
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L187
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L207
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L140
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L163
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L187
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L207
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L163
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L542

# [N-8] Require statements should have descriptive reason strings

## Comments

Adding a descriptive reason string to a require statement provides important information to developers and users about why a particular condition needs to be met. This can make debugging and maintaining code much easier, as it allows for quick identification of the exact point of failure when a transaction fails due to a require statement.

Some of the benefits of using descriptive reason strings with require statements in Solidity:

**Clarity**: When a developer reads a require statement with a clear and descriptive reason string, they can quickly understand what the code is checking for, and what needs to be true before the transaction can execute.

**Debugging**: If a transaction fails due to a require statement, the error message returned by the Ethereum Virtual Machine (EVM) will include the reason string provided in the require statement. This can help developers to quickly pinpoint the cause of the error and fix it.

**Security**: By providing a clear reason string in a require statement, developers can ensure that the condition being checked is understood and cannot be easily bypassed or exploited.

## Instances 
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L598
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L604
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L605
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L611
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L265
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L598
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L604
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L605
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L611
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L519
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L538

# [N-9] Split complex require statements into smaller, more specific requirements

## Comments

Splitting complex require statements into smaller, more specific requirements with descriptive reason strings is a best practice in Solidity development, as it helps to improve code readability, maintainability, and security.

## Instances
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612

# [N-10] Remove unused code 

## Comments
Removing unused code is considered a good practice for several reasons:

**Reducing Contract Size**: Removing unused code can help reduce the size of the contract, which can lead to lower deployment costs and faster transaction processing times.

**Security**: Unused code may contain vulnerabilities that could be exploited by attackers. By removing unused code, you reduce the attack surface of your contract and make it less vulnerable to potential attacks.

**Clarity**: Unused code can make your contract more difficult to read and understand, especially for other developers who may be reviewing or working with your code in the future. Removing unused code can improve the clarity and maintainability of your codebase.

**Optimization**: Removing unused code can also help optimize your contract's performance, as it reduces the amount of processing required by the EVM (Ethereum Virtual Machine) during contract execution.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260






