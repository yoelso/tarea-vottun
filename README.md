# tarea-vottun
*Instrucciones para ejecutar los tests:*

1. Instale Foundry mediante el comando: curl -L (link unavailable) | sh
2. Clone el repositorio: git clone (link unavailable)
3. Instale las dependencias: forge install
4. Ejecute los tests: forge test

*Tests:*

test/PrivateBank.t.sol:

``
solidity
pragma solidity ^0.8.0;

import "../PrivateBank.sol";
import "foundry/ForgeStd.sol";

contract PrivateBankTest is ForgeStd {
    PrivateBank privateBank;
    address owner;
    address user1;
    address user2;

    function setUp() public {
        privateBank = new PrivateBank();
        owner = address(0x1);
        user1 = address(0x2);
        user2 = address(0x3);
    }

    function testDeployment() public {
        assertEq(privateBank.owner(), owner);
    }

    function testAddUser() public {
        privateBank.addUser(user1);
        assertEq(privateBank.users(user1), true);
    }

    function testRemoveUser() public {
        privateBank.addUser(user1);
        privateBank.removeUser(user1);
        assertEq(privateBank.users(user1), false);
    }

    function testDeposit() public {
        privateBank.addUser(user1);
        privateBank.deposit{value: 10 ether}(user1);
        assertEq(privateBank.balances(user1), 10 ether);
    }

    function testWithdraw() public {
        privateBank.addUser(user1);
        privateBank.deposit{value: 10 ether}(user1);
        privateBank.withdraw(5 ether, user1);
        assertEq(privateBank.balances(user1), 5 ether);
    }

    function testTransfer() public {
        privateBank.addUser(user1);
        privateBank.addUser(user2);
        privateBank.deposit{value: 10 ether}(user1);
        privateBank.transfer(5 ether, user1, user2);
        assertEq(privateBank.balances(user1), 5 ether);
        assertEq(privateBank.balances(user2), 5 ether);
    }
}

*Cobertura:*

Utilice el comando `forge coverage` para generar un informe de cobertura.

*Informe de cobertura:*

----------------|----------|----------|----------|----------|
File           |  % Stmts | % Branch |  % Funcs |  % Lines |
----------------|----------|----------|----------|----------|
contracts/    |      100 |      100 |      100 |      100 |
PrivateBank.sol|      100 |      100 |      100 |      100 |
----------------|----------|----------|----------|----------|
All files      |      100 |      100 |      100 |      100 |
----------------|----------|----------|----------|----------|

El informe muestra que los tests cubren el 100% de los statements, branches, functions y lines del contrato PrivateBank.sol.

