 
---

# **EXPERIMENT – 1**

## **1(A) Solidity program to import a Contract into another Contract**

**A.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract A
{
    function showA() external pure returns(string memory)
    {
        return "Hello from A";
    }
}
```

**B.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "./A.sol";
contract B
{
    A a1;
    constructor(address _a1)
    {
        a1 = A(_a1);
    }
    function showB() public view returns(string memory)
    {
        return a1.showA();
    }
}
```

---

## **1(B) Node JS program to demonstrate digital signature**

```javascript
const prompt=require("prompt-sync")()
const { generateKeyPairSync, createSign, createVerify } = require("crypto")

const { publicKey, privateKey } = generateKeyPairSync("rsa",{ modulusLength: 2048 })

let msg = prompt("Enter a message:")
const sign = createSign("SHA256")
sign.update(msg)
sign.end()
const signature = sign.sign(privateKey, "hex")
console.log("Digital Signature created for the message:"+msg)

msg = prompt("Enter message to verify:")
const verify = createVerify("SHA256")
verify.update(msg)
verify.end()
console.log("Signature Verified:", verify.verify(publicKey, signature, "hex"))
```

---

# **EXPERIMENT – 2**

## **2(A) Solidity program to reverse a digit**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract DigitReverse
{
    function getReverse(uint num) public pure returns (uint)
    {
        uint rev=0;
        uint rem=0;
        while(num!=0)
        {
            rem=num%10;
            rev=rev*10+rem;
            num/=10;
        }
        return rev;
    }
}
```

---

## **2(B) Transfer NEO funds between NEO accounts**

```
(No code – tool-based experiment)
```

---

# **EXPERIMENT – 3**

## **3(A) Solidity program to compute Student Grade Report using struct**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract GradeReport
{
    struct student
    {
        string name;
        uint rollno;
        uint s1;
        uint s2;
        uint s3;
    }
    student[] public stds;

    function setStudent(string memory n,uint r,uint m1,uint m2,uint m3) public
    {
        stds.push(student(n,r,m1,m2,m3));
    }

    function getAverage(uint index) private view returns (uint)
    {
        uint total=stds[index].s1+stds[index].s2+stds[index].s3;
        return total/stds.length;
    }

    function getReport(uint index) public view returns (string memory)
    {
        uint report=getAverage(index);
        if(report>=70) return "Distinction";
        else if(report>=60) return "First class";
        else if(report>=50) return "Second class";
        else return "Fail";
    }

    function getStudentCount() public view returns(uint)
    {
        return stds.length;
    }
}
```

---

## **3(B) Link Ganache in Rabby Wallet and perform Ether Transfer**

```
(No code – tool-based experiment)
```

---

# **EXPERIMENT – 4**

## **4(A) Solidity program to display owner address and a message**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Message
{
    string message;
    address owner = msg.sender;

    function setMessage(string memory _m) public
    {
        message = _m;
    }

    function getMessage() public view returns(string memory)
    {
        return message;
    }

    function getOwner() public view returns(address)
    {
        return owner;
    }
}
```

---

## **4(B) Link Ganache in Remix IDE and execute a smart contract**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Hello
{
    string message="Hello World";
    function get() public view returns (string memory)
    {
        return message;
    }
}
```

---

# **EXPERIMENT – 5**

## **5(A) Solidity program to write Self Ether Transfer using mapping**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract EthersMapping
{
    mapping(address => uint) public balances;

    function deposit() public payable
    {
        balances[msg.sender] += msg.value;
    }

    function getBalance() public view returns (uint)
    {
        return balances[msg.sender];
    }

    function withdraw(uint _amount) public
    {
        require(balances[msg.sender] >= _amount, "Insufficient funds");
        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);
    }
}
```

---

## **5(B) Transfer NEO funds between NEO accounts**

```
(No code – tool-based experiment)
```

---

# **EXPERIMENT – 6**

## **6(A) Solidity program to perform ether transfer**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract EtherTransfer
{
    address public sender;
    address public receiver;
    uint public amount;

    function sendEther(address payable _receiver) public payable
    {
        require(msg.value > 0);
        sender = msg.sender;
        receiver = _receiver;
        amount = msg.value;
        _receiver.transfer(msg.value);
    }
}
```

