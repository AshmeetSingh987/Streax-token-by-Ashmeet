# Streax-token-by-Ashmeet
This contract is launched on Goreli Test Net having address  0x2a6AF55577f18c6d85DdE304A10F6838822b4Ce5
if has functionalities like Minting of Token, Burn  , Transfer , Allowance , TransferFrom , Approve etc.  

For testing this contract we can using Hardhat project , I t provider a Bundle for Development and testing of Solidity based Smart Contracts . 
For Configuring Hardhat we can follow these Steps - 
  -> In a directory of your choice, run npm init -y
  -> Run npm install --save-dev hardhat
  -> Run npx hardhat and you will get the following UI on your terminal:
  Following we can set it up and compile using Command NPX Compile 
  
  We can test our contract by writing Unit Test for our Contract using mocha
  
  const { expect } = require("chai");
const hre = require("hardhat");

describe("Strmeet contract", function() {
  // global vars
  let Token;
  let oceanToken;
  let owner;
  let addr1;
  let addr2;
  

  beforeEach(async function () {
    // Get the ContractFactory and Signers here.
    Token = await ethers.getContractFactory("Strmeet");
    [owner, addr1, addr2] = await hre.ethers.getSigners();

    Strmeet = await Token.deploy(tokenCap, tokenBlockReward);
  });

  describe("Deployment", function () {
    it("Should set the right owner", async function () {
      expect(await oceanToken.owner()).to.equal(owner.address);
    });

    it("Should assign the total supply of tokens to the owner", async function () {
      const ownerBalance = await Strmeet.balanceOf(owner.address);
      expect(await Strmeet.totalSupply()).to.equal(ownerBalance);
    });

    it("Should set the max capped supply to the argument provided during deployment", async function () {
      const cap = await Strmeet.cap();
      expect(Number(hre.ethers.utils.formatEther(cap))).to.equal(tokenCap);
    });


  describe("Transactions", function () {
    it("Should transfer tokens between accounts", async function () {
      // Transfer 50 tokens from owner to addr1
      await Strmeet.transfer(addr1.address, 50);
      const addr1Balance = await Strmeet.balanceOf(addr1.address);
      expect(addr1Balance).to.equal(50);

      // Transfer 50 tokens from addr1 to addr2
      // We use .connect(signer) to send a transaction from another account
      awaitStrmeet.connect(addr1).transfer(addr2.address, 50);
      const addr2Balance = await Strmeet.balanceOf(addr2.address);
      expect(addr2Balance).to.equal(50);
    });

    it("Should fail if sender doesn't have enough tokens", async function () {
      const initialOwnerBalance = await oceanToken.balanceOf(owner.address);
      // Try to send 1 token from addr1 (0 tokens) to owner (1000000 tokens).
      // `require` will evaluate false and revert the transaction.
      await expect(
        Strmeet.connect(addr1).transfer(owner.address, 1)
      ).to.be.revertedWith("ERC20: transfer amount exceeds balance");

      // Owner balance shouldn't have changed.
      expect(await Strmeet.balanceOf(owner.address)).to.equal(
        initialOwnerBalance
      );
    });

    it("Should update balances after transfers", async function () {
      const initialOwnerBalance = await oceanToken.balanceOf(owner.address);

      // Transfer 100 tokens from owner to addr1.
      await oceanToken.transfer(addr1.address, 100);

      // Transfer another 50 tokens from owner to addr2.
      await Strmeet.transfer(addr2.address, 50);

      // Check balances.
      const finalOwnerBalance = await Strmeet.balanceOf(owner.address);
      expect(finalOwnerBalance).to.equal(initialOwnerBalance.sub(150));

      const addr1Balance = await Strmeet.balanceOf(addr1.address);
      expect(addr1Balance).to.equal(100);

      const addr2Balance = await Strmeet.balanceOf(addr2.address);
      expect(addr2Balance).to.equal(50);
    });
  });
  
});
