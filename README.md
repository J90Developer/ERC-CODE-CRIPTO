# ERC-CODE-CRIPTO
codigo de criptomoneda usando ERC-20 

✅ Ejemplo de token ERC-20: MiMoneda
Nombre: MiMoneda
Símbolo: MMC
Decimales: 18
Suministro inicial: 1,000,000 tokens

🧾 Código fuente en Solidity (archivo MiMoneda.sol)
solidity
Copiar
Editar
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract MiMoneda is IERC20 {
    string public constant name = "MiMoneda";
    string public constant symbol = "MMC";
    uint8 public constant decimals = 18;
    uint256 private _totalSupply;

    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowed;

    constructor(uint256 total) {
        _totalSupply = total;
        balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(amount <= balances[msg.sender], "Saldo insuficiente");
        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        allowed[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return allowed[owner][spender];
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        require(amount <= balances[sender], "Saldo insuficiente");
        require(amount <= allowed[sender][msg.sender], "No autorizado");

        balances[sender] -= amount;
        allowed[sender][msg.sender] -= amount;
        balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }
}
🚀 ¿Cómo desplegarlo?
Instala Remix IDE en tu navegador: https://remix.ethereum.org

Crea un nuevo archivo: MiMoneda.sol

Pega el código

Compila con Solidity ^0.8.20

Despliega desde la pestaña "Deploy & Run" con un valor como:

Copiar
Editar
1000000 * 10**18
(esto representa 1 millón de tokens con 18 decimales)