---

## **6(B) Node JS program to demonstrate encryption & decryption**

```javascript
const crypto = require("crypto"); 
const prompt=require("prompt-sync")() 
const algorithm = "aes-256-cbc"; 
const key = crypto.randomBytes(32); 
const iv = crypto.randomBytes(16); 

const encrypt = (text) => { 
  let cipher = crypto.createCipheriv(algorithm, key, iv); 
  let encrypted = cipher.update(text, "utf8", "hex"); 
  let encrypted2=encrypted+cipher.final("hex"); 
  return encrypted2; 
}; 

const decrypt = (encryptedText) => { 
  let decipher = crypto.createDecipheriv(algorithm, key, iv); 
  let decrypted = decipher.update(encryptedText, "hex", "utf8") 
  let decrypted2=decrypted+decipher.final("utf-8") 
  return decrypted2; 
}; 
 
let plaintext = prompt("Enter some text:"); 
let ciphertext = encrypt(plaintext); 
console.log("Given Plaintext:", plaintext); 
console.log("Generated Ciphertext:", ciphertext); 
 
 
console.log("Decrypted Text:", decrypt(ciphertext)); 
```

---

# **EXPERIMENT – 7**

## **7(A) Solidity program to check Gapful number**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Gapful
{
    function isGapful(uint256 num) private pure returns (bool)
    {
        if (num < 100) return false;
        uint256 lastDigit = num % 10;
        uint256 firstDigit = num;
        while (firstDigit >= 10)
        {
            firstDigit /= 10;
        }
        uint256 divisor = firstDigit * 10 + lastDigit;
        return num % divisor == 0;
    }

    function display(uint256 num) public pure returns (string memory)
    {
        if (isGapful(num)) return "Gapful";
        else return "Not Gapful";
    }
}
```

---

## **7(B) Link Ganache in Remix IDE and perform ether transfer using smart contrACT**

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0; 
contract EtherTransfer { 
    address public sender; 
    address public receiver; 
    uint public amount; 
    function sendEther(address payable _receiver) public payable { 
        require(msg.value > 0, "You must send some Ether"); 
 
 
sender = msg.sender;        
receiver = _receiver;      
amount = msg.value;        
_receiver.transfer(msg.value); 
} 
function getDetails() public view returns (address, address, uint) { 
return (sender, receiver, amount); 
} 
function getReceiverBalance() public view returns (uint) { 
return address(receiver).balance; 
} 
function getSenderBalance() public view returns (uint) { 
return address(sender).balance; 
} 
}
```

---

# **EXPERIMENT – 8**

## **8(A) Solidity program to demonstrate Inheritance**

**A.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract A
{
    function showA() public pure returns(string memory)
    {
        return "Hello from A contract";
    }
}
```

**B.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "./A.sol";
contract B is A
{
    function showB() public pure returns(string memory)
    {
        return "Hello from B Contract";
    }
}
```

---

## **8(B) Node JS program to display Ganache accounts and balances**

```javascript
const {Web3}=require("web3") 
const prompt=require("prompt-sync")() 
async function sendEther() 
{ 
    try { 
        const web3=new Web3("http://127.0.0.1:7545") 
        console.log("Connected to Ganache...") 
        const accounts=await web3.eth.getAccounts() 
        const sender=accounts[0] 
        const receiver=accounts[1] 
        console.log("Ganache Accounts are:\n===========") 
        for (let i = 0; i < accounts.length; i++) 
        { 
            console.log("Account-"+i+"="+accounts[i])             
        } 
        let a1=parseInt(prompt("Enter an account index to fetch balance:")) 
        if(a1<0||a1>accounts.length) 
            console.log("Invalid Index") 
        else 
        { 
            console.log("Account:"+a1+"="+accounts[a1]) 
            const sbal=await web3.eth.getBalance(accounts[a1]) 
            let ebal=web3.utils.fromWei(sbal,"ether") 
            console.log("=====\nBalance of Account-"+a1+"="+ebal+" Ethers") 
        } 
        
 
 
    } catch (error) 
    { 
        console.log("Error in Connection="+error)     
    } 
} 
sendEther()
```

---

# **EXPERIMENT – 9**

