pragma solidity >=0.4.22 <0.7.0;

interface tokenRecipient{
    function receiveApproval(address _from, uint256 _value, address _token, bytes calldata _extraData) external;
}

contract TokenERC20 {
//DATA
    //Name of our Cryptocurrency
    string public name;
    //Symbol of our Cryptocurrency
    string public symbol;
    //Initial Total Supply
    uint256 public totalSupply;
    //Length of decimal support
    uint8 public decimals;

//Events
event Transfer(address indexed from, address indexed to, uint256 value);
event Burn(address indexed from, uint256 value);

mapping(address => uint256 ) public balanceOf;
mapping(address => mapping(address => uint256)) public allowance;

//Implemnetation
constructor(string memory tokenName, string memory tokenSymbol, uint256 initialSupply, uint8 allowdecimals) public {
    name = tokenName;
    symbol = tokenSymbol;
    decimals = allowdecimals;
    totalSupply = initialSupply;
    balanceOf[msg.sender] = totalSupply;
    
}

//Transfer process**********************************************************************************************
function _transfer(address _from, address _to, uint256 _value) internal{
    //validations of from, to, & value

    require(_to != address(0x0));
    require(balanceOf[_from] >= _value);
    require(balanceOf[_to] + _value >= balanceOf[_to]);

    //prevent double spend
    uint previousBalances = balanceOf[_from] + balanceOf[_to];

    //update from balance
    balanceOf[_from] -= _value;

    //update to balance
    balanceOf[_to] += _value;
    emit Transfer(_from, _to, _value);
    assert(balanceOf[_from] + balanceOf[_to] == previousBalances);
}
function transfer(address _to, uint256 _value) public {
    _transfer(msg.sender, _to, _value);
}
function transferFrom(address _from, address _to, uint256 _value) public returns(bool success){
    require(_value <= allowance[_from][msg.sender]);
    allowance[_from][msg.sender] -= _value;
    _transfer(_from, _to, _value);
    return true;
}
//Transfer process**********************************************************************************************

//Approval******************************************************************************************************
function approve(address _spender, uint256 _value) public returns (bool success) {
    allowance[msg.sender][_spender] = _value;
    return true;
}
function approveAndCall(address _spender, uint256 _value, bytes memory _extraData) public returns (bool success){
    tokenRecipient spender = tokenRecipient(_spender);
    if(approve(_spender,_value))
        spender.receiveApproval(msg.sender, _value, address(this), _extraData);
    return true;
}

//Burning process**********************************************************************************************
function burn(uint256 _value) public returns(bool success){
    require(balanceOf[msg.sender] >= _value);
    balanceOf[msg.sender] -= _value;
    totalSupply -= _value;
    emit Burn(msg.sender, _value);
    return true;
}
function burnFrom(address _from, uint256 _value) public returns(bool success){
    require(_value <= allowance[_from][msg.sender]);
    require(balanceOf[_from] >= _value);
    balanceOf[_from] -= _value;
    allowance[_from][msg.sender] -= _value;
    totalSupply -= _value;
    emit Burn(_from, _value);
    return true;
}
//Burning process**********************************************************************************************


receive() external payable{
    revert();
}

}
