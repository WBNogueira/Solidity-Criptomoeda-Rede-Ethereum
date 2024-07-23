# Criando a Sua Primeira Criptomoeda da Rede Ethereum

# DIOCoin Solidity Contract

Este projeto contém um contrato inteligente para criar e transferir criptomoedas utilizando Solidity. O contrato implementa a interface ERC20 padrão.

## Sumário

- [Introdução](#introdução)
- [Pré-requisitos](#pré-requisitos)
- [Instalação e Implantação](#instalação-e-implantação)
- [Uso](#uso)
- [Licença](#licença)

## Introdução

Este contrato inteligente, `DIOCoin`, implementa a interface ERC20, permitindo a criação, transferência e aprovação de tokens. A criptomoeda é chamada "DIO Coin" com o símbolo "DIO" e possui 18 casas decimais.

## Pré-requisitos

Antes de começar, certifique-se de ter as seguintes ferramentas instaladas:

- [MetaMask](https://metamask.io/)
- [Remix - Ethereum IDE](https://remix.ethereum.org/)

## Instalação e Implantação

1. **Instale e configure o MetaMask:**
   
   - Adicione a extensão MetaMask ao seu navegador e crie uma conta.
   - Conecte-se a uma rede de teste (por exemplo, Ropsten, Rinkeby) ou configure uma rede local usando [Ganache](https://www.trufflesuite.com/ganache).

2. **Acesse o Remix - Ethereum IDE:**
   
   - Abra o [Remix](https://remix.ethereum.org/) em seu navegador.

3. **Crie um novo arquivo Solidity:**
   
   - No Remix, crie um novo arquivo com extensão `.sol` (por exemplo, `DIOCoin.sol`).

4. **Copie e cole o código Solidity:**
   
   ```solidity
   // SPDX-License-Identifier: GPL-3.0
   
   pragma solidity ^0.8.0;
   
   interface IERC20 {
       function totalSupply() external view returns (uint256);
       function balanceOf(address account) external view returns (uint256);
       function allowance(address owner, address spender) external view returns (uint256);
       function transfer(address recipient, uint256 amount) external returns (bool);
       function approve(address spender, uint256 amount) external returns (bool);
       function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
       event Transfer(address indexed from, address indexed to, uint256 value);
       event Approval(address indexed owner, address indexed spender, uint256 value);
   }
   
   contract DIOCoin is IERC20 {
       string public constant name = "DIO Coin";
       string public constant symbol = "DIO";
       uint8 public constant decimals = 18;
   
       mapping (address => uint256) balances;
       mapping(address => mapping(address => uint256)) allowed;
       uint256 totalSupply_ = 10 ether;
   
       constructor() {
           balances[msg.sender] = totalSupply_;
       }
   
       function totalSupply() public override view returns (uint256) {
           return totalSupply_;
       }
   
       function balanceOf(address tokenOwner) public override view returns (uint256) {
           return balances[tokenOwner];
       }
   
       function transfer(address receiver, uint256 numTokens) public override returns (bool) {
           require(numTokens <= balances[msg.sender]);
           balances[msg.sender] -= numTokens;
           balances[receiver] += numTokens;
           emit Transfer(msg.sender, receiver, numTokens);
           return true;
       }
   
       function approve(address delegate, uint256 numTokens) public override returns (bool) {
           allowed[msg.sender][delegate] = numTokens;
           emit Approval(msg.sender, delegate, numTokens);
           return true;
       }
   
       function allowance(address owner, address delegate) public override view returns (uint256) {
           return allowed[owner][delegate];
       }
   
       function transferFrom(address owner, address buyer, uint256 numTokens) public override returns (bool) {
           require(numTokens <= balances[owner]);
           require(numTokens <= allowed[owner][msg.sender]);
   
           balances[owner] -= numTokens;
           allowed[owner][msg.sender] -= numTokens;
           balances[buyer] += numTokens;
           emit Transfer(owner, buyer, numTokens);
           return true;
       }
   }
   ```

5. **Compile o contrato:**
   
   - No Remix, vá para a aba "Solidity Compiler" e clique em "Compile DIOCoin.sol".

6. **Implante o contrato:**
   
   - Vá para a aba "Deploy & Run Transactions".
   - Configure o ambiente para "Injected Web3" para conectar o MetaMask.
   - Selecione a conta do MetaMask e clique em "Deploy".

## Uso

1. **Interaja com o contrato no Remix:**
   
   - Após a implantação, você verá as funções do contrato na seção "Deployed Contracts".
   - Você pode chamar as funções `totalSupply`, `balanceOf`, `transfer`, `approve`, `allowance`, e `transferFrom` diretamente pelo Remix.

2. **Verifique o saldo inicial do criador do contrato:**
   
   - Use a função `balanceOf` com o endereço do criador (geralmente a conta conectada ao MetaMask).

3. **Transfira tokens para outro endereço:**
   
   - Use a função `transfer` com o endereço do destinatário e a quantidade de tokens a serem transferidos.

4. **Aprove a transferência de tokens por um terceiro:**
   
   - Use a função `approve` com o endereço do terceiro e a quantidade de tokens a serem aprovados.

5. **Transfira tokens em nome de outro usuário:**
   
   - Use a função `transferFrom` com os endereços do remetente, destinatário e a quantidade de tokens a serem transferidos.



Este README.md fornece uma visão clara e concisa sobre como configurar, implantar e interagir com o contrato inteligente utilizando o Remix - Ethereum IDE.



# 