## **9(A) Solidity program to read array and find max & min**

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0; 
contract ArrayDemo 
{ 
    uint[] arr; 
    function readArray(uint _value) public 
    { 
        arr.push(_value); 
    } 
    function getLength() public view returns(uint) 
    { 
        return arr.length; 
    } 
    function get() public view returns(uint,uint,uint) 
    { 
        uint max=arr[0]; 
        uint min=arr[0]; 
        for(uint i=1;i<arr.length;i++) 
        { 
            if(arr[i]>max) 
            { 
                max=arr[i]; 
            } 
            if(arr[i]<min) 
            { 
                min=arr[i]; 
            } 
 
} 
return (max,min,max+min); 
} 
function display()public view returns(uint[] memory) 
{ 
return arr; 
} 
}
```

---

## **9(B)Node js program that connects to Ganache, display Ganache accounts and their balances**

```javascript
const {Web3}=require("web3") 
const prompt=require("prompt-sync")() 
async function sendEther() 
{ 
    try { 
        const web3=new Web3("http://127.0.0.1:7545") 
        console.log("Connected to Ganache...") 
        const accounts=await web3.eth.getAccounts() 
        const sender=accounts[0] 
        const receiver=accounts[1] 
        console.log("Ganache Accounts are:\n===========") 
        for (let i = 0; i < accounts.length; i++) 
        { 
            console.log("Account-"+i+"="+accounts[i])             
        } 
        let a1=parseInt(prompt("Enter an account index to fetch balance:")) 
        if(a1<0||a1>accounts.length) 
            console.log("Invalid Index") 
        else 
        { 
            console.log("Account:"+a1+"="+accounts[a1]) 
            const sbal=await web3.eth.getBalance(accounts[a1]) 
            let ebal=web3.utils.fromWei(sbal,"ether") 
            console.log("=====\nBalance of Account-"+a1+"="+ebal+" Ethers") 
        } 
        
 
 
    } catch (error) 
    { 
        console.log("Error in Connection="+error)     
    } 
} 
sendEther()
```

---

# **EXPERIMENT – 10**

## **10(A) Solidity program to demonstrate parameterized constructor**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract ConstructorDemo
{
    uint public x;
    uint public y;
    constructor(uint _x,uint _y)
    {
        x=_x;
        y=_y;
    }
    function get() public view returns(uint)
    {
        return x+y;
    }
}
```

---

## **10(B)Node js program that connects to Ganache and performs fund transfer among dynamic accounts. **

```javascript
const {Web3}=require("web3") 
const prompt=require("prompt-sync")() 
async function sendEther() 
{ 
    try { 
        const web3=new Web3("http://127.0.0.1:7545") 
        //await web3.eth.requestAccounts() 
        console.log("Connected to Ganache...") 
        const accounts=await web3.eth.getAccounts() 
        
        console.log("Ganache Accounts are:\n===========") 
        for (let i = 0; i < accounts.length; i++) 
        { 
            console.log("Account-"+i+"="+accounts[i])             
        } 
        let s=parseInt(prompt("Enter index of sender account:")) 
        let r=parseInt(prompt("Enter index of receiver account:")) 
        if(s<0||s>accounts.length && r<0||r>accounts.length) 
            console.log("Invalid Sender/Receiver Index entered!") 
        else 
        { 
            let sbal,rbal,sebal,rebal; 
            console.log("Before Transfer..Account Status:\n=========") 
            console.log("Sender Account:"+s+"="+accounts[s]) 
            console.log("Receiver Account:"+r+"="+accounts[r]) 
            sbal=await web3.eth.getBalance(accounts[s]) 
 
 
            rbal=await web3.eth.getBalance(accounts[r]) 
            sebal=web3.utils.fromWei(sbal,"ether") 
            rebal=web3.utils.fromWei(rbal,"ether") 
            console.log("=====\nBalance of Account-"+s+"="+sebal+" Ethers") 
            console.log("Balance of Account-"+r+"="+rebal+" Ethers") 
            let amt=parseInt(prompt("Enter amount to transfer:")) 
            const txn=await 
web3.eth.sendTransaction({from:accounts[s],to:accounts[r],value:web3.utils.toWe
i(amt,"ether")}) 
            console.log("Transaction success...Txn Hash="+txn.transactionHash) 
            console.log("=======\nAfter Transfer..Account Status:\n=========") 
            console.log("Sender Account:"+s+"="+accounts[s]) 
            console.log("Receiver Account:"+r+"="+accounts[r]) 
            sbal=await web3.eth.getBalance(accounts[s]) 
            rbal=await web3.eth.getBalance(accounts[r]) 
            sebal=web3.utils.fromWei(sbal,"ether") 
            rebal=web3.utils.fromWei(rbal,"ether") 
            console.log("=====\nBalance of Account-"+s+"="+sebal+" Ethers") 
            console.log("Balance of Account-"+r+"="+rebal+" Ethers") 
        } 
        
    } catch (error) 
    { 
        console.log("Error in Connection="+error)     
    } 
} 
sendEther()
```

