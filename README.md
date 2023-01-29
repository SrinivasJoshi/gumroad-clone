
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

contract onShare{
    IERC20 private _USDCtoken;
    struct Asset{
        uint256 price;
        string link;
        address creator;
    }
    uint256 public totalAssets;
    mapping(uint256 => Asset) assetList;
    mapping(uint256 => mapping(address => bool)) private isUserOwned;

    event buyAssetEvent(uint256 _assetId,Asset _asset);
    event createAssetEvent(uint256 _assetId,Asset _asset);

    constructor(address _USDCtokenAddr){
        _USDCtoken = IERC20(_USDCtokenAddr);
    }

    function yourBalance() public view returns(uint256){
        return _USDCtoken.balanceOf(msg.sender);
    }

    function doesUserOwnAsset(uint256 _assetId) public view returns(bool){
        return isUserOwned[_assetId][msg.sender];
    }

    function buyAsset(uint256 _assetId) public payable{
        require(_assetId < totalAssets,"Asset ID does not exist");
        require(msg.value >= assetList[_assetId].price,"Not enough money to buy asset");
        isUserOwned[_assetId][msg.sender] = true;
        emit buyAssetEvent(_assetId,assetList[_assetId]);
    }

    function createAsset(string calldata _link,uint256 _price) public{
        Asset memory _asset = Asset({
            price : _price,
            link:_link,
            creator : msg.sender
        });
        assetList[totalAssets] = _asset;
        emit createAssetEvent(totalAssets,_asset);
        totalAssets+=1;
    }

    function getBoughtAssets() public view returns(Asset[] memory){

    }

    function getCreateAssests() public view returns(Asset[] memory){
        
    }


}
```
