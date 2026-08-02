// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";


contract AgriNovaToken is ERC20, ERC20Burnable, ERC20Pausable, Ownable {

    uint256 public maxSupply;


    constructor(
        uint256 initialSupply,
        uint256 maximumSupply
    )
        ERC20("AgriNova Token", "AGNV")
        Ownable(msg.sender)
    {
        maxSupply = maximumSupply * 10 ** decimals();

        uint256 supply = initialSupply * 10 ** decimals();

        require(
            supply <= maxSupply,
            "Initial supply exceeds max supply"
        );

        _mint(msg.sender, supply);
    }


    function mint(address to, uint256 amount)
        public
        onlyOwner
    {
        uint256 mintAmount = amount * 10 ** decimals();

        require(
            totalSupply() + mintAmount <= maxSupply,
            "Max supply exceeded"
        );

        _mint(to, mintAmount);
    }


    function pause()
        public
        onlyOwner
    {
        _pause();
    }


    function unpause()
        public
        onlyOwner
    {
        _unpause();
    }


    function _update(
        address from,
        address to,
        uint256 value
    )
        internal
        override(ERC20, ERC20Pausable)
    {
        super._update(from, to, value);
    }
}
