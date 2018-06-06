# LDB-NFT 
[![Build Status](https://img.shields.io/travis/lordlessio/airdrop.svg)](https://travis-ci.org/lordlessio/airdrop)
![license](https://img.shields.io/github/license/lordlessio/airdrop.svg)

### airdrop :candy:
![](https://olxvlcccu.qnssl.com/blog/ekkz4.jpg?imageView2/2/w/600)
 
### source code
```javascript
pragma solidity ^0.4.21;

import "../node_modules/openzeppelin-solidity/contracts/ownership/Ownable.sol";
import "../node_modules/openzeppelin-solidity/contracts/token/ERC20/ERC20.sol";


contract Airdrop is Ownable {
  /**
   * @dev daAirdrop to address
   * @param _tokenAddr address the erc20 token address
   * @param dests address[] addresses to airdrop
   * @param values uint256[] value(in ether) to airdrop
   */
  function doAirdrop(address _tokenAddr, address[] dests, uint256[] values) onlyOwner public
    returns (uint256) {
    uint256 i = 0;
    while (i < dests.length) {
      ERC20(_tokenAddr).transferFrom(msg.sender, dests[i], values[i] * (10 ** 18));
      i += 1;
    }
    return(i);
  }


```
### usage

```js
const SimpleToken = artifacts.require('SimpleToken');
const Airdrop = artifacts.require('Airdrop');
const BigNumber = web3.BigNumber;
require('chai')
  .use(require('chai-as-promised'))
  .use(require('chai-bignumber')(BigNumber))
  .should();

contract('Airdrop', function (accounts) {
  before(async function () {
    this.simpleToken = await SimpleToken.new({ from: accounts[0] });
    this.airdrop = await Airdrop.new({ from: accounts[0] });
    this.airdropAmount = 666
    this.mockdata = {
      addresses: accounts,
      tokenAmounts: Array(10).fill(this.airdropAmount.toString()),
    }
  });

  describe('Airdrop basic test', function () {
    it('should airdrop to addresses', async function () {
      await this.simpleToken.approve(this.airdrop.address, 1e27);
      await this.airdrop.doAirdrop(
        this.simpleToken.address,
        this.mockdata.addresses,
        this.mockdata.tokenAmounts
      );
      const balance = (await this.simpleToken.balanceOf(accounts[1])).toNumber();
      await balance.should.be.equal(parseInt(await web3.toWei(this.airdropAmount)));
    });
  });
})

```

```sh
# test

npm run test
```
