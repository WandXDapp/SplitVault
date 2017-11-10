pragma solidity ^0.4.18;

import "./Ownable.sol";

/**
 * @title Orders Vault
 * @dev Its a vault for storing traded order hashes. Kind of TimeMachine for Orders storage
 * @author Dinesh
 */
contract OrderVault is Ownable{
    //Storage Array for Order Hash
    bytes32[] internal orderHashes;
    
    // Vault opens/opened at this time
    uint256 public beginTime;
    
    // Vault closes/Closed at this times
    uint256 public endTime;
    
    // flag to track whether vault closed permanently or not
    bool public vaultSealed;
    
    // Checks whether the vault is opened or not
    bool public isVaultOpened;
    
    /**
     * @dev Constructor Sets the unsealed vault status
     */
    function OrderVault() public {
        vaultSealed = false;
        isVaultOpened = false;
    }
    
    /**
     * @dev function opens the vault at specific time
     * @param _startTime specifies when to open the vault
     * @param _closureTime specifies when to close the vault
     */
    function openVault(uint256 _startTime, uint256 _closureTime) public onlyOwner {
        require(!vaultSealed);
        require(!isVaultOpened);
        require(_startTime >= uint256(now)); 
        require(_closureTime >= uint256(now)); 
        require(_closureTime >= _startTime);    
        
        beginTime = _startTime; 
        endTime = _closureTime;
        isVaultOpened = true; 
    }
    
    /**
     * @dev function Extends the vault till the given time. Eventhough flag says its started,
     *      its a logical start only not a real start. The real start happens at begin time. 
     *      Extensions then possible when its really started
     * @param _closureTime specifies when to close the vault 
     */
    function extendVault(uint256 _closureTime) public onlyOwner {
        require(!vaultSealed);
        require(beginTime <= uint256(now)); 
        require(_closureTime >= uint256(now)); 
        require(_closureTime >= beginTime);  
        
        endTime = _closureTime; 
        isVaultOpened = true;
    }
    
      /**
     * @dev function closes the vault  
     */
    function closeVault() public onlyOwner {
        require(!vaultSealed);
        require(isVaultOpened); 
        
        endTime = uint256(now);
        isVaultOpened = false; 
    }
    
    /**
     * @dev function Storens the given hash in the Vault
     * @param _orderHash brings the hash to be stored 
     */
    function storeInVault(bytes32 _orderHash) public onlyOwner {
        require(!vaultSealed);
        require(isVaultOpened);
        require(beginTime <= uint256(now)); 
        require(endTime >= uint256(now)); 
        require(endTime >= beginTime);  
        
        orderHashes.push(_orderHash);
    }
    
    /** @dev function Close Vault does close the vault in future timestamp. and it can be reopen by owner again
     *       But SealVault does it Immediately and permanently. Once sealed cant be open again. 
     */
    function sealVault() public onlyOwner {
        require(!vaultSealed); 
        vaultSealed = true;
    }
    
    /**
     * @dev function gives the number of orders in the current vault 
     * @return gives the number of orders in the vault
     */
    function getNumberOfOders() public view returns (uint256) {
        return orderHashes.length;
    }
    
    /**
     * @dev function returns all the stored order hashes
     * @return An array of hashes
     */
    function getOrdersByIndex(uint256 index) public view returns (bytes32) { 
        require(index >= 0);
        require(index <= orderHashes.length);
        
        return orderHashes[index];
    }
}