---

# **EXPERIMENT – 11**

## **11(A) Bank DAPP using Metamask**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Bank
{
    uint public balance;
    address public owner;

    constructor()
    {
        balance = 10000;
        owner = msg.sender;
    }

    function withdraw(uint amt) public
    {
        balance -= amt;
    }

    function deposit(uint amt) public
    {
        balance += amt;
    }

    function getBalance() public view returns(uint)
    {
        return balance;
    }
}
```

---

## **11(B) OpenSSL encryption & decryption**

```bash
openssl enc -aes256 -salt -in plaintext.txt -out encrypted.enc
openssl enc -d -aes256 -in encrypted.enc -out decrypted.txt
```

---

# **EXPERIMENT – 12**

## **12(A) Bank DAPP using Ganache**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Bank
{
    uint public balance;
    address public owner;

    constructor()
    {
        balance = 10000;
        owner = msg.sender;
    }

    function withdraw(uint amt) public
    {
        balance -= amt;
    }

    function deposit(uint amt) public
    {
        balance += amt;
    }
}
```

---

## **12(B) Digital Signature using OpenSSL**

```bash
openssl dgst -sha256 -sign private_key.pem -out file.sig demo.txt
openssl dgst -sha256 -verify public_key.pem -signature file.sig demo.txt
```

---

# **EXPERIMENT – 13**

## **13(A) Solidity program to import contract**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract A
{
    function showA() external pure returns(string memory)
    {
        return "Hello from A";
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "./A.sol";
contract B
{
    A a;
    constructor(address _a)
    {
        a=A(_a);
    }
    function showB() public view returns(string memory)
    {
        return a.showA();
    }
}
```

---

## **13(B) OpenSSL public & private key generation**

```bash
openssl genpkey -algorithm RSA -out private_key.pem -aes256
openssl rsa -in private_key.pem -pubout -out public_key.pem
```

---

# **EXPERIMENT – 14**

## **14(A) Solidity program to display owner and message**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Message
{
    string message;
    address owner=msg.sender;
    function setMessage(string memory _m) public { message=_m; }
    function getMessage() public view returns(string memory){ return message; }
    function getOwner() public view returns(address){ return owner; }
}
```

---

## **14(B) Link Ganache in Remix IDE**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Hello
{
    string msg="Hello Ganache";
    function get() public view returns(string memory)
    {
        return msg;
    }
}
```

---

# **EXPERIMENT – 15**

## **15(A) Solidity program on Inheritance**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract A
{
    function showA() public pure returns(string memory)
    {
        return "Hello from A";
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "./A.sol";
contract B is A
{
    function showB() public pure returns(string memory)
    {
        return "Hello from B";
    }
}
```

---

## **15(B) Link Ganache in Remix IDE and perform ether transfer using smart contract**

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0; 
contract EtherTransfer { 
    address public sender; 
    address public receiver; 
    uint public amount; 
    function sendEther(address payable _receiver) public payable { 
        require(msg.value > 0, "You must send some Ether"); 
 
 
sender = msg.sender;        
receiver = _receiver;      
amount = msg.value;        
_receiver.transfer(msg.value); 
} 
function getDetails() public view returns (address, address, uint) { 
return (sender, receiver, amount); 
} 
function getReceiverBalance() public view returns (uint) { 
return address(receiver).balance; 
} 
function getSenderBalance() public view returns (uint) { 
return address(sender).balance; 
} 
}
```

---
